

> এই ফাইল দেখে একা একাই প্রেজেন্ট করতে পারবে। প্রতিটা step-এ **কী করবে**, **কেন করবে**, **কী হবে** — সব আছে।

---

## প্রেজেন্টেশনের আগে যা যা চালু রাখবে

**Terminal 1** — FastAPI server চলবে **Terminal 2** — MySQL দেখার জন্য **Postman** — API call করার জন্য **Gmail** — email দেখার জন্য **MySQL Workbench** — database দেখার জন্য

---

## PHASE 1 — Project চালু করা

### Terminal-এ যা লিখবে

```bash
# Project folder-এ যাও
cd C:\Users\ahmed\Downloads\job_queue_sim

# Server চালু করো
python -m uvicorn api:app --reload
```

### কেন করছো?

FastAPI server চালু হবে। Worker Pool background-এ ready হবে। তুমি order দিলে এই server সেটা নেবে।

### কী হবে?

Terminal-এ এটা দেখবে:

```
Worker Pool তৈরি — 5টা Worker ready
[Worker-1[FREE], Worker-2[FREE], Worker-3[FREE], Worker-4[FREE], Worker-5[FREE]]
Worker Engine ready! Postman থেকে order দাও!
Uvicorn running on http://127.0.0.1:8000
```

### কাউকে বলবে:

> "Server চালু হয়েছে। ৫টা Worker ready অবস্থায় আছে। এখন কেউ order দিলে এই Workers background-এ সেটা process করবে।"

---

## PHASE 2 — MySQL দেখাও (শুরুতে খালি)

### Terminal 2-এ যা লিখবে

```bash
mysql -u root -p
```

```sql
USE job_queue_db;
SELECT id, type, status, created_at FROM jobs ORDER BY id DESC LIMIT 5;
```

### কেন করছো?

প্রেজেন্টেশনের শুরুতে দেখাবে যে database এখন খালি (বা আগের data আছে)। Order দেওয়ার পরে আবার এখানে আসবে — তখন নতুন row দেখাবে। এটা prove করে যে data সত্যিই MySQL-এ যাচ্ছে।

### কী হবে?

```
+-----+------------------+-----------+---------------------+
| id  | type             | status    | created_at          |
+-----+------------------+-----------+---------------------+
| 433 | ORDER_PROCESSING | COMPLETED | 2026-07-09 03:15:00 |
+-----+------------------+-----------+---------------------+
```

### কাউকে বলবে:

> "এটা আমার database। প্রতিটা job এখানে track হয়। কোন status-এ আছে, কখন শুরু হয়েছে, কখন শেষ হয়েছে — সব এখানে।"

---

## PHASE 3 — SYNC vs ASYNC Comparison (সবচেয়ে গুরুত্বপূর্ণ)

এটাই তোমার project-এর মূল proof। এখানে দেখাবে Queue ছাড়া কতটা slow, Queue দিয়ে কতটা fast।

---

### STEP A — আগে SYNC দেখাও (ধীর version)

#### Postman-এ যা করবে:

```
Method: POST
URL: http://127.0.0.1:8000/orders/sync
Body → raw → JSON:

{
    "user_id": 1,
    "item": "Laptop",
    "amount": 85000,
    "fail_payment": false,
    "fail_inventory": false
}
```

Send চাপো।

#### কেন করছো?

এটা "পুরনো পদ্ধতি" — Queue ছাড়া। সব কাজ request-এর ভেতরেই হয়। User কে সব শেষ না হওয়া পর্যন্ত অপেক্ষা করতে হয়।

#### কী হবে?

Postman-এ response আসতে **৩ সেকেন্ড** লাগবে।

Response:

```json
{
    "message": "অর্ডার সম্পন্ন হয়েছে (sync)",
    "response_time": "3009ms",
    "note": "এই পুরো সময়টা user অপেক্ষা করেছে। Async-এ মাত্র ~50ms লাগে।"
}
```

Postman-এ নিচে দেখাবে: **3.02 s**

#### কাউকে বলবে:

> "দেখো — ৩ সেকেন্ড লেগেছে। এই পুরো সময়টা user অপেক্ষা করেছে। মানে user ৩ সেকেন্ড ধরে loading দেখছে। ১০ জন একসাথে order দিলে ৩০ সেকেন্ড লাগবে। এটা scale করে না।"

---

### STEP B — এখন ASYNC দেখাও (তোমার system)

#### Postman-এ যা করবে:

```
Method: POST
URL: http://127.0.0.1:8000/orders
Body → raw → JSON:

{
    "user_id": 1,
    "item": "Laptop",
    "amount": 85000,
    "fail_payment": false,
    "fail_inventory": false
}
```

