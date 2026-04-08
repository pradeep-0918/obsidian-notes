# 🐘 Hadoop Ecosystem

> **Hadoop** is an open-source framework for distributed storage and processing of large datasets using clusters of commodity hardware.

---

## 📜 Historical Timeline

```
2002 ──► Google File System (GFS) paper published
2004 ──► Google MapReduce (GMR) paper published
2005 ──► Doug Cutting creates Hadoop (inspired by GFS + GMR)
         └── Originally built for Apache Nutch (web crawler)
```

### File System Context

| OS      | Native File System |
| ------- | ------------------ |
| Windows | NTFS               |
| Linux   | Ext (Ext2/3/4)     |
| Hadoop  | HDFS               |

---

## 👨‍💻 Father of Hadoop

**Doug Cutting** — Created Hadoop while working at Yahoo!
- Named after his son's yellow stuffed elephant toy 🐘
- Later donated to the **Apache Software Foundation**

---

## 🏗️ Core Architecture

Hadoop is built on **4 core components**. All higher-level tools are **abstractions over MapReduce**.

```
┌─────────────────────────────────────────────┐
│              HADOOP FRAMEWORK               │
│                                             │
│   ┌───────────┐      ┌──────────────────┐   │
│   │   HDFS    │      │   MapReduce      │   │
│   │ (Storage) │      │  (Processing)    │   │
│   └───────────┘      └──────────────────┘   │
│                                             │
│   ┌───────────┐      ┌──────────────────┐   │
│   │   YARN    │      │   Common Utils   │   │
│   │(Scheduler)│      │  (Libraries)     │   │
│   └───────────┘      └──────────────────┘   │
└─────────────────────────────────────────────┘
```

---

## 🧱 Core Components

### 1. 🗄️ HDFS — Hadoop Distributed File System

> The storage backbone of Hadoop — modeled after Google's GFS.

- Splits files into **blocks (default 128MB)** and distributes across nodes
- Each block is **replicated 3× by default** for fault tolerance
- **Architecture:**
  - `NameNode` — Master; stores metadata (file names, locations)
  - `DataNode` — Slaves; store actual data blocks
- Designed for **write-once, read-many** workloads
- Optimized for **large files**, not millions of small files
- Works on **commodity hardware** (cheap, standard servers)

---

### 2. ⚙️ MapReduce — Batch Processing Engine

> The original distributed computation engine. Requires **Java**.

- **Map Phase** → Transforms input data into key-value pairs
- **Shuffle & Sort Phase** → Groups same keys together
- **Reduce Phase** → Aggregates key-value pairs into final output
- Processes data in **batch mode** (not real-time)
- All ecosystem tools (Hive, Pig, etc.) are abstractions built on top of MR
- **Limitation:** Slow due to disk I/O between map and reduce phases → reason why **Spark** emerged

---

### 3. 🐝 Hive — SQL Interface for Hadoop

> Invented by **Facebook**. Lets SQL users query HDFS data without knowing Java.

- Converts **HiveQL (SQL-like syntax)** → MapReduce jobs under the hood
- Acts as a **Query Engine / Data Warehouse** layer over HDFS
- Stores schema info in **Metastore** (usually MySQL/PostgreSQL)
- Supports **partitioning** and **bucketing** for performance
- **Target Users:** Analysts who know SQL but not Java
- **Skill requirement:** `SQL` knowledge only
- **Limitation:** Still slow (MR-based); modern versions support **Tez / Spark** execution engine

---

### 4. 🐷 Pig / Pig Latin — Data Flow Language

> Developed by **Yahoo!**. A scripting language for ETL and data transformation.

- Uses **Pig Latin** — a high-level data flow language (neither SQL nor Java)
- Automatically converts scripts → MapReduce jobs
- Better suited for **unstructured/semi-structured data** than Hive
- **Target Users:** Developers comfortable with scripting
- **Skill requirement:** Pig Latin only (no Java or SQL needed)
- **Use case:** Complex multi-step data pipelines / ETL workflows

---

### 5. 🔄 Sqoop — SQL ↔ Hadoop Data Transfer

> **Sqoop** = **SQ**L to Had**oop**. Built by a community of contributors.

