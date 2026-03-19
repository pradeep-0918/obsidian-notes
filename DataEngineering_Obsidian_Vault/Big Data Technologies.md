# 🐘 Big Data Technologies

> **Level:** Intermediate | **Stage:** 3 of 5
> Big Data is about processing data that's too large, fast, or varied for traditional tools — understanding these concepts unlocks the most powerful DE tools.

---

## 🤔 What is "Big Data"?

Big Data is characterized by the **5 V's:**

| V | Meaning | Example |
|---|---------|---------|
| **Volume** | Massive amounts of data | Petabytes of logs |
| **Velocity** | Fast-moving data | Real-time stock feeds |
| **Variety** | Many data types | Text, images, JSON, video |
| **Veracity** | Data quality/trust | Inconsistent sensor readings |
| **Value** | Business insight gained | Fraud detection |

> When a single machine can't handle your data — you need distributed systems.

---

## 🧠 Key Concepts

### 1. Distributed Computing
Split work across **many machines** (a cluster) instead of one.

```
Traditional:   [1 Machine] → processes all 10TB → slow / crashes

Distributed:   [Machine 1] → processes 1TB
               [Machine 2] → processes 1TB
               ...
               [Machine 10] → processes 1TB → combine results
```

### 2. Hadoop Ecosystem (The Original Big Data Stack)

**HDFS (Hadoop Distributed File System):**
- Stores large files split into **blocks** (128MB each)
- Each block is **replicated 3x** (fault tolerant)
- Like a distributed hard drive

**MapReduce:**
- Original Hadoop compute engine
- **Map:** Process each chunk in parallel
- **Reduce:** Combine all results
- Mostly replaced by Spark today (but important to understand)

```
Word Count Example:
Map: "hello world hello" → [("hello",1), ("world",1), ("hello",1)]
Reduce: [("hello",2), ("world",1)]
```

**YARN (Yet Another Resource Negotiator):**
- Manages cluster resources (CPU, memory)
- Decides which machine runs which job

### 3. The Modern Big Data Stack

```
❌ Old Stack (2010s):     HDFS + MapReduce + Hive
✅ Modern Stack (2020s):  S3/GCS + Apache Spark + Delta Lake + dbt
```

### 4. Key Concepts in Distributed Processing

**Partitioning:** Split data into chunks processed in parallel
```python
# Spark auto-partitions; you can also control it:
df.repartition(100)  # 100 partitions
df.coalesce(10)      # reduce to 10 partitions
```

**Shuffling:** Moving data between machines (expensive — minimize it!)

**Fault Tolerance:** If a machine fails, recompute only the lost partition

**Lazy Evaluation (Spark):** Transformations aren't run until you trigger an action
```python
df.filter(df.age > 25).select("name")  # Nothing runs yet (lazy)
df.show()  # NOW Spark runs the whole plan
```

### 5. CAP Theorem (for Distributed Systems)
You can only guarantee **2 of 3:**
- **Consistency:** All nodes see same data
- **Availability:** System always responds
- **Partition Tolerance:** Works despite network failures

---

## 🏭 Real-World Big Data Use Cases

| Company | Problem | Solution |
|---------|---------|----------|
| Netflix | Recommend shows to 200M users | Spark ML on petabytes of watch history |
| Uber | Real-time surge pricing | Kafka + Flink streaming |
| LinkedIn | Who viewed your profile | Hadoop → Spark migration |
| Twitter/X | Trending topics | Real-time stream processing |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Apache Hadoop** | Original distributed storage + compute |
| **Apache Spark** | Fast distributed processing (see [[Apache Spark]]) |
| **Apache Kafka** | Real-time event streaming (see [[Apache Kafka]]) |
| **Apache Hive** | SQL on top of HDFS |
| **Apache Flink** | Real-time stream processing |
| **Presto / Trino** | Fast SQL query engine across lakes |
| **Delta Lake** | ACID transactions on data lakes |

---

## ✅ Mini Practice Tasks

- [ ] Explain MapReduce to someone using a word count example
- [ ] Research the difference between Spark and Hadoop MapReduce — why did Spark win?
- [ ] Draw a simple 3-node Hadoop cluster with HDFS replication
- [ ] Look up one real-world case study of a company migrating from Hadoop to Spark

---

## 🔗 Related Notes

- [[Apache Spark]] — The dominant big data processing engine today
- [[Apache Kafka]] — Handles the Velocity (fast-moving data) problem
- [[Data Lakes vs Data Warehouses]] — Hadoop/HDFS is the original data lake
- [[Cloud for Data Engineering]] — Cloud-managed big data services (EMR, Dataproc)

---

## ➡️ Next Step

[[Apache Spark]] — The most important big data tool you'll use day-to-day.
