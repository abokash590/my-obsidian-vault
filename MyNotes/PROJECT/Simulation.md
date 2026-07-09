# Technical Design Document

## Asynchronous Job Queue Simulation — Updated

---

## ১. System Overview

এই সিমুলেশনে আমরা একটা **Order Placement System** মডেল করব।

ইউজার অর্ডার দেওয়ার সাথে সাথে রেসপন্স পাবে। ভারী কাজগুলো (Payment, Inventory, Notification) ব্যাকগ্রাউন্ডে Worker Pool দিয়ে handle হবে।

এই সিমুলেশনে ৪টি Design Pattern naturally ব্যবহার হবে:

|Pattern|কোথায় ব্যবহার|
|---|---|
|**Singleton**|JobQueue — পুরো সিস্টেমে একটাই queue instance|
|**Factory Method**|JobFactory — job type দেখে সঠিক Job object তৈরি|
|**Object Pool**|WorkerPool — fixed worker pool, checkout/checkin mechanism|
|**Proxy Pattern**|JobExecutorProxy — logging, timing, retry সব এখানে|

---

## ২. Architecture — তিনটি মূল স্তর

```
┌─────────────────────────────────────────────────────────────┐
│  LAYER 1 — PRODUCER (FastAPI / Simulator Script)            │
│                                                             │
│  Request আসে → Validate → DB-তে PENDING job insert →       │
│  Instant 200 OK response → connection close                 │
└──────────────────────────┬──────────────────────────────────┘
                           │  job insert
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  LAYER 2 — JOB QUEUE (PostgreSQL jobs table — Singleton)    │
│                                                             │
│  [PENDING] → [PROCESSING] → [COMPLETED / FAILED / DEAD]    │
│                                                             │
│  Priority queue: CRITICAL(0) > HIGH(1) > NORMAL(2) > LOW(3)│
└──────────────────────────┬──────────────────────────────────┘
                           │  poll & fetch
                           ▼
┌─────────────────────────────────────────────────────────────┐
│  LAYER 3 — WORKER POOL (Object Pool)                        │
│                                                             │
│  Worker-1 [BUSY] → JobExecutorProxy → OrderProcessingJob   │
│  Worker-2 [BUSY] → JobExecutorProxy → OrderProcessingJob   │
│  Worker-3 [FREE]                                            │
│  Worker-4 [FREE]                                            │
│  Worker-5 [FREE]                                            │
│                                                             │
│  Checkout → Execute → Checkin                               │
└─────────────────────────────────────────────────────────────┘
```

---

## ৩. Database Schema — jobs Table

```sql
CREATE TABLE jobs (
    id              SERIAL PRIMARY KEY,
    type            VARCHAR(50) NOT NULL,
    -- job এর ধরন: 'ORDER_PROCESSING', 'SEND_EMAIL' ইত্যাদি

    payload         JSONB NOT NULL,
    -- job-এর data: {"user_id": 1, "item": "Laptop", "amount": 1200}
    -- simulation-এ error flag-ও এখানে থাকবে:
    -- {"fail_payment": true} বা {"fail_inventory": true}

    status          VARCHAR(20) NOT NULL DEFAULT 'PENDING',
    -- PENDING | PROCESSING | COMPLETED | FAILED | DEAD

    priority        INT NOT NULL DEFAULT 2,
    -- 0=CRITICAL, 1=HIGH, 2=NORMAL, 3=LOW

    retry_count     INT NOT NULL DEFAULT 0,
    -- কতবার retry হয়েছে

    max_retries     INT NOT NULL DEFAULT 3,
    -- সর্বোচ্চ কতবার retry করবে

    error_message   TEXT,
    -- সর্বশেষ error কী ছিল

    created_at      TIMESTAMP DEFAULT NOW(),
    -- job কখন তৈরি হয়েছে

    started_at      TIMESTAMP,
    -- worker কখন job নিয়েছে (Scenario C recovery-র জন্য critical)

    completed_at    TIMESTAMP,
    -- job কখন শেষ হয়েছে

    next_retry_at   TIMESTAMP
    -- exponential backoff — কখন পরের retry হবে
);
```

---

## ৪. Design Pattern বিস্তারিত

---

### Pattern 1 — Singleton: JobQueue

**কেন দরকার?** Producer এবং Worker দুজনেই queue access করে। দুটো আলাদা queue instance থাকলে Producer একটায় job দেবে, Worker আরেকটা থেকে নেবে — কেউ কাউকে দেখবে না।