- Transfers data **bidirectionally** between RDBMS and HDFS/Hive/HBase
- **Import:** RDBMS → HDFS / Hive / HBase
- **Export:** HDFS → RDBMS
- Uses **JDBC** to connect to databases (MySQL, Oracle, PostgreSQL, etc.)
- Runs as MapReduce jobs for parallel data transfer
- **Limitation:** Does not natively understand **HBase schema** (HBase is schema-less from Sqoop's perspective)
- **Note:** Sqoop is now largely replaced by **Apache NiFi** and **Kafka Connect**

```
Flow:  RDBMS ──Sqoop──► HDFS ──► Hive (for querying)
                                └──► HBase (❌ schema mismatch issue)
```

---

### 6. ⏰ Oozie — Workflow Scheduler

> Developed by **Yahoo!** for automating and orchestrating Hadoop jobs.

- A **server-based workflow scheduler** for managing Hadoop jobs
- Defines workflows using **XML-based DAGs** (Directed Acyclic Graphs)
- Supports scheduling of: **MapReduce, Hive, Pig, Sqoop, Shell** jobs
- Two types of jobs:
  - **Workflow** → Linear/conditional sequence of actions
  - **Coordinator** → Time-based or data-availability triggered jobs
- Integrates with **HDFS** to detect data availability before triggering jobs
- **Use case:** Run Sqoop import at midnight → process with Hive → export results

---

## 🔌 Extended Ecosystem Components

> These components also depend on MapReduce and extend Hadoop's capabilities.

### 7. 📊 HBase — NoSQL Database on HDFS

> A distributed, column-oriented **NoSQL database** modeled after Google Bigtable.

- Stores data **on top of HDFS** but provides **random read/write access** (unlike HDFS)
- **Schema-less** — columns can be added dynamically per row
- Organized into **Column Families** (must be defined at table creation)
- Supports **billions of rows × millions of columns**
- Provides **millisecond latency** lookups by row key
- **Use cases:** Real-time analytics, time-series data, messaging systems
- **Note:** Sqoop cannot directly map RDBMS schema to HBase (schema incompatibility)

---

### 8. 🤖 Mahout — Machine Learning Library

> Apache Mahout provides scalable **machine learning algorithms** on Hadoop.

- Originally ran on **MapReduce** (now moving to Spark backend)
- Provides algorithms for:
  - **Clustering** (K-Means, Fuzzy K-Means)
  - **Classification** (Naive Bayes, Random Forests)
  - **Collaborative Filtering** (Recommendation engines)
- **Use case:** Building recommendation systems, spam detection, customer segmentation
- Largely superseded by **Spark MLlib** in modern pipelines

---

### 9. 🌊 Flume — Log & Event Data Ingestion

> A distributed service for **collecting, aggregating, and moving** large volumes of streaming data.

- Designed to ingest **log data** from web servers, applications, social media
- Supports both **batch** and **stream (real-time)** data ingestion
- Architecture: **Source → Channel → Sink**
  - `Source` — Collects data (HTTP, Syslog, Twitter, Kafka)
  - `Channel` — Buffers data (Memory or File channel)
  - `Sink` — Delivers to destination (HDFS, HBase, Kafka)
- **Use case:** Streaming server logs → HDFS for analysis

---

## 🧩 Ecosystem Dependency Map

```
                    ┌─────────────────────────────────────┐
                    │         ABSTRACTION LAYER            │
  ┌──────────┐      │  ┌────────┐  ┌────────┐  ┌───────┐  │
  │  Sqoop   │──────┼─►│  Hive  │  │  Pig   │  │Mahout │  │
  │  Flume   │      │  └────────┘  └────────┘  └───────┘  │
  └──────────┘      │         │         │           │      │
        │           │         ▼         ▼           ▼      │
        ▼           │         ┌─────────────────────┐      │
    ┌───────┐       │         │     MapReduce        │      │
    │ HBase │       │         └──────────┬──────────┘      │
    └───────┘       └──────────────────  │  ───────────────┘
        │                               │
        └──────────────►  ┌─────────────▼───────────┐
                          │          HDFS            │
                          └─────────────────────────-┘
                                        ▲
                          ┌─────────────┴──────────────┐
                          │          Oozie             │
                          │   (Orchestrates all jobs)  │
                          └────────────────────────────┘
```

---

## ⚡ Why Spark Replaced Much of Hadoop MR

| Feature        | Hadoop MapReduce       | Apache Spark               |
| -------------- | ---------------------- | -------------------------- |
| Processing     | Disk-based (slow)      | In-memory (100× faster)    |
| API            | Java only              | Java, Python, Scala, R     |
| Streaming      | Batch only             | Batch + Real-time          |
| ML Support     | Mahout (limited)       | MLlib (rich)               |
| Graph Support  | None                   | GraphX                     |
| Integration    | HDFS native            | Works with HDFS, S3, etc.  |

> **Spark** can run **on top of Hadoop (YARN)** — it replaces MapReduce but still uses HDFS for storage. This is why ~95% of Hadoop components can integrate with Spark.

---

## 🧰 Hadoop as a Framework

- **Loosely coupled** — components are independently deployable and replaceable
- **Open Source** — Apache Software Foundation licensed
- **Highly integrable** — connects with ML, AI, BI tools, cloud platforms
- **Language support:** Java (core), SQL (Hive), Python, Scala (via Spark)
- Approximately **50% Java/Linux** knowledge + **50% SQL/Python/Scala** is the skill requirement for Hadoop engineers

---

## 🗺️ Quick Reference — Who Made What

| Component  | Creator              | Language   | Purpose                     |
| ---------- | -------------------- | ---------- | --------------------------- |
| HDFS       | Yahoo! / Apache      | Java       | Distributed storage         |
| MapReduce  | Yahoo! / Apache      | Java       | Batch processing            |
| Hive       | Facebook             | HiveQL/SQL | SQL querying on HDFS        |
| Pig        | Yahoo!               | Pig Latin  | ETL / data flow scripting   |
| Sqoop      | Community (Apache)   | Java       | RDBMS ↔ Hadoop transfer    |
| Oozie      | Yahoo!               | XML/Java   | Workflow scheduling         |
| HBase      | Apache               | Java       | NoSQL on HDFS               |
| Mahout     | Apache               | Java       | Machine learning            |
| Flume      | Cloudera → Apache    | Java       | Log/event data ingestion    |

---

## 🔗 Related Notes

- [[Apache Spark]]
- [[HDFS Deep Dive]]
- [[Data Engineering Pipeline]]
- [[NoSQL Databases]]
- [[MapReduce Algorithm]]

---

*Tags: #hadoop #bigdata #data-engineering #distributed-systems #hdfs #mapreduce #hive #spark*
