# 🎯 Top 30 Big Data & Hadoop Interview Q&A

#interview #hadoop #big-data #qa

---

## 🍅 Pomodoro 10 — Interview Blast

> Go through 5-6 questions per sitting. Answer out loud before reading the answer.

---

## 🟢 Beginner Level (Q1–Q10)

---

**Q1. What is Big Data? Why can't traditional systems handle it?**

> Big Data refers to datasets so large, fast, or complex that traditional database tools cannot capture, store, process, or analyze them within reasonable time or cost. Traditional systems (like MySQL) run on a single machine with limited CPU, RAM, and storage. Big Data works by distributing data and processing across hundreds or thousands of machines.

---

**Q2. What are the 5 Vs of Big Data?**

> **Volume** (size), **Velocity** (speed of arrival), **Variety** (structured/semi-structured/unstructured), **Veracity** (data quality/accuracy), **Value** (business insights derived). The first 3 Vs were defined by Gartner in 2001; Veracity and Value were added later.

---

**Q3. What is Hadoop? Who created it?**

> Hadoop is an open-source distributed computing framework for storing and processing large datasets across commodity hardware. Created by **Doug Cutting** (then at Yahoo) in 2005, inspired by Google's GFS and MapReduce papers. Named after his son's toy elephant.

---

**Q4. What are the 3 core components of Hadoop?**

> **HDFS** (storage), **MapReduce** (processing), **YARN** (resource management). Together they handle: store it → process it → manage who gets what resources.

---

**Q5. What is HDFS? How is it different from a regular file system?**

> HDFS (Hadoop Distributed File System) stores files across hundreds of machines in 128MB blocks with 3x replication. Unlike a regular file system (ext4, NTFS), HDFS: (1) works across multiple machines, (2) uses large 128MB blocks instead of 4KB, (3) replicates data 3x for fault tolerance, (4) is append-only (no random writes), and (5) is optimized for sequential reads of large files.

---

**Q6. What is the role of NameNode in HDFS?**

> The NameNode is the **master** of HDFS. It stores metadata: the locations of all blocks, file-to-block mapping, and DataNode status. It does NOT store actual data. It's a single point of failure in Hadoop 1.x (addressed by HA NameNode in later versions).

---

**Q7. What is a DataNode?**

> DataNodes are worker machines that actually store the data blocks. They send a **heartbeat** every 3 seconds to the NameNode. If a DataNode misses heartbeats, NameNode marks it as dead and initiates re-replication of its blocks on other nodes.

---

**Q8. What is Secondary NameNode? Is it a backup NameNode?**

> ⚠️ Common trap! Secondary NameNode is **NOT** a backup NameNode. It periodically merges the **edit log** (log of all file operations) with the **FsImage** (snapshot of the file system) to reduce NameNode startup time. If the NameNode dies, Secondary NameNode's data helps recovery but it doesn't automatically take over.

---

**Q9. Explain the MapReduce workflow.**

> MapReduce has 4 phases:
> 1. **Split**: Input data split into chunks
> 2. **Map**: Each mapper processes its chunk, outputting (key, value) pairs
> 3. **Shuffle & Sort**: Intermediate results grouped by key and sorted
> 4. **Reduce**: Reducers aggregate values for each key
>
> Example: Word count — Mapper outputs (word, 1) for each word. Reducer sums all 1s per word.

---

**Q10. What is YARN and why was it introduced?**

> YARN (Yet Another Resource Negotiator) was introduced in Hadoop 2.x to separate resource management from job processing. In Hadoop 1.x, the JobTracker did everything (schedule, track, manage resources) — a bottleneck limited to ~4,000 nodes. YARN split this into Resource Manager (global), Application Master (per job), and Node Manager (per machine) — enabling 10,000+ node clusters and allowing non-MapReduce frameworks (Spark, Flink) to run on Hadoop.

---

## 🟡 Intermediate Level (Q11–Q20)

---

**Q11. What is a Combiner in MapReduce? How does it help?**

> A Combiner is a mini-reducer that runs **locally on each mapper** before the shuffle phase. It pre-aggregates data, reducing the amount of data sent across the network. Example: Instead of sending (word,1),(word,1),(word,1) to the reducer, the combiner first combines them to (word,3). Not all reduce operations can use a combiner — it only works if the function is **commutative and associative** (like sum, max, min — but not average).

---

**Q12. What is block size in HDFS? Why is it so large (128MB)?**

