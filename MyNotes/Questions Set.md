Job Queue Simulation
Async Job Processing Engine — Design Patterns in Action
---
Before You Start
1. MySQL Setup
```sql
-- Log in to MySQL and run the schema:
mysql -u root -p < schema.sql
```
2. Configure Environment Variables
Copy `.env.example` to `.env` and fill in your real credentials:
```bash
cp .env.example .env
```
```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_mysql_password_here
DB_NAME=job_queue_db

GMAIL_ADDRESS=your_email@gmail.com
GMAIL_APP_PASSWORD=your_gmail_app_password_here
```
> `.env` is git-ignored — your credentials never get committed.
3. Install Dependencies
```bash
pip install -r requirements.txt
```
4. Run
```bash
python main.py
```
---
Project Structure
```
job_queue_simulation/
├── config.py                      ← Loads DB/Gmail credentials from .env
├── schema.sql                     ← MySQL table DDL
├── requirements.txt
├── .env.example                   ← Template for environment variables
├── main.py                        ← Entry point — runs everything together
│
├── utils/
│   ├── logger.py                  ← Colored terminal output
│   └── errors.py                  ← TransientError, PermanentError
│
├── db/
│   └── database.py                ← MySQL queries (insert, fetch, update)
│
├── models/
│   ├── job.py                     ← BaseJob, JobStatus, JobPriority
│   └── worker.py                  ← Worker class
│
├── patterns/
│   ├── job_queue.py               ← SINGLETON PATTERN
│   ├── job_factory.py             ← FACTORY METHOD PATTERN
│   ├── worker_pool.py             ← OBJECT POOL PATTERN
│   └── job_executor_proxy.py      ← PROXY PATTERN
│
├── jobs/
│   └── order_processing_job.py    ← Concrete Job Implementation
│
├── producer_simulator.py          ← Fake order generator
├── worker_simulator.py            ← Worker engine
└── reconciliation_script.py       ← Crash recovery
```
---
Design Patterns
Pattern	File	What it does
Singleton	`patterns/job_queue.py`	Ensures a single JobQueue instance across the whole system
Factory Method	`patterns/job_factory.py`	Creates the correct Job class from a job type
Object Pool	`patterns/worker_pool.py`	Fixed worker pool with checkout/checkin
Proxy Pattern	`patterns/job_executor_proxy.py`	Logging, retry, and status updates
---
Simulation Scenarios
Scenario	Payload Flag	Result
Happy Path	(no flag)	All steps succeed → COMPLETED
Transient Error	`"fail_payment": True`	Retry → Exponential Backoff → DEAD
Permanent Error	`"fail_inventory": True`	Immediately DEAD → Notification
Crash Recovery	Answer "y" in main.py prompt	Stuck in PROCESSING → Reconciliation → PENDING
---
Terminal Color Guide
Color	Component
🔵 Cyan	Producer
🟣 Magenta	Worker Pool
⚪ White	Proxy
🟢 Green	Job steps / Success
🟡 Yellow	Retry
🔴 Red	Dead / Recovery
🔵 Blue	Notification
---
Inspecting the `jobs` Table in MySQL
```sql
USE job_queue_db;

-- All jobs
SELECT id, type, status, priority, retry_count, error_message,
       created_at, started_at, completed_at
FROM jobs ORDER BY id;

-- Summary by status
SELECT status, COUNT(*) as total FROM jobs GROUP BY status;

-- DEAD jobs and their reasons
SELECT id, error_message, retry_count FROM jobs WHERE status = 'DEAD';
```