```
JobQueue.get_instance() → সবসময় একই object return করে
    Producer → get_instance() → job insert
    Worker   → get_instance() → job fetch
    একই instance, সব ঠিক
```

**Simulation-এ থাকবে:**

- `JobQueue` class with `_instance = None`
- `get_instance()` classmethod
- `threading.Lock` দিয়ে thread-safe instance creation

---

### Pattern 2 — Factory Method: JobFactory

**কেন দরকার?** আজকে `ORDER_PROCESSING` job আছে। কালকে `SEND_REPORT`, `RESIZE_IMAGE` যোগ হবে। প্রতিবার Worker-এর কোড বদলানো ঠিক না।

```
JobFactory.create("ORDER_PROCESSING", payload)
    → OrderProcessingJob(payload) return করে

JobFactory.create("SEND_EMAIL", payload)
    → SendEmailJob(payload) return করে

নতুন job type যোগ করতে = শুধু নতুন class বানাও
Worker-এর কোড ছুঁতে হবে না
```

**Simulation-এ থাকবে:**

- `BaseJob` abstract class — `execute()` method
- `OrderProcessingJob(BaseJob)` — inventory, payment, notification
- `JobFactory` — dictionary-based registry

---

### Pattern 3 — Object Pool: WorkerPool

**কেন দরকার?** প্রতিটা job-এর জন্য নতুন thread তৈরি করা expensive। ৫০টা job একসাথে এলে ৫০টা thread — memory ও CPU crash করবে।

```
Pool size: 5 workers (configurable)

Job এলে:
  pool.checkout() → available worker নাও
  worker দিয়ে job execute করো
  pool.checkin(worker) → worker ফেরত দাও

কোনো worker নেই?
  নতুন job wait করবে → worker available হলে নেবে
  এটাই controlled, predictable concurrency
```

**Simulation-এ থাকবে:**

- `Worker` class — একটা worker unit
- `WorkerPool` class — `available` এবং `busy` list
- `threading.Lock()` — race condition prevent করতে
- `checkout()` → worker নেওয়া
- `checkin()` → worker ফেরত দেওয়া

---

### Pattern 4 — Proxy: JobExecutorProxy

**কেন দরকার?** প্রতিটা Job class-এ যদি logging, timing, retry logic লিখি — প্রতিটা class-এ ৬০% একই boilerplate কোড হবে। `OrderProcessingJob` শুধু order process করুক। বাকি সব proxy সামলাবে।

```
Worker → JobExecutorProxy.execute(job)
              │
              ├── [Before] Log: "job_42 started"
              ├── [Before] DB update: PENDING → PROCESSING
              ├── [Before] Timer শুরু
              │
              ├── job.execute() ← actual কাজ এখানে
              │
              ├── [After - Success] Timer stop, log duration
              ├── [After - Success] DB update: PROCESSING → COMPLETED
              │
              └── [After - Error] retry_count চেক
                      retry_count < max_retries?
                          → Exponential backoff calculate
                          → DB update: PROCESSING → PENDING
                          → next_retry_at set করো
                      retry_count >= max_retries?
                          → DB update: PROCESSING → DEAD
                          → Notification পাঠাও
```

**Actual Job class জানে না:**

- কতবার retry হচ্ছে
- DB status কীভাবে আপডেট হচ্ছে
- Logging কীভাবে হচ্ছে

এটাই Proxy Pattern-এর সৌন্দর্য।

---

## ৫. Step-by-Step Simulation Flow

### Stage 1 — Synchronous (Producer Side)

```
Step 1: producer_simulator.py প্রতি ৩ সেকেন্ডে একটা fake order তৈরি করে

Step 2: Basic Validation
        user_id আছে? amount positive? payload structure ঠিক?

Step 3: JobFactory.create("ORDER_PROCESSING", payload)
        → OrderProcessingJob object তৈরি হয়

Step 4: JobQueue.get_instance().enqueue(job, priority=HIGH)
        → DB-তে INSERT, status = 'PENDING'

Step 5: Instant response → {"message": "Order placed!", "job_id": 45}
        → Connection close, user খুশি
```

### Stage 2 — Asynchronous (Worker Side)