> Default block size in Hadoop 2.x is 128MB (64MB in 1.x). It's large because: (1) Big Data files are large — fewer blocks means less metadata in NameNode, (2) Fewer block seek operations, (3) Optimized for sequential read of large files, (4) Reduces overhead of network connections. The tradeoff: small files are stored inefficiently (a 1KB file still occupies a 128MB block entry in NameNode memory).

---

**Q13. Explain data replication in HDFS. What is the default replication factor?**

> Default replication factor is **3**. Placement strategy (rack-aware): Copy 1 on local node, Copy 2 on a different node in the same rack (fast local write), Copy 3 on a node in a different rack (fault isolation). This ensures: (1) Data survives single node failure, (2) Data survives single rack failure (power outage, switch failure).

---

**Q14. What is the difference between Hadoop 1.x and Hadoop 2.x?**

> **Hadoop 1.x**: JobTracker + TaskTracker model; only MapReduce supported; NameNode was single point of failure; limited to ~4000 nodes.
> **Hadoop 2.x**: YARN introduced; any framework (Spark, Tez, Flink) can run on cluster; HA NameNode (two NameNodes in active/standby); scales to 10,000+ nodes.

---

**Q15. What is Hadoop streaming?**

> Hadoop Streaming is a utility that allows you to write MapReduce jobs in any language (Python, Ruby, Bash) using stdin/stdout. The mapper reads from stdin, the reducer reads from stdin. Useful when your team knows Python but not Java.

---

**Q16. What is the difference between MapReduce and Apache Spark?**

> **MapReduce**: Disk-based processing, reads/writes HDFS between each stage, slower (high latency), good for simple batch jobs.
> **Spark**: In-memory processing (RDDs/DataFrames cached in RAM), 10-100x faster, supports batch + streaming + ML + graph in one framework, lazy evaluation for optimization. Spark replaced MapReduce for most use cases but both can run on YARN.

---

**Q17. What is the CAP Theorem? Where does HDFS/HBase fall?**

> CAP theorem: A distributed system can only guarantee 2 of 3: **Consistency** (all nodes see same data), **Availability** (always responds), **Partition Tolerance** (survives network splits). Since network partitions always happen, the real choice is CP vs AP.
> **HDFS**: CP — prioritizes consistency; unavailable if NameNode is down.
> **HBase**: CP — consistent reads/writes but may be unavailable during failures.
> **Cassandra**: AP — always available, eventually consistent.

---

**Q18. What is a Data Lake? How is it different from a Data Warehouse?**

> **Data Lake**: Raw data storage in any format (schema-on-read), cheap, flexible, can become a "data swamp" without governance. Tools: HDFS, S3.
> **Data Warehouse**: Structured, cleaned data (schema-on-write), expensive, fast for SQL queries. Tools: Redshift, BigQuery, Snowflake.
> **Data Lakehouse**: Combines both — raw storage with ACID transactions and query performance. Tools: Delta Lake, Apache Iceberg.

---

**Q19. Explain data locality in Hadoop. Why is it important?**

> Data locality = running computation on the same machine where data is stored, avoiding network transfer. Hadoop's scheduler attempts to assign mappers to nodes that hold their data blocks. Three levels: (1) Data-local (best — compute and data on same node), (2) Rack-local (data on different node, same rack), (3) Off-rack (worst — cross-rack transfer). Data locality is critical because network bandwidth is the bottleneck in a cluster, not CPU.

---

**Q20. What is the small files problem in Hadoop? How do you solve it?**

> HDFS is optimized for large files. Millions of small files (each smaller than a block) create problems: each file requires a metadata entry in NameNode's memory (~150 bytes per file), NameNode heap fills up, MapReduce launches one mapper per file creating massive overhead. Solutions: (1) Merge small files into larger files using **HAR files** or **SequenceFiles**, (2) Use Hive with ORC/Parquet which pack many records, (3) Use Apache Kafka to batch small events before landing in HDFS.

---

## 🔴 Advanced Level (Q21–Q30)

---

**Q21. What is Speculative Execution in Hadoop?**

> If a task is running slower than average (possibly due to a bad machine), Hadoop launches a duplicate "speculative" task on another machine. Whichever finishes first wins; the other is killed. This prevents one slow machine ("straggler") from delaying the entire job. Configured by `mapreduce.map.speculative` and `mapreduce.reduce.speculative`.

---

**Q22. What is HDFS Federation?**

