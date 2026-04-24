# 🧠 Online Coding Judge System (System Design Mini Project)

---

## 📌 Overview

This project focuses on designing an **Online Coding Judge system**, similar to platforms like LeetCode and HackerRank.

The system simulates how large-scale coding platforms handle:

* ⚡ Code execution
* 🧪 Test case evaluation
* 📈 High concurrent submissions
* 🏗️ Scalable and reliable architecture

This design is based on my existing project:

👉 https://github.com/AshutoshYadav08/ContestCompilerMajor

---

## 🎯 Objective

The objective of this mini project is to apply core system design concepts:

* High-Level Design (HLD)
* Low-Level Design (LLD)
* API Design
* Database Design
* Scalability & Reliability

and understand how real-world coding platforms work internally.

---

## ⚙️ Features

* 👨‍💻 Users can write and submit code
* ▶️ Run code with custom input
* 🧪 Evaluate submissions against multiple test cases
* 📊 Get detailed verdicts:

  * ✅ Accepted
  * ❌ Wrong Answer
  * ⏱️ Time Limit Exceeded
  * 💥 Runtime Error
  * ⚠️ Compilation Error

---

## 🧠 Key Design Ideas

### ⚡ Asynchronous Processing

Submissions are processed asynchronously:

```text
User → API → Queue → Worker → Execution → Result
```

This ensures:

* High scalability
* Better performance under load

---

### 🧪 Execution Engine

* Uses Judge0 API for code execution
* Handles compilation + runtime safely

---

### 🧩 Separation of Concerns

* **API Layer** → Handles requests & validation
* **Queue System** → Manages submission load
* **Worker Service** → Executes code
* **Database** → Stores submissions & results

---

### 📈 Scalability Approach

* Horizontal scaling of workers
* Queue-based load balancing
* Caching for read-heavy endpoints

---

## 🏗️ High-Level Architecture

```text
Client
  ↓
API Gateway
  ↓
Submission Service → Queue (Kafka/Redis)
  ↓
Judge Workers → Execution Engine (Judge0)
  ↓
Database (PostgreSQL)
  ↓
Cache (Redis)
```

---

## 📁 Repository Structure

```text
/project-name
├── HLD.md        # System architecture & components
├── LLD.md        # Classes, DB schema, relationships
├── api.md        # API endpoints & contracts
├── scaling.md    # Scaling strategy & optimizations
└── README.md     # Project overview
```

---

## 📄 File Descriptions

* **HLD.md** → Overall system architecture and data flow
* **LLD.md** → Class design, DB schema, relationships
* **api.md** → REST APIs and request/response formats
* **scaling.md** → Bottlenecks, scaling strategies, optimizations

---

## ⚠️ Assumptions

* Code execution is handled by an external service (Judge0)
* Focus is on system design (not full backend implementation)
* Output comparison is exact match (no partial scoring)

---

## ⚖️ Trade-offs

* ✅ Faster development using external execution API

* ❌ Limited control over execution environment

* ✅ Strong consistency for submissions

* ❌ Some read APIs use eventual consistency for scalability

---

## 🚀 Future Improvements

* 🐳 Build in-house execution engine (Docker sandbox)
* 🏆 Real-time contest & leaderboard system
* 🧠 Advanced judging (floating point precision, edge cases)
* 🔍 Plagiarism detection system
* ⚡ Optimize for massive scale (millions of submissions)

---

## 🧱 Tech Stack (Suggested)

* Backend: Node.js / Express
* Database: PostgreSQL
* Cache: Redis
* Queue: Kafka / Redis Queue
* Execution: Judge0 API
* Containerization: Docker

---

## 📊 What I Learned

This project helped me understand:

* Handling **concurrent submissions at scale**
* Designing **distributed systems**
* Managing **async workflows with queues**
* Making **trade-offs between scalability & consistency**

---

## 🙌 Final Note

This project demonstrates how real-world coding platforms are designed to handle:

* High traffic
* Distributed execution
* Reliable and scalable judging

It reflects practical system design thinking and backend architecture skills.

---
