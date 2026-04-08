# 🧠 Key Big Data Concepts — Must Know

#big-data #concepts #interview #fundamentals

---

## 🏗️ Batch vs Stream Processing

### The Core Difference

```
Batch Processing:
Data accumulates → Processed all at once → Results delivered later
[COLLECT] → [WAIT] → [PROCESS] → [RESULT]
Example: Monthly salary calculation

Stream Processing:
Data arrives → Processed immediately → Results delivered instantly
[ARRIVE] → [PROCESS] → [RESULT in milliseconds]
Example: UPI transaction fraud detection
```

### Tool Comparison

| | Batch | Micro-batch | Real-time Stream |
|--|-------|-------------|-----------------|
| **Tool** | Hadoop MapReduce | Spark Streaming | Apache Flink / Kafka Streams |
| **Latency** | Minutes to hours | Seconds | Milliseconds |
| **Use case** | Daily reports | Near-real-time dashboards | Fraud detection, live feeds |

---

## 🏞️ Data Lake vs Data Warehouse vs Data Lakehouse

### Data Warehouse (Old Model)

```
Source → ETL (Extract, Transform, Load) → Structured Warehouse → SQL Queries
```

- Data is **cleaned and structured BEFORE** storing
- Schema-on-write
- Fast for queries, inflexible for raw data
- Example: Amazon Redshift, Snowflake, Google BigQuery

### Data Lake (Hadoop Era)

```
Source → Raw Storage (HDFS/S3) → Schema applied WHEN reading
```

- Store **everything raw**, figure out structure later
- Schema-on-read
- Flexible, cheap storage; can become a "data swamp" without governance
- Example: HDFS, Amazon S3 with Hive

### Data Lakehouse (Modern)

```
Combines the flexibility of Data Lake + query performance of Data Warehouse
```

- Store raw data in open formats (Parquet/ORC)
- Add ACID transactions via Delta Lake / Apache Iceberg
- One platform for BI, ML, and streaming
- Example: Databricks, Apache Iceberg on S3

```
Data Warehouse: 🏢 Organized office — everything has a place, expensive
Data Lake:      🌊 Ocean — everything goes in, chaotic, cheap
Data Lakehouse: 🏠 Home — organized rooms (warehouse) with a garage (lake)
```

---

## 🔑 ETL vs ELT

| | ETL | ELT |
|-|-----|-----|
| **Stands for** | Extract → Transform → Load | Extract → Load → Transform |
| **When transform** | Before loading | After loading |
| **Where transform** | Dedicated ETL server | Inside the data warehouse |
| **Era** | Traditional (pre-Big Data) | Modern (cloud + Big Data) |
| **Example** | Informatica, Talend | dbt, Spark SQL |

---

## 📋 Key Distributed Systems Concepts

### Fault Tolerance
> System keeps working even when components fail.

HDFS achieves this via 3x replication. If one DataNode dies, 2 copies remain.

### Scalability — Horizontal vs Vertical

```
Vertical Scaling (Scale Up):   Make one machine bigger (more RAM, faster CPU)
                                Limit: Most expensive server in the world still has limits

Horizontal Scaling (Scale Out): Add more machines
                                Limit: Nearly unlimited (this is what Hadoop does)
```

### Data Locality

> Move computation to where data lives, not data to computation.

In MapReduce, the system tries to run a Mapper **on the same machine** that holds its data block. This eliminates network transfer.

```
Without locality: DataNode → Network → Compute Node (slow)
With locality:    DataNode = Compute Node (fast — reads from local disk)
```

### Idempotency

> Running an operation multiple times gives the same result as running it once.

Critical in Big Data because failures happen. If a task is re-run after failure, it shouldn't corrupt data.

---

## ⚡ Serialization Formats — Row vs Column

### Row-Based (Traditional)
```
| ID | Name    | Age | City    |
|----|---------|-----|---------|
| 1  | Pradeep | 22  | Salem   |
| 2  | Anitha  | 25  | Chennai |

Storage: [1, Pradeep, 22, Salem] [2, Anitha, 25, Chennai]
```
Good for: Fetching complete records (OLTP — operational databases)
Bad for: Analytical queries ("What's the average age?" scans ALL columns)

### Column-Based (Big Data)
```
ID column:   [1, 2, 3, ...]
Name column: [Pradeep, Anitha, ...]
Age column:  [22, 25, ...]
```
Good for: Analytical queries — only read relevant columns
Bad for: Fetching a complete record (need to read from multiple column files)

**Formats:** Parquet (most popular), ORC (Hive-native), Avro (row-based, for streaming)

---

## 🗜️ Compression in Big Data

### Why Compress?

- 1 TB uncompressed → 200 GB compressed (80% space savings)
- Less data = faster transfers across the cluster
- Less disk I/O = faster processing

### Common Algorithms

| Algorithm | Ratio | Speed | Splittable? |
|-----------|-------|-------|-------------|
| **Gzip** | High | Medium | ❌ No |
| **Snappy** | Medium | Very Fast | ❌ No |
| **LZO** | Medium | Fast | ✅ Yes |
| **Bzip2** | Highest | Slow | ✅ Yes |

> 🔑 **Interview point:** "Splittable" means Hadoop can split a compressed file and assign chunks to different mappers. Non-splittable files (like gzip) force Hadoop to use ONE mapper for the entire file — killing parallelism.

---

## 🔗 Connected Notes

- [[The 5 Vs of Big Data]] — Velocity drives stream processing need
- [[HDFS Deep Dive]] — Storage layer for Data Lakes
- [[MapReduce Explained]] — Classic batch processing
- [[Top 30 Interview QA]] — All these concepts in Q&A format

---

## 🧠 Quick Recall Check

1. What's the difference between a Data Lake and a Data Warehouse?
2. What is data locality and why does it matter?
3. Why is columnar storage better for analytics?
4. What does "splittable" mean for compression codecs?
5. What is ELT vs ETL?
