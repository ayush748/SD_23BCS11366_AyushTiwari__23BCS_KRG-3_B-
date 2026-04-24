# 🚀 API Design — Online Coding Judge (Contest Compiler)

---

## 🔐 1. Authentication

### POST `/api/v1/auth/register`

Register a new user

```json
{
  "name": "Ayush",
  "email": "ayush@gmail.com",
  "password": "123456"
}
```

### POST `/api/v1/auth/login`

```json
{
  "email": "ayush@gmail.com",
  "password": "123456"
}
```

### GET `/api/v1/auth/me`

Get current logged-in user

---

## 👤 2. Users

### GET `/api/v1/users/{userId}`

Get user profile

### GET `/api/v1/users/{userId}/submissions`

Get user submission history

### GET `/api/v1/users/{userId}/contests`

Get contests joined by user

---

## 📚 3. Problems

### GET `/api/v1/problems`

Query params:

```
?difficulty=easy&tag=dp&page=1
```

### GET `/api/v1/problems/{problemId}`

Get problem details

### POST `/api/v1/problems` (Admin/Organizer)

Create new problem

### PATCH `/api/v1/problems/{problemId}`

Update problem

### DELETE `/api/v1/problems/{problemId}` (Admin)

Delete problem

---

## 🏆 4. Contests

### POST `/api/v1/contests`

Create contest

```json
{
  "title": "CodeSprint",
  "description": "Weekly contest",
  "startTime": 1725000000000,
  "duration": 90,
  "visibility": "private",
  "joinCode": "ABC123"
}
```

### GET `/api/v1/contests`

Filters:

```
?status=upcoming|running|ended
```

### GET `/api/v1/contests/{contestId}`

Get contest details

### POST `/api/v1/contests/{contestId}/join`

```json
{
  "joinCode": "ABC123"
}
```

### POST `/api/v1/contests/{contestId}/start` (Organizer)

### POST `/api/v1/contests/{contestId}/end` (Organizer)

### GET `/api/v1/contests/{contestId}/leaderboard`

Get leaderboard

---

## 🧩 5. Contest Problems

### GET `/api/v1/contests/{contestId}/problems`

Get all problems in a contest

### POST `/api/v1/contests/{contestId}/problems` (Organizer)

Attach problem to contest

```json
{
  "problemId": "p101",
  "order": 1
}
```

---

## ⚡ 6. Runs (Custom Code Execution)

### POST `/api/v1/runs`

Run code with custom input

```json
{
  "problemId": "p101",
  "language": "cpp",
  "code": "...",
  "stdin": "1 2"
}
```

### Response

```json
{
  "output": "3",
  "status": "SUCCESS",
  "time": "0.01s",
  "memory": "10MB"
}
```

---

## 📤 7. Submissions (Judge System)

### POST `/api/v1/submissions`

Submit code for evaluation

```json
{
  "problemId": "p101",
  "contestId": "c202",
  "language": "cpp",
  "code": "...",
  "idempotencyKey": "unique-key-123"
}
```

### Response

```json
{
  "submissionId": "sub_123",
  "status": "QUEUED"
}
```

### GET `/api/v1/submissions/{submissionId}`

Get submission status/result

### GET `/api/v1/users/{userId}/submissions`

Get all submissions of user

---

## 📊 8. Submission Lifecycle

```
QUEUED → RUNNING → COMPLETED

Final Verdicts:
- ACCEPTED
- WRONG_ANSWER
- TIME_LIMIT_EXCEEDED
- MEMORY_LIMIT_EXCEEDED
- RUNTIME_ERROR
- COMPILATION_ERROR
```

---

## 🧾 9. Detailed Submission Response

```json
{
  "submissionId": "sub_123",
  "status": "COMPLETED",
  "verdict": "ACCEPTED",
  "score": 100,
  "testcases": [
    {
      "id": 1,
      "status": "PASSED",
      "time": "0.01s"
    },
    {
      "id": 2,
      "status": "PASSED",
      "time": "0.02s"
    }
  ]
}
```

---

## 🧠 10. Internal Judge APIs (Private)

### POST `/internal/judge`

Push submission to queue

### POST `/internal/judge/result`

Judge worker sends result callback

---

## 🔔 11. Webhooks (Optional)

### POST `/api/v1/webhooks/submission`

Receive async updates (optional for frontend)

---

## ⚙️ 12. Status Codes

* `200 OK`
* `201 Created`
* `202 Accepted` (async processing)
* `400 Bad Request`
* `401 Unauthorized`
* `403 Forbidden`
* `404 Not Found`
* `409 Conflict`
* `429 Too Many Requests`
* `500 Internal Server Error`

---

## 🔄 13. Idempotency

Header:

```
Idempotency-Key: abc123
```

* Prevent duplicate submissions
* Store key → submission mapping

---

## 🚦 14. Rate Limiting

* `/runs` → 10 requests/min
* `/submissions` → 5 requests/min (stricter during contests)
* `/auth/login` → brute-force protection

---

## 🧱 15. High-Level Architecture

* API Gateway
* Auth Service
* Problem Service
* Contest Service
* Submission Service
* Judge Workers (Docker sandbox)
* Queue (Kafka / Redis)
* Database (PostgreSQL)
* Cache (Redis)

---

## 📌 16. Versioning Strategy

Use URI versioning:

```
/api/v1/...
```

---

## ✅ Notes

* All APIs are RESTful
* JWT-based authentication recommended
* Contest endpoints require role-based access (organizer/admin)
* Submission system should be asynchronous and scalable

---