Send চাপো।

#### কেন করছো?

এটা তোমার system — Queue দিয়ে। Job MySQL-এ ঢুকে যায়, user সাথে সাথে response পায়। বাকি কাজ background-এ হয়।

#### কী হবে?

Postman-এ response আসবে **মাত্র ~100ms-এ।**

Response:

```json
{
    "message": "অর্ডার সফলভাবে নেওয়া হয়েছে!",
    "job_id": 434,
    "status": "PENDING",
    "response_time": "100ms",
    "note": "Worker background-এ process করছে।"
}
```

Terminal-এ দেখবে Worker কাজ শুরু করে দিয়েছে।

#### কাউকে বলবে:

> "দেখো — মাত্র 100ms। User সাথে সাথে response পেয়ে গেছে। কিন্তু কাজ শেষ হয়নি — Terminal দেখো। Worker background-এ এখন inventory check, payment, email করছে। User জানেও না — কিন্তু সব হচ্ছে।"

---

### STEP C — Gmail দেখাও

Terminal-এ দেখবে job complete হয়েছে। তখন Gmail খোলো — email এসে গেছে।

#### কাউকে বলবে:

> "দেখো — Gmail-এ email এসেছে। এটা আমার system automatically পাঠিয়েছে। user order দিয়েছে, কাজ background-এ হয়েছে, শেষে confirmation email গেছে। আমি manually কিছু করিনি।"

---

### STEP D — MySQL-এ verify করো

Terminal 2-এ:

```sql
SELECT id, status, created_at, started_at, completed_at
FROM jobs
WHERE id = 434;
```

#### কেন করছো?

Proof দেখাবে যে data সত্যিই MySQL-এ আছে। Timeline দেখাবে — কখন create হয়েছে, কখন শুরু হয়েছে, কখন শেষ।

#### কী দেখাবে:

```
created_at:  03:15:20  ← তুমি order দিলে
started_at:  03:15:21  ← Worker নিল (1 সেকেন্ড পরে)
completed_at: 03:15:23 ← শেষ হলো (2.5 সেকেন্ড কাজ)
```

#### কাউকে বলবে:

> "দেখো timeline। আমি 03:15:20 এ order দিয়েছি — সাথে সাথে response পেয়েছি। Worker 03:15:21 এ job নিয়েছে — background-এ। 03:15:23 এ সব শেষ। পুরো process টা আমার request থেকে আলাদা।"

---

## PHASE 4 — Failure Scenario দেখাও

এটা দেখাবে তোমার system কীভাবে error handle করে।

### STEP A — Payment Fail (Retry দেখাও)

#### Postman-এ যা করবে:

```
Method: POST
URL: http://127.0.0.1:8000/orders
Body → raw → JSON:

{
    "user_id": 2,
    "item": "GPU",
    "amount": 50000,
    "fail_payment": true,
    "fail_inventory": false
}
```

Send চাপো।

#### কেন করছো?

Payment gateway timeout simulate করছো। Real life-এ এটা হয় — network error, gateway down। System কীভাবে handle করে সেটা দেখাবে।

#### কী হবে?

Terminal-এ দেখবে:

```
[↺ RETRY]  job_435 → retry 1/3 | 2s পরে আবার চেষ্টা হবে
           error: Payment Gateway Timeout

[↺ RETRY]  job_435 → retry 2/3 | 4s পরে আবার চেষ্টা হবে

[↺ RETRY]  job_435 → retry 3/3 | 8s পরে আবার চেষ্টা হবে

[✗ DEAD]   job_435 → 3বার চেষ্টা করেও ব্যর্থ → DEAD
[NOTIFY]   User 2 → আপনার অর্ডার process হয়নি
```

৩০-৪০ সেকেন্ড পর Gmail-এ failure email আসবে।

#### কাউকে বলবে:

> "দেখো — payment fail হয়েছে। কিন্তু system সাথে সাথে give up করেনি। ৩ বার try করেছে — প্রতিবার বেশি সময় অপেক্ষা করে। ২ সেকেন্ড, তারপর ৪ সেকেন্ড, তারপর ৮ সেকেন্ড। এটাকে Exponential Backoff বলে। ৩ বার fail হওয়ার পর user-কে email গেছে। সব automatically হয়েছে।"

---

### STEP B — MySQL-এ DEAD job দেখাও

```sql
SELECT id, status, retry_count, error_message
FROM jobs
WHERE status = 'DEAD';
```

#### কী দেখাবে:

