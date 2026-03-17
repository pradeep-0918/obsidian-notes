# 🗺️ Data Engineering Roadmap

> [!tip] Career Resources 📅 **[Book a Session on Topmate.io](https://topmate.io/)** → Interview Prep · Resume Review · Career Guidance

---

## 🗂️ Tags

`#data-engineering` `#roadmap` `#career` `#big-data` `#cloud`

---

## 📚 Core Skills Overview

```dataview
TABLE skill, category, priority
FROM #data-engineering
SORT priority ASC
```

---

## 1️⃣ SQL

> [!note] Foundation Skill SQL is the backbone of every data role. Master this first.

### Topics to Cover

- **DDL / DML / DCL** — CREATE, ALTER, DROP, INSERT, UPDATE, DELETE
- **Joins** — INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF
- **Window Functions** — ROW_NUMBER, RANK, DENSE_RANK, LAG, LEAD, NTILE
- **CTEs & Subqueries** — WITH clause, correlated subqueries
- **Aggregations** — GROUP BY, HAVING, ROLLUP, CUBE
- **Indexing & Query Optimization** — EXPLAIN plans, execution costs
- **Stored Procedures & Views**
- **Transactions** — ACID properties, COMMIT, ROLLBACK

### Practice Platforms

- LeetCode (SQL section)
- HackerRank SQL
- Mode Analytics SQL Tutorial

---

## 2️⃣ Python / Java / Scala

> [!note] Pick Your Primary Language Python is most common. Scala is essential for Spark internals.

### Python for Data Engineering

- **Libraries** — `pandas`, `numpy`, `pyspark`, `boto3`, `sqlalchemy`
- **File I/O** — CSV, JSON, Parquet, Avro
- **OOP Concepts** — Classes, Inheritance, Decorators
- **Error Handling & Logging**
- **Virtual Environments & Packaging** — `pip`, `poetry`, `conda`

### Scala for Spark

- Functional programming basics
- Case classes, pattern matching
- Spark native API (RDD, Dataset)

### Java (Optional)

- Used in Kafka, Flink, and enterprise ETL tools

---

## 3️⃣ Linux

> [!important] Must-Know for All Data Engineers

### Core Commands

```bash
# File Operations
ls -la | grep ".csv"
find /data -name "*.parquet" -mtime -7
chmod 755 script.sh

# Text Processing
awk '{print $1, $3}' file.txt
sed 's/old/new/g' file.txt
grep -r "ERROR" /logs/

# Process Management
ps aux | grep spark
kill -9 <PID>
nohup python pipeline.py &

# Disk & Memory
df -h
du -sh /data/*
free -m
top / htop
```

### Shell Scripting

- Cron jobs for scheduling
- Environment variables
- Pipe & redirect operators (`|`, `>`, `>>`, `2>&1`)

---

## 4️⃣ Data Modeling & ETL Concepts

> [!abstract] Theoretical Foundation

### Data Modeling

- **Relational Modeling** — 1NF, 2NF, 3NF, BCNF
- **Dimensional Modeling**
    - ⭐ Star Schema
    - ❄️ Snowflake Schema
    - Galaxy Schema
- **Slowly Changing Dimensions (SCD)** — Type 1, 2, 3, 4, 6
- **Fact Tables vs Dimension Tables**
- **Data Vault 2.0** — Hubs, Links, Satellites

### ETL vs ELT

|Aspect|ETL|ELT|
|---|---|---|
|Transform Location|Before Load|After Load|
|Best For|On-prem warehouses|Cloud DWH|
|Tools|Informatica, SSIS|dbt, Spark|

### Key Concepts

- **Idempotency** — Re-running produces same results
- **Incremental vs Full Load**
- **Change Data Capture (CDC)**
- **Data Lineage**
- **Data Quality** — Completeness, Accuracy, Consistency

---

## 5️⃣ Hadoop Ecosystem

> [!info] The OG Big Data Stack

### HDFS (Hadoop Distributed File System)

- NameNode (master) & DataNode (workers)
- Block size (default 128MB)
- Replication factor (default 3)
- Commands: `hdfs dfs -ls / -put / -get / -mkdir`

### Hive

- SQL-like interface on top of HDFS
- HiveQL — DDL, DML, partitioning, bucketing
- Internal vs External tables
- SerDe (Serializer/Deserializer)
- ORC vs Parquet formats

### Sqoop

- Relational DB ↔ HDFS/Hive data transfer
- `sqoop import` / `sqoop export`
- Incremental imports (`--check-column`, `--last-value`)
- Sqoop jobs for scheduling

### Other Hadoop Tools

- **MapReduce** — Map phase, Shuffle, Reduce phase
- **YARN** — Resource manager, Node manager
- **ZooKeeper** — Distributed coordination

---

## 6️⃣ Apache Spark

> [!success] Most In-Demand Big Data Skill

### Spark Architecture

- **Driver** — Orchestrates execution
- **Executors** — Worker nodes
- **DAG Scheduler** — Directed Acyclic Graph
- **Transformations** — Lazy (map, filter, join)
- **Actions** — Trigger execution (collect, count, write)

### Spark Batch Processing

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("BatchJob") \
    .getOrCreate()

df = spark.read.parquet("s3://bucket/data/")
df_filtered = df.filter(df["status"] == "active")
df_filtered.write.mode("overwrite").parquet("s3://bucket/output/")
```

### Spark SQL

```python
df.createOrReplaceTempView("users")
result = spark.sql("""
    SELECT country, COUNT(*) as cnt
    FROM users
    WHERE age > 18
    GROUP BY country
    ORDER BY cnt DESC
""")
```

### Spark Streaming

- **Structured Streaming** — Micro-batch & continuous
- **DStream** (legacy) vs **DataFrame-based** API
- Sources: Kafka, Files, Sockets
- Output modes: Append, Update, Complete
- **Watermarking** — Handling late data

### Spark Optimization

- Partitioning & Repartitioning
- Broadcast joins
- Caching / Persistence levels
- Avoiding shuffles
- Salting for skewed data

---

## 7️⃣ Cloud Data Services

> [!cloud] Multi-Cloud Strategy

### ☁️ AWS

|Service|Purpose|
|---|---|
|S3|Object Storage|
|Glue|ETL + Data Catalog|
|Redshift|Data Warehouse|
|EMR|Managed Spark/Hadoop|
|Kinesis|Real-time Streaming|
|Athena|Serverless SQL on S3|
|Lambda|Serverless Functions|
|Step Functions|Workflow Orchestration|

### ☁️ GCP

|Service|Purpose|
|---|---|
|GCS|Object Storage|
|BigQuery|Serverless DWH|
|Dataflow|Apache Beam (ETL)|
|Dataproc|Managed Spark|
|Pub/Sub|Messaging Queue|
|Composer|Managed Airflow|
|Looker|BI & Analytics|

### ☁️ Azure

|Service|Purpose|
|---|---|
|ADLS Gen2|Data Lake Storage|
|Synapse Analytics|Unified Analytics|
|Data Factory|ETL Pipelines|
|HDInsight|Managed Hadoop|
|Event Hubs|Streaming Ingestion|
|Databricks|Managed Spark|
|Purview|Data Governance|

---

## 8️⃣ Apache Airflow

> [!abstract] Workflow Orchestration

### Core Concepts

- **DAG** (Directed Acyclic Graph) — Defines workflow
- **Operator** — Unit of work (PythonOperator, BashOperator, etc.)
- **Task** — Instance of an Operator
- **Scheduler** — Triggers DAGs based on schedule
- **Executor** — LocalExecutor, CeleryExecutor, KubernetesExecutor

### Basic DAG Example

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def extract(): pass
def transform(): pass
def load(): pass

with DAG('etl_pipeline',
         start_date=datetime(2024, 1, 1),
         schedule_interval='@daily',
         catchup=False) as dag:

    t1 = PythonOperator(task_id='extract', python_callable=extract)
    t2 = PythonOperator(task_id='transform', python_callable=transform)
    t3 = PythonOperator(task_id='load', python_callable=load)

    t1 >> t2 >> t3
```

### Best Practices

- Use XCom for small data passing between tasks
- Idempotent tasks (safe to retry)
- Use Variables and Connections for configs
- TaskFlow API (Airflow 2.0+)
- SLA monitoring & alerting

---

## 9️⃣ Apache Flink

> [!info] Real-Time Stream Processing

### Flink vs Spark Streaming

|Feature|Flink|Spark Streaming|
|---|---|---|
|Processing Model|True Streaming|Micro-batch|
|Latency|Milliseconds|Seconds|
|State Management|Native|Limited|
|Event Time|Strong support|Good support|

### Core Concepts

- **DataStream API** — For streaming jobs
- **Table API / Flink SQL** — Declarative
- **State Backends** — RocksDB, Memory, FileSystem
- **Checkpointing** — Fault tolerance
- **Watermarks** — Event-time handling
- **Windows** — Tumbling, Sliding, Session

---

## 🔟 Databricks

> [!success] Unified Data + AI Platform

### Key Components

- **Delta Lake** — ACID transactions on data lake
- **MLflow** — ML experiment tracking
- **Unity Catalog** — Data governance
- **Databricks SQL** — SQL analytics
- **Notebooks** — Collaborative development
- **Workflows** — Job scheduling

### Delta Lake Features

```python
# Read Delta
df = spark.read.format("delta").load("/mnt/delta/table")

# Write Delta
df.write.format("delta").mode("overwrite").save("/mnt/delta/table")

# Time Travel
df_old = spark.read.format("delta") \
    .option("versionAsOf", 5) \
    .load("/mnt/delta/table")

# Optimize
spark.sql("OPTIMIZE delta.`/mnt/delta/table` ZORDER BY (date)")
```

---

## 1️⃣1️⃣ Snowflake

> [!info] Cloud-Native Data Warehouse

### Architecture

- **Virtual Warehouses** — Compute (can scale independently)
- **Storage Layer** — Columnar storage (S3/Azure/GCS)
- **Cloud Services Layer** — Query optimizer, metadata

### Key Features

- **Time Travel** — Query historical data (up to 90 days)
- **Cloning** — Zero-copy clone of tables/schemas/databases
- **Streams & Tasks** — CDC + scheduling
- **Snowpipe** — Continuous data ingestion
- **Data Sharing** — Share data across accounts

### Performance Tips

- Clustering keys for large tables
- Result caching
- Warehouse sizing & auto-suspend
- Materialized views

---

## 1️⃣2️⃣ Apache Kafka

> [!abstract] Distributed Event Streaming

### Core Concepts

- **Topic** — Named stream of records
- **Partition** — Parallelism unit
- **Broker** — Kafka server
- **Producer** — Writes messages
- **Consumer / Consumer Group** — Reads messages
- **Offset** — Position in partition
- **Zookeeper / KRaft** — Cluster coordination

### Producer & Consumer (Python)

```python
from kafka import KafkaProducer, KafkaConsumer
import json

# Producer
producer = KafkaProducer(
    bootstrap_servers='localhost:9092',
    value_serializer=lambda v: json.dumps(v).encode('utf-8')
)
producer.send('events', {'user': 'alice', 'action': 'click'})

# Consumer
consumer = KafkaConsumer(
    'events',
    bootstrap_servers='localhost:9092',
    value_deserializer=lambda m: json.loads(m.decode('utf-8'))
)
for message in consumer:
    print(message.value)
```

### Key Concepts

- **Kafka Connect** — Source/Sink connectors
- **Kafka Streams** — Stream processing library
- **Schema Registry** — Avro/Protobuf schema management
- **Replication** — `replication.factor` for fault tolerance
- **Retention** — Time-based or size-based

---

## 1️⃣3️⃣ dbt (Data Build Tool)

> [!success] Modern Data Transformation

### Core Concepts

- **Models** — SQL SELECT statements (`.sql` files)
- **Sources** — Raw data definitions
- **Tests** — `not_null`, `unique`, `accepted_values`, `relationships`
- **Macros** — Jinja2 reusable SQL functions
- **Seeds** — CSV lookup tables
- **Snapshots** — SCD Type 2 tracking
- **Lineage** — Auto-generated DAG

### Project Structure

```
dbt_project/
├── models/
│   ├── staging/        # Raw → cleaned
│   ├── intermediate/   # Business logic
│   └── marts/          # Final analytics tables
├── tests/
├── macros/
├── seeds/
├── snapshots/
└── dbt_project.yml
```

### Commands

```bash
dbt run              # Run all models
dbt test             # Run all tests
dbt docs generate    # Generate documentation
dbt docs serve       # View lineage graph
dbt run --select staging.*   # Run specific models
dbt build            # run + test + snapshot + seed
```

---

## 1️⃣4️⃣ Open Table Formats

> [!info] The Future of the Data Lakehouse

### Apache Iceberg

- Schema evolution without rewrite
- **Hidden partitioning** — No partition columns in queries
- Time travel & rollback
- Row-level deletes
- Supported by: Spark, Flink, Trino, Dremio

### Apache Hudi (Hadoop Upserts Deletes Incrementals)

- Upserts & deletes on data lake
- **Copy-On-Write (COW)** vs **Merge-On-Read (MOR)**
- Incremental query support
- Timeline-based versioning
- Best for: CDC use cases, streaming ingestion

### Apache Delta Lake

- ACID transactions on cloud storage
- Schema enforcement & evolution
- Time travel (`VERSION AS OF`, `TIMESTAMP AS OF`)
- Optimistic concurrency control
- Native to Databricks; open-sourced

### Comparison

|Feature|Iceberg|Hudi|Delta Lake|
|---|---|---|---|
|Upserts|✅|✅|✅|
|Time Travel|✅|✅|✅|
|Schema Evolution|✅|✅|✅|
|Streaming|✅|✅|✅|
|Native Platform|Neutral|Neutral|Databricks|

---

## 🎯 Interview Prep Checklist

> [!todo] Before Your Interview

- [ ] SQL — Window functions, complex joins, query optimization
- [ ] System Design — Design a data pipeline for 1TB/day
- [ ] Spark — Explain shuffles, partitioning, optimization
- [ ] Kafka — Exactly-once semantics, consumer lag
- [ ] Cloud — Architect a cost-effective lakehouse on AWS/GCP/Azure
- [ ] Data Modeling — Star schema, SCD Type 2
- [ ] Airflow — DAG design patterns, failure handling
- [ ] dbt — Incremental models, testing strategy

---

## 📌 Topmate.io — Get Personalized Help

> [!success] Don't Navigate This Alone

|Session Type|What You Get|
|---|---|
|🎤 **Interview Prep**|Mock interviews, question banks, feedback|
|📄 **Resume Review**|ATS optimization, project framing|
|🧭 **Career Guidance**|Roadmap planning, skill gap analysis|
|💼 **Job Search Strategy**|LinkedIn, referrals, negotiation|

🔗 **[→ Book a Session on Topmate.io](https://topmate.io/)**

---

## 🔗 Related Notes

- [[System Design for Data Engineers]]
- [[Cloud Cost Optimization]]
- [[Data Engineering Interview Questions]]
- [[SQL Practice Problems]]
- [[Spark Optimization Techniques]]

---

_Last Updated: {{date}}_ _Tags: #data-engineering #roadmap #career #sql #spark #kafka #airflow #dbt #snowflake #databricks_