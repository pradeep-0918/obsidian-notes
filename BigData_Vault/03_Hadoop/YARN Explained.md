# 🚦 YARN Explained — Yet Another Resource Negotiator

#hadoop #yarn #resource-management #feynman

---

## 🍅 Pomodoro 7 — YARN: The Traffic Controller

---

## 🧒 Feynman Explanation — The Airport Analogy

Imagine an airport (your Hadoop cluster):
- It has **runways** (CPU and RAM on each machine)
- Many **airlines** want to use them (Spark jobs, MapReduce jobs, Hive queries)
- Without control, planes crash into each other (resource conflicts)

**YARN = The Air Traffic Controller**

YARN decides:
- Who gets to use which runway (which job gets which machine's resources)
- How much runway time each airline gets (resource allocation)
- What to do if a plane goes off-course (fault recovery)

---

## 🏗️ YARN Architecture

```
                    ┌──────────────────────┐
                    │  Resource Manager    │  ← The ATC Tower
                    │  (One per cluster)   │    Global resource decisions
                    │                      │
                    │  ┌─────────────────┐ │
                    │  │    Scheduler    │ │  ← Assigns containers
                    │  └─────────────────┘ │
                    │  ┌─────────────────┐ │
                    │  │ Apps Manager    │ │  ← Tracks all running apps
                    │  └─────────────────┘ │
                    └──────────┬───────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
   ┌──────▼──────┐      ┌──────▼──────┐      ┌──────▼──────┐
   │ Node Manager│      │ Node Manager│      │ Node Manager│
   │  Machine 1  │      │  Machine 2  │      │  Machine 3  │
   │             │      │             │      │             │
   │ [Container] │      │ [Container] │      │ [Container] │
   │ [Container] │      │ [Container] │      │             │
   └─────────────┘      └─────────────┘      └─────────────┘
        Workers                Workers              Workers
```

---

## 🔑 YARN Components

### 1. Resource Manager (RM)

- **What it is:** The master of the entire cluster
- **Job:** Allocates resources (CPU + RAM) across all running applications
- **Has two parts:**
  - **Scheduler** — Pure resource allocator (doesn't monitor tasks)
  - **Applications Manager** — Accepts job submissions, launches App Masters

### 2. Node Manager (NM)

- **What it is:** The worker on each machine
- **Job:** Manages resources on ONE machine
- Reports to Resource Manager: "I have 8 CPU cores and 32GB RAM available"
- Launches and monitors **Containers** on its machine

### 3. Application Master (AM)

- **What it is:** The manager of ONE specific job/application
- **Where it runs:** Inside a Container on any node
- **Job:** Negotiates resources from RM, monitors its tasks, handles failures

> 🧠 One Application Master is created **per job**. It lives only for the duration of that job.

### 4. Container

- **What it is:** A bundle of resources (X CPU cores + Y GB RAM) on a specific machine
- **Where tasks run:** Every mapper and reducer runs inside a Container
- **Isolated:** Each container is sandboxed from others

---

## ⚙️ How a Job Runs on YARN (Step by Step)

```
Step 1: Client submits job to Resource Manager
         ↓
Step 2: RM creates an Application Master for this job
         ↓
Step 3: AM asks RM for Containers (resources) for its tasks
         ↓
Step 4: RM tells AM: "You can use Machine 2 (2 cores, 4GB) and Machine 5 (4 cores, 8GB)"
         ↓
Step 5: AM tells Node Managers on those machines to launch Containers
         ↓
Step 6: Tasks (mappers, reducers) run inside those Containers
         ↓
Step 7: AM monitors tasks. If a task fails → AM requests a new Container and retries
         ↓
Step 8: Job completes. AM reports to RM. Containers released.
```

---

## 🌟 Why YARN Was a Game Changer

### Hadoop 1.x Problem: JobTracker Overload

In Hadoop 1.x, the **JobTracker** did EVERYTHING:
- Resource management
- Task scheduling
- Task monitoring
- Job tracking

This was a massive bottleneck — JobTracker could only manage ~4,000 nodes.

### Hadoop 2.x Solution: YARN Separation of Concerns

```
Hadoop 1.x JobTracker = Resource Manager + App Master + Task Tracking
(All in one overloaded component)

Hadoop 2.x YARN = Resource Manager (global) + App Master (per-job) + Node Manager (per-node)
(Separated responsibilities = scales to 10,000+ nodes)
```

### YARN Enables Multi-Framework Support

| Before YARN | After YARN |
|-------------|-----------|
| Only MapReduce could run on Hadoop | Spark, Tez, Flink, MPI can all run on YARN |
| Cluster dedicated to one framework | One cluster, multiple frameworks sharing resources |

---

## 📊 YARN Schedulers

YARN has 3 built-in schedulers:

| Scheduler | How It Works | Best For |
|-----------|-------------|----------|
| **FIFO** | First job submitted, first served | Simple single-tenant clusters |
| **Capacity** | Divide cluster into queues (e.g., 60% for analytics, 40% for ML) | Multi-department organizations |
| **Fair Scheduler** | Every job gets equal share; unused resources lent to active jobs | Mixed workload, fairness priority |

---

## 🔗 Connected Notes

- [[HDFS Deep Dive]] — Where data is stored before YARN processes it
- [[MapReduce Explained]] — One of many frameworks that runs on YARN
- [[Hadoop Origin Story]] — Why YARN was added in Hadoop 2.x
- [[Top 30 Interview QA]] — YARN interview questions

---

## 🧠 Quick Recall Check

1. What does YARN stand for?
2. What is the difference between Resource Manager and Node Manager?
3. What is a Container in YARN?
4. Why was YARN introduced in Hadoop 2.x? What problem did it solve?
5. Which scheduler gives equal resources to all jobs?
6. How many Application Masters run per Hadoop cluster?