```
| id  | status | retry_count | error_message                    |
|-----|--------|-------------|----------------------------------|
| 435 | DEAD   | 3           | Payment Gateway Timeout: ...     |
```

#### কাউকে বলবে:

> "DEAD status মানে এই job আর retry হবে না। কতবার চেষ্টা হয়েছে — retry_count 3 দেখাচ্ছে। কী error হয়েছিল — সেটাও saved আছে। এই data দিয়ে পরে investigate করা যায়।"

---

### STEP C — Stock Out দেখাও (Permanent Error)

#### Postman-এ যা করবে:

```
Method: POST
URL: http://127.0.0.1:8000/orders
Body → raw → JSON:

{
    "user_id": 3,
    "item": "RTX 5090",
    "amount": 150000,
    "fail_payment": false,
    "fail_inventory": true
}
```

#### কেন করছো?

এটা Permanent Error — retry করার কোনো মানে নেই। Stock নেই মানে নেই — ১০ বার check করলেও stock আসবে না।

#### কী হবে?

Retry ছাড়াই সরাসরি DEAD হবে। Gmail-এ stock out email আসবে।

Terminal-এ:

```
[✗ DEAD]  job_436 → Permanent failure → DEAD
           কারণ: Stock শেষ: 'RTX 5090' available নেই
```

#### কাউকে বলবে:

> "এটা আলাদা ধরনের error। Payment fail → retry করা যায়, হয়তো gateway ঠিক হবে। Stock নেই → retry করে লাভ নেই, stock আসবে না। তাই system দুটো error আলাদাভাবে handle করে। TransientError vs PermanentError।"

---

## PHASE 5 — Design Patterns দেখাও

এখানে code দেখিয়ে explain করবে।

### Singleton Pattern — job_queue.py

VS Code-এ `patterns/job_queue.py` খোলো।

দেখাও:

```python
_instance = None

@classmethod
def get_instance(cls):
    if cls._instance is None:
        cls._instance = object.__new__(cls)
    return cls._instance
```

#### কাউকে বলবে:

> "এটা Singleton Pattern। `get_instance()` যতবারই call করো — একই object পাবে। Producer যে queue-তে job দেয়, Worker সেই একই queue থেকে নেয়। দুটো আলাদা queue হলে কাজ করতো না।"

---

### Object Pool Pattern — worker_pool.py

VS Code-এ `patterns/worker_pool.py` খোলো।

দেখাও:

```python
def checkout(self, job_id):
    with self._condition:
        while not self._available:
            self._condition.wait()
        worker = self._available.pop(0)
        worker.assign(job_id)
        self._busy.append(worker)
    return worker

def checkin(self, worker):
    with self._condition:
        worker.release()
        self._busy.remove(worker)
        self._available.append(worker)
        self._condition.notify_all()
```

#### কাউকে বলবে:

> "এটা Object Pool Pattern। ৫টা Worker সবসময় ready আছে। Job এলে checkout — Worker নাও। Job শেষে checkin — Worker ফিরিয়ে দাও। নতুন Thread বানানোর চেয়ে এটা অনেক efficient।"

---

### Proxy Pattern — job_executor_proxy.py

VS Code-এ `patterns/job_executor_proxy.py` খোলো।

দেখাও:

```python
def execute(self, worker_id):
    # Before — logging, timer
    logger.proxy(f"job_{job_id} শুরু")

    try:
        self._job.execute()  # ← actual কাজ এখানে

        # After — success
        db.mark_completed(job_id)

    except TransientError as e:
        self._handle_transient_error(...)  # retry

    except PermanentError as e:
        self._handle_permanent_error(...)  # dead
```

#### কাউকে বলবে:

> "এটা Proxy Pattern। Worker সরাসরি job execute করে না। Proxy মাঝখানে বসে। Proxy করে: logging, timing, retry, status update। Actual job class এর কিছুই জানার দরকার নেই। OrderProcessingJob শুধু জানে order process করতে।"

---

### Factory Method — job_factory.py

VS Code-এ `patterns/job_factory.py` খোলো।

দেখাও:

```python
_registry = {
    "ORDER_PROCESSING": OrderProcessingJob,
}

@classmethod
def create(cls, job_type, job_id, payload):
    job_class = cls._registry.get(job_type)
    return job_class(job_id, payload)
```

#### কাউকে বলবে:

> "এটা Factory Method Pattern। Worker জানে না কোন job type-এর কী class। Factory-কে বলে 'ORDER_PROCESSING দাও' — Factory বানিয়ে দেয়। নতুন job type যোগ করতে শুধু নতুন class বানাও। Worker-এর code ছোঁয়া লাগে না।"