```
Step 6: WorkerPool background-এ চলছে
        Available worker আছে? JobQueue-তে PENDING job আছে?

Step 7: DB থেকে job fetch:
        SELECT * FROM jobs
        WHERE status = 'PENDING'
        AND next_retry_at <= NOW()
        ORDER BY priority ASC, created_at ASC
        LIMIT 1
        FOR UPDATE SKIP LOCKED;
        ↑
        এই SKIP LOCKED মানে অন্য worker এই job পাবে না

Step 8: pool.checkout() → একটা available worker নাও

Step 9: JobExecutorProxy শুরু হয়
        → Log করে, timer শুরু করে
        → DB update: PENDING → PROCESSING
        → started_at = NOW()

Step 10: OrderProcessingJob.execute() চলে:
         Sub-step A: Inventory Check & Deduct (time.sleep(1) simulate)
         Sub-step B: Payment Processing       (time.sleep(1) simulate)
         Sub-step C: Email/SMS Notification   (time.sleep(1) simulate)

Step 11: সব ঠিকঠাক → Proxy বলে:
         → DB update: PROCESSING → COMPLETED
         → completed_at = NOW()
         → Log: "job_45 completed in 3.02s"

Step 12: pool.checkin(worker) → worker pool-এ ফেরত
         → পরের job নেওয়ার জন্য ready
```

---

## ৬. Failure Handling — তিনটি Scenario

---

### Scenario A — Transient Error (Payment Gateway Timeout)

**সমস্যা:** Network error, temporary API failure।

**Flow:**

```
Payment step-এ exception raise হলো

Proxy catch করলো:
  retry_count (1) < max_retries (3)?  → YES

  Exponential Backoff calculate:
    delay = 2 ^ retry_count = 2^1 = 2 seconds
    next_retry_at = NOW() + 2 seconds

  DB update:
    status = 'PENDING'
    retry_count = 1
    next_retry_at = [2 seconds later]
    error_message = "PaymentGatewayTimeoutError: connection timed out"

  Log: "[RETRY] job_45 will retry in 2s (attempt 1/3)"

--- ২ সেকেন্ড পরে ---

Worker আবার job তোলে → আবার try করে

Attempt 1 fail → wait 2s
Attempt 2 fail → wait 4s
Attempt 3 fail → wait 8s → তারপর Scenario B
```

---

### Scenario B — Permanent Error (Card Declined / Out of Stock)

**সমস্যা:** Business logic failure — retry করেও লাভ নেই।

**Flow:**

```
retry_count (3) >= max_retries (3)?  → YES

Proxy বলে এটা recoverable না:

  DB update:
    status = 'DEAD'
    error_message = "PaymentError: card declined"

  Compensating Transaction (Rollback):
    → Inventory যদি deduct হয়ে থাকে, add back করো
    → DB transaction rollback

  Out-of-band Notification:
    → [EMAIL] "User 1, your order #45 failed.
               Reason: Insufficient stock.
               No charge has been made."
    (Simulation-এ print করব)

  Log: "[DEAD] job_45 moved to dead letter after 3 attempts"
```

---

### Scenario C — Server Crash (Thread Dies Mid-Processing)

**সমস্যা:** job PROCESSING state-এ আছে, কিন্তু server crash করলো। `started_at` আপডেট হয়েছে কিন্তু `completed_at` কখনো আসেনি।

**Recovery — Reconciliation Script:**

```
Server restart হওয়ার পর reconciliation_script.py চলবে

SQL:
SELECT * FROM jobs
WHERE status = 'PROCESSING'
AND started_at < NOW() - INTERVAL '10 minutes';

এই jobs গুলো কেউ process করছে না — আটকে আছে।

Action:
  UPDATE jobs
  SET status = 'PENDING',
      started_at = NULL,
      error_message = 'Recovered after server crash'
  WHERE [above condition];

Log: "[RECOVERY] 2 stuck jobs requeued after crash recovery"

এরপর Worker স্বাভাবিকভাবে এই jobs আবার তুলবে।
```

---

## ৭. Simulation File Structure

```
job_queue_simulation/
│
├── models/
│   ├── job.py              ← BaseJob abstract class, JobStatus enum
│   └── worker.py           ← Worker class
│
├── patterns/
│   ├── job_queue.py        ← Singleton pattern
│   ├── job_factory.py      ← Factory Method pattern
│   ├── worker_pool.py      ← Object Pool pattern
│   └── job_executor_proxy.py ← Proxy pattern
│
├── jobs/
│   └── order_processing_job.py ← Concrete job: inventory, payment, notification
│
├── db/
│   └── database.py         ← PostgreSQL connection, queries
│
├── producer_simulator.py   ← প্রতি ৩ সেকেন্ডে fake order insert করে
├── worker_simulator.py     ← WorkerPool চালু করে, jobs process করে
├── reconciliation_script.py ← Crash recovery — stuck jobs requeue করে
│
├── schema.sql              ← jobs table DDL
└── README.md
```

