# 🧠 LLD — Online Coding Judge (Contest Compiler)

---

## 🧩 1. Core Classes

```text
User
- id: string
- name: string
- email: string
- passwordHash: string
- role: enum (USER, ORGANIZER, ADMIN)
- createdAt: timestamp

Problem
- id: string
- title: string
- slug: string
- difficulty: enum (EASY, MEDIUM, HARD)
- statement: text
- timeLimitMs: int
- memoryLimitMb: int
- createdBy: userId

Contest
- id: string
- title: string
- description: string
- startTime: timestamp
- endTime: timestamp
- visibility: enum (PUBLIC, PRIVATE)
- joinCode: string
- createdBy: userId

Submission
- id: string
- userId: string
- problemId: string
- contestId: string (nullable)
- language: string
- sourceCode: text
- status: enum (QUEUED, RUNNING, COMPLETED)
- verdict: enum (PENDING, ACCEPTED, WA, TLE, MLE, RE, CE)
- score: int
- runtimeMs: int
- memoryKb: int
- createdAt: timestamp

TestCase
- id: string
- problemId: string
- input: text
- expectedOutput: text
- isHidden: boolean

JudgeJob
- id: string
- submissionId: string
- status: enum (QUEUED, PROCESSING, DONE, FAILED)
- retryCount: int
- createdAt: timestamp
```

---

## 🔗 2. Relationships

* One **User → many Submissions**
* One **Problem → many TestCases**
* One **Contest → many Problems** (via `contest_problems`)
* One **Submission → one User + one Problem**
* One **Submission → one JudgeJob**

---

## 🧱 3. Important Modules

### 🔐 Auth Manager

* JWT validation
* Role-based access (RBAC)

### 📤 Submission Manager

* Validate input
* Create submission
* Push to queue

### ⚙️ Judge Manager

* Consume queue
* Assign job to worker
* Retry failed jobs

### 🧮 Leaderboard Manager

* Calculate rankings
* Handle tie-breaking (time penalty, attempts)

### 📦 Problem Manager

* CRUD operations
* Manage test cases

---

## 🔄 4. Sequence Flow — Run Code

```text
User
  → Client (Frontend)
    → Runs API
      → Execution Service (no DB persistence)
        → Sandbox (Docker)
          → Return Output
            → Client
```

---

## 🚀 5. Sequence Flow — Submit Code

```text
User
 → Submission API
   → Validate Request
   → Save Submission (status = QUEUED)
   → Push to Queue (Kafka/Redis)

Queue
 → Judge Worker
   → Fetch Submission
   → Compile Code
   → Run on Hidden Test Cases
   → Compute Verdict

 → Update DB (status = COMPLETED, verdict)
 → Leaderboard Manager updates ranking
```

---

## 🗄️ 6. Database Schema

### users

* id (PK)
* name
* email (UNIQUE)
* password_hash
* role
* created_at

---

### problems

* id (PK)
* title
* slug (UNIQUE)
* difficulty
* statement
* time_limit_ms
* memory_limit_mb
* created_by
* created_at

---

### contests

* id (PK)
* title
* description
* start_time
* end_time
* visibility
* join_code
* created_by

---

### contest_problems

* contest_id (FK)
* problem_id (FK)
* order_index

---

### submissions

* id (PK)
* user_id (FK)
* problem_id (FK)
* contest_id (FK, nullable)
* language
* source_code
* status
* verdict
* score
* runtime_ms
* memory_kb
* created_at

---

### test_cases

* id (PK)
* problem_id (FK)
* input_data
* expected_output
* is_hidden

---

### judge_jobs

* id (PK)
* submission_id (FK)
* status
* retry_count
* created_at

---

## ⚡ 7. Indexing Strategy

* `submissions(user_id, created_at DESC)`
* `submissions(problem_id, verdict)`
* `submissions(contest_id, score DESC)`
* `problems(slug)`
* `contest_problems(contest_id, problem_id)`
* `users(email)`

---

## 🔒 8. Concurrency & Consistency

* Use **optimistic locking** for leaderboard updates
* Prevent duplicate submissions using **Idempotency Key**
* Queue ensures **eventual consistency**
* Use **transactions** for:

  * submission creation
  * leaderboard updates

---

## 🧠 9. Design Patterns Used

### 🏭 Factory Pattern

* Create executors per language

```text
ExecutorFactory → CppExecutor / PythonExecutor / JavaExecutor
```

### 🎯 Strategy Pattern

* Different judging logic

```text
JudgeStrategy → StandardJudge / InteractiveJudge
```

### 🔁 Retry Pattern

* Retry failed judge jobs

### 🧵 Singleton Pattern

* DB connection
* Config manager

---

## 🐳 10. Code Execution Design

* Use **Docker containers**
* Resource limits:

  * CPU time
  * Memory
* Disable:

  * Network access
  * File system writes
* Kill process on limit exceed

---

## 📊 11. Leaderboard Logic

Ranking based on:

1. Total score (descending)
2. Penalty time (ascending)
3. Earliest submission time

---

## 🚦 12. Rate Limiting

* Runs API → 10/min/user
* Submissions → 5/min/user
* Login → brute-force protection

---

## 🔄 13. Idempotency

* Client sends:

```text
Idempotency-Key: unique-key
```

* Server:

  * Stores key → submissionId
  * Returns same response on retry

---

## 🧱 14. Scalability Notes

* Horizontal scaling for:

  * Judge Workers
  * API servers
* Use Redis for:

  * caching problems
  * leaderboard snapshots
* Use Kafka for:

  * submission queue

---

## 📌 15. Future Improvements

* Plagiarism detection
* AI-based code review
* Multi-language support expansion
* Contest analytics dashboard

---

## ✅ Summary

* Asynchronous judging system
* Scalable queue-based architecture
* Secure sandbox execution
* Real-time leaderboard updates

---