---

## PHASE 6 — Stats দেখাও

#### Postman-এ যা করবে:

```
Method: GET
URL: http://127.0.0.1:8000/stats
```

#### কী দেখাবে:

```json
{
    "queue": {
        "COMPLETED": 5,
        "DEAD": 2,
        "PENDING": 0
    },
    "pool": {
        "total": 5,
        "available": 5,
        "busy": 0
    }
}
```

#### কাউকে বলবে:

> "এটা system-এর current state। কতটা job complete হয়েছে, কতটা fail। Worker Pool-এ কতজন available। Real production system-এ এই endpoint monitoring করা হয়।"

---

## PHASE 7 — MySQL-এ সব data দেখাও

```sql
-- সব jobs এর summary
SELECT status, COUNT(*) as total
FROM jobs
GROUP BY status;

-- Details
SELECT
    id,
    type,
    status,
    retry_count,
    error_message,
    TIMEDIFF(completed_at, created_at) as total_time
FROM jobs
ORDER BY id DESC
LIMIT 10;
```

#### কাউকে বলবে:

> "দেখো — সব কিছু tracked। কোন job কত সময় নিয়েছে। কোনটা fail হয়েছে, কেন হয়েছে। Real system-এ এই data দিয়ে analytics করা যায়।"

---

## সংক্ষেপে পুরো story টা

প্রেজেন্টেশনের শেষে এটা বলো:

> "আমি একটা backend system বানিয়েছি।
> 
> Problem ছিল — কেউ order দিলে সব কাজ একসাথে করতে গেলে ৩ সেকেন্ড লাগে। User ৩ সেকেন্ড অপেক্ষা করে।
> 
> Solution — Job Queue। Order আসলে শুধু MySQL-এ save করো। User-কে সাথে সাথে বলো 'হয়ে গেছে'। বাকি কাজ background-এ Worker করবে।
> 
> এতে response time ৩০০০ms থেকে ১০০ms হয়েছে। ৯৬% faster।
> 
> সাথে আছে: • Automatic retry — failure হলে নিজেই আবার চেষ্টা করে • Gmail notification — success বা failure দুটোতেই email যায় • Error tracking — সব MySQL-এ saved • ৪টা Design Pattern — Singleton, Factory, Object Pool, Proxy
> 
> এই system টা দিয়ে যেকোনো ধরনের background job handle করা যাবে — email, PDF, SMS, যেকোনো কিছু।"

---

## Quick Reference — সব URL একসাথে

```
Health check:
GET  http://127.0.0.1:8000/health

Normal order (Async):
POST http://127.0.0.1:8000/orders

Slow order (Sync — comparison):
POST http://127.0.0.1:8000/orders/sync

Job status check:
GET  http://127.0.0.1:8000/jobs/{job_id}

System stats:
GET  http://127.0.0.1:8000/stats

API documentation:
GET  http://127.0.0.1:8000/docs
```

---

## Quick Reference — MySQL queries

```sql
-- সব jobs
SELECT id, type, status, retry_count, created_at FROM jobs ORDER BY id DESC;

-- শুধু COMPLETED
SELECT id, status, completed_at FROM jobs WHERE status = 'COMPLETED';

-- শুধু DEAD (failed)
SELECT id, status, retry_count, error_message FROM jobs WHERE status = 'DEAD';

-- Timeline দেখো
SELECT id, created_at, started_at, completed_at,
       TIMEDIFF(completed_at, created_at) as duration
FROM jobs WHERE status = 'COMPLETED' ORDER BY id DESC;

-- Summary
SELECT status, COUNT(*) FROM jobs GROUP BY status;
```

---

## প্রেজেন্টেশনের ক্রম

```
1. Server চালু করো          → Terminal
2. MySQL দেখাও (শুরুতে)     → Workbench
3. SYNC order দাও           → Postman (3 সেকেন্ড দেখাও)
4. ASYNC order দাও          → Postman (100ms দেখাও)
5. Terminal-এ Worker দেখাও  → Background কাজ হচ্ছে
6. Gmail দেখাও              → Email এসেছে
7. MySQL verify করো         → Data সেভ আছে
8. Payment fail দেখাও       → Retry দেখাও
9. Stock out দেখাও          → Permanent error দেখাও
10. Stats endpoint           → Postman
11. Code দেখাও              → VS Code (4 patterns)
12. পুরো story বলো          → মুখে
```

---

_এই ফাইল দেখে step by step এগোলে কোনো কিছু miss হবে না।_