---

## ৮. Simulation Error Injection Strategy

Simulation-এ real error না ঘটিয়ে আমরা payload-এ flag দিয়ে error simulate করব:

```python
# Normal order — happy path
payload = {
    "user_id": 1,
    "item": "Laptop",
    "amount": 1200
}

# Transient error simulate — payment timeout হবে
payload = {
    "user_id": 2,
    "item": "Phone",
    "amount": 800,
    "fail_payment": True,       # payment-এ exception raise হবে
    "fail_after_attempt": 2     # ২ বার try করার পর permanently fail
}

# Permanent error simulate — stock নেই
payload = {
    "user_id": 3,
    "item": "GPU",
    "amount": 3000,
    "fail_inventory": True      # ১ম চেষ্টাতেই permanent fail
}
```

Job class-এর `execute()` method payload-এর এই flag দেখে exception raise করবে।

---

## ৯. Exponential Backoff Formula

```
delay = base ^ attempt_number

base = 2 seconds হলে:
  Attempt 1 fail → 2¹ = 2  seconds অপেক্ষা
  Attempt 2 fail → 2² = 4  seconds অপেক্ষা
  Attempt 3 fail → 2³ = 8  seconds অপেক্ষা
  Attempt 4 fail → DEAD (max_retries পার)

Jitter (optional, advanced):
  delay = delay + random(0, 1)
  একই সময়ে অনেক job retry করলে DB-তে চাপ না পড়ে
  এই technique টার নাম: "Thundering Herd Prevention"
```

---

## ১০. Key Technical Decisions ও Trade-offs

|Decision|কেন এটা বেছে নিলাম|Alternative কী ছিল|
|---|---|---|
|Threading (not multiprocessing)|Jobs mostly I/O bound (sleep simulate করছি)|Multiprocessing — CPU-bound হলে ভালো, GIL bypass করে|
|PostgreSQL as Queue|Simple, persistent, `SKIP LOCKED` support আছে|Redis — faster, কিন্তু extra dependency|
|DB polling (not event-driven)|Simple to implement, understand করা সহজ|RabbitMQ/Kafka — production-grade কিন্তু complex|
|Fixed pool size (5)|Predictable resource usage|Dynamic scaling — complex, overkill for simulation|
|Exponential backoff|Prevents hammering failed services|Fixed delay — simpler কিন্তু less intelligent|

---

## ১১. Console Output — Simulation চলার সময় যা দেখবে

```
[PRODUCER] Order #45 placed → job inserted (status: PENDING)
[PRODUCER] Order #46 placed → job inserted (status: PENDING)

[POOL] Worker-1 checked out
[PROXY] job_45 started at 10:30:05 | type: ORDER_PROCESSING
[JOB]   job_45 → Inventory check... OK
[JOB]   job_45 → Payment processing... OK
[JOB]   job_45 → Notification sent... OK
[PROXY] job_45 completed in 3.02s → status: COMPLETED
[POOL] Worker-1 checked in

[POOL] Worker-2 checked out
[PROXY] job_46 started at 10:30:06 | type: ORDER_PROCESSING
[JOB]   job_46 → Inventory check... OK
[JOB]   job_46 → Payment processing... FAILED (gateway timeout)
[PROXY] job_46 retry 1/3 → next attempt in 2s
[POOL] Worker-2 checked in

--- 2 seconds later ---

[PROXY] job_46 retry attempt 2...
[JOB]   job_46 → Payment processing... FAILED again
[PROXY] job_46 retry 2/3 → next attempt in 4s

--- 4 seconds later ---

[PROXY] job_46 retry attempt 3...
[JOB]   job_46 → Payment processing... FAILED again
[PROXY] job_46 max retries reached → status: DEAD
[EMAIL] User 2 → "Your order failed after 3 attempts. No charge made."
```

---

_এই document-এর উপর ভিত্তি করে simulation কোড লেখা হবে।_ _প্রতিটা section কোডে directly map করবে।_