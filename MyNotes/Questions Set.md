# Design Document — Async Job Queue & Worker System

This document explains *why* each design pattern was chosen — the concrete
problem it solves in this system, the alternative that was rejected, and the
trade-off accepted.

---

## 1. Singleton — `patterns/job_queue.py`

**Problem it solves:**
Two independent processes touch the job queue — the Producer (`POST /orders`
inserts a job) and the Worker (`worker_simulator.py` pulls the next job).
If each created its own `JobQueue` object, the Worker's in-memory view and
the Producer's in-memory view could drift apart — for example, cached stats
or queue state computed by one instance would never be visible to the other.
Singleton guarantees every part of the app is talking to the *same* queue
object, so there is one source of truth for state like `get_stats()`.

**Alternative considered:** Pass a `JobQueue` instance around as a function
argument (dependency injection) instead of a global singleton.
**Why rejected:** With FastAPI request handlers, the simulator script, and
the reconciliation script all needing queue access independently, threading
one shared instance through every function signature added complexity for
no real benefit at this project's scale. A thread-safe Singleton (double-
checked locking, see `_lock`) gives the same guarantee with less ceremony.

**Trade-off accepted:** Singletons make unit testing harder (global mutable
state) and hide the dependency inside the class instead of the constructor.
Acceptable here because the "true" state of the world already lives in
MySQL — `JobQueue` is a thin, largely stateless wrapper around DB calls, not
where business data actually lives. That defuses the usual Singleton-testing
pain.

---

## 2. Factory Method — `patterns/job_factory.py`

**Problem it solves:**
The Worker receives a job's `type` string from the database (e.g.
`"ORDER_PROCESSING"`) but must run *different code* depending on that type.
Without a factory, the Worker would need an `if/elif` chain that grows every
time a new job type is added, e.g.:
```python
if job_type == "ORDER_PROCESSING": job = OrderProcessingJob(...)
elif job_type == "SEND_EMAIL": job = SendEmailJob(...)
elif job_type == "GENERATE_PDF": job = PDFJob(...)
```
That violates the Open/Closed Principle — the Worker's code changes every
time the system grows.

**Alternative considered:** A simple `dict` lookup inline in the Worker
(`JOB_CLASSES = {"ORDER_PROCESSING": OrderProcessingJob}`), no dedicated
class.
**Why rejected:** Functionally similar for one job type, but a proper
`JobFactory.register()` method makes the pattern explicit and gives a clear
extension point — the roadmap already lists `SEND_EMAIL`, `GENERATE_PDF`,
`RESIZE_IMAGE` as future job types. Isolating the mapping in its own class
also means the Worker imports zero concrete `Job` classes — it only knows
`BaseJob` (see `models/job.py`), so adding a job type never touches Worker
code, only `job_factory.py`'s registry.

**Trade-off accepted:** One extra layer of indirection for what is currently
a single job type — mild over-engineering today, but it's the standard way
to keep a job-processing system open to new job types without touching
dispatch logic, which is exactly the direction this project is headed
(SSLCommerz payment jobs, notification jobs, etc.).

---

## 3. Object Pool — `patterns/worker_pool.py`

**Problem it solves:**
Each order needs inventory check → payment → email, which in this
simulation takes ~2.5s of blocking work. Spawning a new OS thread per
incoming order (`threading.Thread(target=...)` per job) means: thread
creation overhead per job, ~8MB stack memory per thread, and no ceiling — a
burst of 100 orders would spin up 100 threads and could exhaust system
resources. There is also no back-pressure: nothing stops the system from
accepting unlimited concurrent work.

**Alternative considered:** `concurrent.futures.ThreadPoolExecutor` from the
standard library, which already implements pooling.
**Why rejected:** `ThreadPoolExecutor` pools *threads*, but this system
needed to pool **stateful Worker objects** (`Worker.worker_id`,
`Worker.is_busy`, `Worker.current_job_id`) that carry identity across their
lifetime — useful for logging ("Worker-3 picked up job_42") and for the
`/stats` endpoint to report which workers are busy vs free. A raw thread
pool has no concept of "which logical worker is doing what." Implementing
the Object Pool pattern directly (checkout/checkin with
`threading.Condition` for blocking when all workers are busy) gave that
identity for free.

**Trade-off accepted:** Hand-rolled pooling means hand-rolled correctness —
the checkout/checkin logic has to get its locking right (this is why
`checkout()` uses a `Condition`, not a plain `Lock`: a worker waiting for
availability needs to block *and* release the lock while waiting, then wake
up when `checkin()` calls `notify_all()`). A battle-tested library pool
would have been safer for production; here it's justified because
demonstrating the pattern *is* the point of the project.

---

## 4. Proxy Pattern — `patterns/job_executor_proxy.py`

**Problem it solves:**
Every job — regardless of type — needs the same surrounding behavior:
timing, logging, marking `COMPLETED`/`DEAD` in the DB, retry-count bookkeeping
with exponential backoff, and sending a failure notification email. If that
logic lived inside `OrderProcessingJob.execute()`, every future job type
(`SendEmailJob`, `PDFJob`, ...) would have to duplicate it, and the
job classes would be mixing two concerns: *what the job does* and *how the
system manages the job's lifecycle*.

**Alternative considered:** A decorator function (`@with_retry_and_logging`)
wrapping `execute()`.
**Why rejected:** A decorator works for stateless cross-cutting logic, but
here the wrapper needs to hold a reference to the job, query the DB for
`retry_count`/`max_retries` mid-execution, and conditionally call back into
`send_failure_email()` — that's closer to "control access to and manage the
lifecycle of a real object" (the textbook definition of Proxy) than a
one-shot function wrapper. Modeling it as `JobExecutorProxy(job)` matches
how the Worker actually uses it: it never calls `job.execute()` directly, it
always goes through the proxy, which decides *if and when* the real
`execute()` runs (and whether it retries).

**Trade-off accepted:** The `except TransientError` / `except PermanentError`
branching in `_handle_transient_error` / `_handle_permanent_error` is doing
more than a "pure" Proxy (which classically doesn't need business-specific
error branching). Accepted because this system's core value proposition —
transient errors retry with backoff, permanent errors fail immediately — has
to live somewhere, and the Proxy is the one place that already sees every
job's outcome uniformly.

---

## How the Four Patterns Work Together

```
POST /orders
     │
     ▼
JobQueue.enqueue()  ← Singleton: same queue instance for every request
     │  (inserts PENDING row in MySQL)
     ▼
Worker Simulator polls
     │
     ▼
JobQueue.dequeue()  ← Singleton again: consistent view of pending jobs
     │
     ▼
WorkerPool.checkout()  ← Object Pool: get a free Worker, block if none free
     │
     ▼
JobFactory.create(job_type, ...)  ← Factory Method: build the right Job class
     │
     ▼
JobExecutorProxy(job).execute()  ← Proxy: logging, timing, retry, DB status
     │
     ▼
WorkerPool.checkin()  ← Object Pool: return the Worker for the next job
```

Each pattern owns exactly one concern: **Singleton** owns *identity* (one
queue), **Factory Method** owns *construction* (which job class), **Object
Pool** owns *resource management* (bounded concurrency), and **Proxy** owns
*lifecycle control* (retry/logging/status). None of them overlap — that
separation is what made it possible to add real Gmail emails and retry
logic later without touching the queue or pool code at all.
