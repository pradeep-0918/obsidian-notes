# ⚡ Big Data Cheat Sheet — One-Page Reference

#cheat-sheet #quick-reference #revision

---

## 🔑 The 5 Vs

| V | Definition | Example |
|---|-----------|---------|
| **Volume** | Size of data | Facebook: 4 PB/day |
| **Velocity** | Speed of arrival | UPI: 400M transactions/day |
| **Variety** | Types of data | Text + images + logs + JSON |
| **Veracity** | Data quality | Sensor noise, fake social media |
| **Value** | Business insight | Netflix: 80% streams from recommendations |

---

## 🏗️ Hadoop Core — 3 Components

```
HDFS         → Store data in 128MB blocks, 3x replicated
MapReduce    → Process data: Split → Map → Shuffle/Sort → Reduce
YARN         → Manage resources: RM + NM + AppMaster + Container
```

---

## 💾 HDFS Key Facts

| Fact | Value |
|------|-------|
| Default block size | 128 MB |
| Default replication | 3 |
| NameNode role | Metadata only (NOT data) |
| DataNode role | Actual data storage |
| Secondary NameNode | Merges EditLog + FsImage (NOT a backup!) |
| Write model | Append-only (no random writes) |
| Heartbeat interval | 3 seconds |

---

## ⚙️ MapReduce Phases

```
1. Split     → Divide input by blocks
2. Map       → (key, value) → list of (key, value)
3. Shuffle   → Group by key across all mappers
4. Sort      → Sort within each group
5. Reduce    → Aggregate values per key
```

**Combiner** = Mini-reducer on map side (reduces shuffle data)
**Partitioner** = Decides which reducer gets which key

---

## 🚦 YARN Components

| Component | Role | Count |
|-----------|------|-------|
| Resource Manager | Global cluster resource allocation | 1 per cluster |
| Node Manager | Resources on one machine | 1 per machine |
| Application Master | Manages one specific job | 1 per job |
| Container | Bundle of resources (CPU + RAM) | Many per job |

---

## 📊 Key Comparisons

### Hadoop 1.x vs 2.x
| | 1.x | 2.x |
|-|-----|-----|
| Resource manager | JobTracker (overloaded) | YARN (separated) |
| Frameworks supported | MapReduce only | Spark, Flink, Tez... |
| NameNode HA | No | Yes |
| Max cluster nodes | ~4,000 | 10,000+ |

### MapReduce vs Spark
| | MapReduce | Spark |
|-|-----------|-------|
| Processing | Disk-based | In-memory |
| Speed | Slower | 10-100x faster |
| API | Java verbose | Python/Scala/R friendly |
| Use case | Simple batch | Batch + Streaming + ML |

### Data Lake vs Warehouse vs Lakehouse
| | Lake | Warehouse | Lakehouse |
|-|------|-----------|-----------|
| Schema | On-read | On-write | On-read + ACID |
| Data type | Raw/any | Structured | Any |
| Cost | Cheap | Expensive | Medium |
| Tool | HDFS/S3 | Redshift/BQ | Delta Lake |

---

## ⚖️ CAP Theorem

> Choose 2 of: **C**onsistency, **A**vailability, **P**artition Tolerance
> Since P is always required: Real choice = **CP vs AP**

| System | Type | Trade-off |
|--------|------|-----------|
| HDFS | CP | Unavailable if NameNode dies |
| HBase | CP | Consistent, may be unavailable |
| Cassandra | AP | Always up, eventually consistent |
| ZooKeeper | CP | Consistent coordination |

---

## 📁 File Formats

| Format | Type | Best For |
|--------|------|---------|
| Parquet | Columnar | Spark analytics, cross-platform |
| ORC | Columnar | Hive-native, ACID support |
| Avro | Row-based | Streaming, Kafka, schema evolution |
| CSV | Row-based | Human-readable, small files only |

---

## 🗜️ Compression Codecs

| Codec | Splittable | Speed | Ratio |
|-------|-----------|-------|-------|
| Gzip | ❌ No | Medium | High |
| Snappy | ❌ No | Fast | Medium |
| LZO | ✅ Yes | Fast | Medium |
| Bzip2 | ✅ Yes | Slow | Highest |

> **Splittable = Critical for Hadoop parallelism**

---

## 🐘 Hadoop Ecosystem Quick Map

```
Ingest:    Sqoop (DB→HDFS) | Flume (Logs) | Kafka (Streaming)
Store:     HDFS (Files) | HBase (NoSQL) | Hive (SQL Tables)
Process:   MapReduce | Spark | Flink
SQL:       Hive | Impala | Presto | Spark SQL
Coordinate: ZooKeeper | YARN
```

---

## 📅 Key Dates

| Year | Event |
|------|-------|
| 2003 | Google publishes GFS paper |
| 2004 | Google publishes MapReduce paper |
| 2005 | Doug Cutting creates Hadoop (at Yahoo) |
| 2006 | Hadoop becomes Apache project |
| 2012 | Apache Spark released |
| 2013 | YARN released (Hadoop 2.x) |
| 2014 | Spark replaces MapReduce at most companies |
| 2017 | Hadoop 3.x: Erasure coding |
| 2020+ | Data Lakehouse era begins |

---

## 🧠 One-Line Definitions

- **Big Data**: Data too large/fast/complex for one machine → solved by distributed systems
- **HDFS**: Distributed file system with 128MB blocks and 3x replication
- **MapReduce**: Divide data into chunks, process in parallel, combine results
- **YARN**: Cluster resource manager that replaced JobTracker
- **Spark**: Faster in-memory alternative to MapReduce
- **Kafka**: Real-time message streaming platform
- **Hive**: SQL interface on top of HDFS/MapReduce
- **ZooKeeper**: Distributed coordination service (leader election, locks)
- **Data Lake**: Raw data storage at any scale (HDFS/S3)
- **Data Lakehouse**: Data Lake + ACID transactions + query performance

---

## 🔗 Full Notes

- [[What is Big Data]]
- [[The 5 Vs of Big Data]]
- [[History of Big Data]]
- [[Hadoop Origin Story]]
- [[HDFS Deep Dive]]
- [[MapReduce Explained]]
- [[YARN Explained]]
- [[CAP Theorem]]
- [[Data Partitioning]]
- [[Key Big Data Concepts]]
- [[Top 30 Interview QA]]
- [[Real World Use Cases]]