> In a large cluster, a single NameNode becomes a bottleneck (all metadata in one machine's RAM). HDFS Federation allows multiple independent NameNodes, each managing a separate namespace volume. DataNodes are shared across all NameNodes. Introduced in Hadoop 2.x to scale beyond single-NameNode limits.

---

**Q23. What is erasure coding in HDFS 3.x? How is it better than replication?**

> Traditional replication: 3 copies = 200% overhead.
> Erasure coding (RS-6-3): Stores 6 data blocks + 3 parity blocks. Can reconstruct data from any 6 of 9. Storage overhead: only 50% (vs 200% for 3x replication). Trade-off: higher CPU cost for encoding/decoding. Best for cold data (accessed rarely). HDFS 3.x supports erasure coding for storage efficiency.

---

**Q24. Explain checkpointing in HDFS.**

> NameNode maintains two files: **FsImage** (full snapshot of namespace) and **EditLog** (log of all changes since last snapshot). On restart, NameNode replays EditLog on top of FsImage to reconstruct current state. If EditLog grows too large, startup takes forever. Checkpointing = Secondary NameNode periodically merges EditLog into FsImage, creating a new FsImage and clearing the EditLog. In HA setups, the Standby NameNode does this automatically.

---

**Q25. What is partitioning in Hive? What is bucketing?**

> **Partitioning**: Divides data into folders by column value. `PARTITIONED BY (year INT, month INT)` creates `data/year=2024/month=01/`. Enables partition pruning — queries skip irrelevant partitions.
> **Bucketing**: Within a partition, further divide data into fixed number of files using hash(column). `CLUSTERED BY user_id INTO 32 BUCKETS`. Enables efficient joins (both sides bucketed on join key) and sampling.

---

**Q26. How does YARN handle node failures during a job?**

> Node Manager on a failed node stops sending heartbeats to Resource Manager. RM marks the node as dead and notifies the relevant Application Masters. Each AM requests new containers for its failed tasks and re-runs them. Since MapReduce tasks are deterministic (same input → same output), re-running is safe. The job continues with the failed node's work redistributed.

---

**Q27. What is the difference between Avro, Parquet, and ORC?**

> **Avro**: Row-based, schema embedded in file, ideal for write-heavy workloads and Kafka/streaming. Schema evolution support.
> **Parquet**: Columnar, ideal for analytical queries (Spark, Hive, Presto), high compression. Most widely used in Data Lakes.
> **ORC**: Columnar, optimized for Hive, ACID transaction support, built-in indexes and bloom filters.
> Rule of thumb: Use Parquet for Spark/cross-platform; ORC for Hive-centric workflows; Avro for streaming.

---

**Q28. What are RDDs in Spark? How do they relate to Hadoop?**

> RDDs (Resilient Distributed Datasets) are Spark's fundamental data structure — an immutable, distributed collection of objects. "Resilient" because Spark tracks the lineage of operations (DAG) and can recompute lost partitions. RDDs can be created from HDFS files, making Spark and Hadoop complementary: HDFS for storage, Spark for processing. Unlike MapReduce, Spark caches RDDs in memory for iterative processing.

---

**Q29. What is ZooKeeper? Why is it used in the Hadoop ecosystem?**

> ZooKeeper is a distributed coordination service — think of it as a "traffic cop" for distributed systems. Uses: (1) HDFS HA — ZooKeeper detects NameNode failure and triggers failover to standby, (2) HBase — master election and region server coordination, (3) Kafka — broker election and consumer group coordination. Based on Google's Chubby design. Provides: distributed locks, leader election, configuration management, service discovery.

---

**Q30. Explain the evolution from Hadoop to modern cloud data platforms.**

> **2005-2013 (Hadoop era)**: HDFS + MapReduce on on-premise commodity clusters. Challenges: complex setup, slow MapReduce, operational burden.
> **2013-2018 (Spark era)**: Spark replaced MapReduce (10-100x faster), Hive/Impala for SQL. Clusters still mostly on-premise.
> **2018-2023 (Cloud-native era)**: Storage separated from compute (S3/GCS instead of HDFS), auto-scaling clusters (AWS EMR, Databricks), managed SQL warehouses (Snowflake, BigQuery).
> **2023+ (Lakehouse era)**: Delta Lake/Iceberg bring ACID transactions to Data Lakes, unified batch+streaming (Apache Paimon), AI/ML integrated directly into data platforms.

---

## 🔗 Connected Notes

- [[What is Big Data]] — Foundation answers
- [[HDFS Deep Dive]] — For Q5-Q8, Q12-Q13, Q22-Q24
- [[MapReduce Explained]] — For Q9, Q11, Q15, Q21
- [[YARN Explained]] — For Q10, Q14, Q26
- [[CAP Theorem]] — For Q17
- [[Key Big Data Concepts]] — For Q16, Q18, Q27, Q28
