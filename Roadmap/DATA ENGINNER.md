# 🗺️ Data Engineering Roadmap 
> **Goal:** Land a Data Engineering role with GenAI skills | Built for Obsidian

#data-engineering #roadmap #career #genai #big-data #cloud

---

## 📊 Roadmap At A Glance

| Phase | Duration | Focus | Resume Project |
|-------|----------|-------|---------------|
| [[#Phase 1 — Foundations]] | Weeks 1–4 | SQL · Python · Linux | Multi-source ETL CLI |
| [[#Phase 2 — Big Data Core]] | Weeks 5–10 | Spark · Airflow · Data Modeling | Airflow + Spark Lakehouse Pipeline |
| [[#Phase 3 — Cloud & Warehousing]] | Weeks 11–16 | AWS/Azure · Snowflake · dbt | Production Cloud Data Platform |
| [[#Phase 4 — Streaming & Real-Time]] | Weeks 17–20 | Kafka · Flink · CDC | Real-Time Event Processing System |
| [[#Phase 5 — GenAI Data Engineering]] | Weeks 21–24 | RAG · Vector DBs · LLM Pipelines | AI-Powered Data Product |

---

## ⚡ Daily Practice Routine

```
🌅 Morning (30 min) — LeetCode SQL / HackerRank Python
🏗️ Afternoon (2–3 hrs) — Phase project / topic deep dive
🌙 Evening (30 min) — Review notes, update this roadmap
```

---

## Phase 1 — Foundations
> **Weeks 1–4** | Goal: Be dangerous with SQL, Python, and the terminal.

### 🎯 Weekly Goals
- **Week 1:** SQL Joins, Aggregations, Window Functions
- **Week 2:** SQL Optimization + Python File I/O + OOP Refresher
- **Week 3:** Linux Shell Scripting + Python ETL scripting
- **Week 4:** Resume Project Sprint

---

### 🗄️ SQL — [[SQL]]

#### Core Topics
- [ ] DDL / DML / DCL (CREATE, ALTER, DROP, INSERT, UPDATE, DELETE)
- [ ] JOINs — INNER, LEFT, RIGHT, FULL OUTER, CROSS, SELF JOIN
- [ ] Window Functions — ROW_NUMBER, RANK, DENSE_RANK, LAG, LEAD, NTILE
- [ ] CTEs & Subqueries — WITH clause, correlated subqueries
- [ ] Aggregations — GROUP BY, HAVING, ROLLUP, CUBE
- [ ] EXPLAIN plans & query cost analysis
- [ ] Indexing strategies — B-Tree, composite indexes, covering indexes
- [ ] Stored Procedures, Views, Materialized Views
- [ ] Transactions — ACID, COMMIT, ROLLBACK, Isolation Levels
- [ ] Recursive CTEs (hierarchical data problems)

#### Practice
- [ ] Solve 30 LeetCode SQL problems (Easy → Medium → Hard)
- [ ] Complete HackerRank SQL track (all sections)
- [ ] Practice EXPLAIN on real queries in PostgreSQL

---

### 🐍 Python for Data Engineering — [[Python]]

#### Core Topics
- [ ] File I/O — CSV, JSON, Parquet, Avro with `pandas` and `pyarrow`
- [ ] OOP — Classes, Inheritance, Abstract Base Classes, Decorators
- [ ] Error Handling — try/except, custom exceptions, retry logic
- [ ] Logging — structured logging with `loguru` or built-in `logging`
- [ ] Type hints and dataclasses
- [ ] Concurrency — `asyncio`, `ThreadPoolExecutor` for I/O-bound tasks
- [ ] Virtual Environments — `poetry`, `pip`, `conda`
- [ ] Testing — `pytest`, unit tests for ETL functions
- [ ] Key Libraries: `pandas`, `numpy`, `boto3`, `sqlalchemy`, `httpx`, `pydantic`

#### Practice
- [ ] Build a CSV → Parquet converter with schema validation
- [ ] Write a reusable `ETLBase` class with logging and retry
- [ ] Practice writing `pytest` unit tests for a data cleaning function

---

### 🐧 Linux & Shell — [[Linux]]

#### Core Commands to Master
- [ ] File ops: `ls`, `find`, `chmod`, `chown`, `ln`, `rsync`
- [ ] Text processing: `awk`, `sed`, `grep`, `cut`, `sort`, `uniq`, `wc`
- [ ] Process management: `ps`, `kill`, `nohup`, `screen`, `tmux`
- [ ] Disk & memory: `df`, `du`, `free`, `top`, `iotop`
- [ ] Networking: `curl`, `wget`, `netstat`, `ssh`, `scp`
- [ ] Piping and redirection: `|`, `>`, `>>`, `2>&1`, `tee`

#### Shell Scripting
- [ ] Write a cron job to automate a daily data download
- [ ] Build a bash script that monitors a log file and sends alerts
- [ ] Parameterize scripts using `$1`, `$@`, `getopts`
- [ ] Understand exit codes and error trapping (`set -e`, `trap`)

---

### 🏆 Phase 1 Resume Project: Multi-Source ETL CLI Tool
> **What:** A command-line Python tool that extracts data from 2+ sources (REST API + CSV files), cleans/transforms it, and loads it into a PostgreSQL database.

**Must-haves:**
- [ ] Modular code with `src/` layout
- [ ] Argparse CLI (`--source`, `--date`, `--dry-run`)
- [ ] Structured logging to file + stdout
- [ ] Schema validation with `pydantic`
- [ ] `pytest` test suite with mocked API calls
- [ ] `README.md` with architecture diagram and usage examples
- [ ] GitHub repo with clean commit history

---

## Phase 2 — Big Data Core
> **Weeks 5–10** | Goal: Build and run real distributed data pipelines.

### 🎯 Weekly Goals
- **Week 5:** Spark architecture, RDDs, DataFrames basics
- **Week 6:** Spark SQL, joins, aggregations, writing to Parquet/Delta
- **Week 7:** Spark Streaming + Structured Streaming basics
- **Week 8:** Airflow DAGs — operators, dependencies, XCom
- **Week 9:** Data Modeling — Star/Snowflake schemas, SCDs
- **Week 10:** Resume Project Sprint

---

### ⚡ Apache Spark — [[DataEngineering_Obsidian_Vault/Apache Spark]]

#### Architecture
- [ ] Driver vs Executor model
- [ ] DAG Scheduler, Stage, Task lifecycle
- [ ] Lazy evaluation — transformations vs actions
- [ ] Catalyst Optimizer and Tungsten execution engine

#### Batch Processing
- [ ] Read/write: Parquet, Delta, JSON, CSV, ORC
- [ ] DataFrame API — `select`, `filter`, `groupBy`, `agg`, `join`
- [ ] Spark SQL — `createOrReplaceTempView`, complex SQL in Spark
- [ ] UDFs — when to use and when NOT to use them
- [ ] Schema enforcement and evolution

#### Spark Optimization (Critical for Interviews!)
- [ ] Partitioning — `repartition` vs `coalesce`
- [ ] Broadcast joins — when and how
- [ ] Caching / Persistence levels (`MEMORY_AND_DISK`, etc.)
- [ ] Shuffle optimization — avoiding wide transformations
- [ ] Salting for skewed data
- [ ] AQE (Adaptive Query Execution) in Spark 3.x
- [ ] Reading Spark UI — stages, tasks, GC time, spill

#### Spark Streaming — [[Structured Streaming]]
- [ ] Structured Streaming — micro-batch vs continuous mode
- [ ] Sources: Kafka, file directory, rate source (for testing)
- [ ] Output modes: Append, Update, Complete
- [ ] Watermarking for late data
- [ ] Checkpointing for fault tolerance

#### Practice
- [ ] Run Spark locally with `pyspark` shell
- [ ] Join two large DataFrames and check the Spark UI for shuffle details
- [ ] Optimize a slow query using broadcast join and EXPLAIN

---

### 🔀 Apache Airflow — [[Apache Airflow]]

#### Core Concepts
- [ ] DAG lifecycle — scheduling, catchup, `start_date`
- [ ] Operators — PythonOperator, BashOperator, sensors, HTTP
- [ ] Task dependencies — `>>`, `<<`, `set_downstream`
- [ ] XCom — passing small data between tasks
- [ ] Variables and Connections — configuration management
- [ ] TaskFlow API (Airflow 2.0+) with `@task` decorators
- [ ] Executors — LocalExecutor, CeleryExecutor, KubernetesExecutor
- [ ] SLAs, retries, `on_failure_callback`, alerting

#### Best Practices
- [ ] Idempotent tasks — safe to re-run without side effects
- [ ] Dynamic DAGs — `dag_factory` patterns
- [ ] Testing DAGs locally with `airflow dags test`
- [ ] DAG documentation with `doc_md`

---

### 📐 Data Modeling — [[Data Modeling]]

#### Relational Modeling
- [ ] 1NF, 2NF, 3NF, BCNF normalization
- [ ] When to denormalize (OLAP vs OLTP)

#### Dimensional Modeling
- [ ] ⭐ Star Schema — fact tables + dimension tables
- [ ] ❄️ Snowflake Schema — normalized dimensions
- [ ] Galaxy / Constellation Schema — multiple fact tables
- [ ] Slowly Changing Dimensions (SCD)
  - [ ] Type 1 — Overwrite
  - [ ] Type 2 — Add new row (most common)
  - [ ] Type 3 — Add column
  - [ ] Type 6 — Hybrid

#### Advanced Concepts
- [ ] Data Vault 2.0 — Hubs, Links, Satellites
- [ ] Idempotency and incremental load patterns
- [ ] Change Data Capture (CDC) — log-based vs trigger-based
- [ ] Data Lineage tracking

---

### 🏆 Phase 2 Resume Project: Airflow + Spark Lakehouse Pipeline
> **What:** An end-to-end batch pipeline using Airflow to orchestrate Spark jobs that process raw data into a Delta Lake–backed dimensional model.

**Must-haves:**
- [ ] Airflow DAG with 4+ tasks: ingest → validate → transform → load
- [ ] PySpark job that reads raw JSON/CSV and writes to Delta format
- [ ] Star schema output (1 fact table, 2+ dimension tables)
- [ ] SCD Type 2 logic for at least one dimension
- [ ] Data quality checks (null checks, row count assertions)
- [ ] Docker Compose setup so it runs locally
- [ ] GitHub repo with architecture diagram

---

## Phase 3 — Cloud & Warehousing
> **Weeks 11–16** | Goal: Build production-grade pipelines on real cloud infrastructure.

### 🎯 Weekly Goals
- **Week 11:** AWS core services (S3, IAM, Glue, Athena)
- **Week 12:** AWS EMR + Redshift or Azure Synapse
- **Week 13:** Snowflake architecture, features, performance
- **Week 14:** dbt — models, tests, docs, incremental models
- **Week 15:** Databricks — Delta Lake, Unity Catalog, Workflows
- **Week 16:** Resume Project Sprint

---

### ☁️ AWS for Data Engineering — [[AWS]]

#### Storage & Compute
- [ ] S3 — storage classes, lifecycle policies, partitioning strategy
- [ ] IAM — roles, policies, cross-account access, least privilege
- [ ] EC2 basics — for running custom jobs

#### Data Services
- [ ] Glue — ETL jobs (Spark), Data Catalog, crawlers, classifiers
- [ ] Athena — serverless SQL on S3, partitioned tables, CTAS
- [ ] Redshift — architecture, distribution styles, sort keys, WLM
- [ ] EMR — cluster types, spot instances, cost optimization
- [ ] Kinesis Data Streams + Firehose — streaming ingestion
- [ ] Lambda — event-driven triggers for pipelines
- [ ] Step Functions — pipeline orchestration alternative to Airflow
- [ ] Secrets Manager / Parameter Store — credential management

#### Practice
- [ ] Create an S3 → Glue → Athena serverless query pipeline
- [ ] Set up a Redshift cluster and run EXPLAIN on a heavy query
- [ ] Build a Lambda that triggers on S3 upload

---

### ❄️ Snowflake — [[Snowflake]]

#### Architecture
- [ ] Virtual Warehouses (compute) vs Storage Layer vs Cloud Services
- [ ] Multi-cluster warehouses for concurrency
- [ ] Micro-partitioning and clustering

#### Key Features
- [ ] Time Travel — query historical data up to 90 days
- [ ] Zero-copy cloning — tables, schemas, databases
- [ ] Streams — CDC on tables
- [ ] Tasks — scheduled SQL execution
- [ ] Snowpipe — continuous data ingestion from S3/Azure
- [ ] Data Sharing — cross-account data sharing without copy

#### Performance
- [ ] Clustering keys for large tables (when NOT to use them)
- [ ] Result caching — query result reuse
- [ ] Warehouse sizing and auto-suspend/resume
- [ ] Materialized views — trade-offs

---

### 🔧 dbt (Data Build Tool) — [[dbt]]

#### Core Concepts
- [ ] Models — SQL SELECT files, materialization types
  - [ ] `table` vs `view` vs `incremental` vs `ephemeral`
- [ ] Sources — declaring raw tables with freshness checks
- [ ] Tests — `not_null`, `unique`, `accepted_values`, `relationships`
- [ ] Macros — Jinja2 reusable SQL
- [ ] Seeds — CSV reference data
- [ ] Snapshots — SCD Type 2 automated
- [ ] Lineage DAG — auto-generated documentation

#### Project Structure Best Practices
```
models/
  staging/      ← 1:1 with raw sources, light cleaning
  intermediate/ ← business logic, joins
  marts/        ← final analytics tables (fact/dim)
```

- [ ] Incremental models with `is_incremental()` macro
- [ ] dbt tests at scale — custom generic tests
- [ ] `dbt build` in CI/CD (GitHub Actions)
- [ ] dbt Cloud vs dbt Core — when to use which

---

### 🧱 Databricks — [[Databricks]]

#### Platform
- [ ] Workspace, clusters (interactive vs job clusters)
- [ ] Unity Catalog — data governance, row/column security
- [ ] Databricks Workflows — multi-task job orchestration
- [ ] Delta Live Tables (DLT) — declarative pipeline framework

#### Delta Lake — [[Delta Lake]]
- [ ] ACID transactions on cloud storage
- [ ] `MERGE INTO` for upserts
- [ ] Time Travel — `VERSION AS OF`, `TIMESTAMP AS OF`
- [ ] `OPTIMIZE` and `ZORDER BY` for file compaction
- [ ] Schema enforcement vs schema evolution
- [ ] Change Data Feed (CDF)
- [ ] Liquid Clustering (Delta 3.x)

---

### 📊 Open Table Formats — [[Open Table Formats]]
- [ ] **Apache Iceberg** — hidden partitioning, row-level deletes, engine-neutral
- [ ] **Apache Hudi** — upserts, COW vs MOR, streaming ingestion
- [ ] **Delta Lake** — Databricks native, ACID, time travel
- [ ] Compare: when to use Iceberg vs Hudi vs Delta

---

### 🏆 Phase 3 Resume Project: Production Cloud Data Platform
> **What:** A full lakehouse on AWS (or Azure) — raw data lands in S3/ADLS → transformed by Glue/Spark → modeled with dbt in Snowflake → dashboarded in a BI tool.

**Must-haves:**
- [ ] IaC setup with Terraform (at minimum S3 + IAM + Glue)
- [ ] dbt project with staging/intermediate/marts layers
- [ ] Snowflake as the warehouse target with clustering
- [ ] CI/CD with GitHub Actions running `dbt test` on PRs
- [ ] Cost monitoring — at least track Snowflake credit usage
- [ ] Architecture diagram (draw.io or Excalidraw)

---

## Phase 4 — Streaming & Real-Time
> **Weeks 17–20** | Goal: Build low-latency pipelines that process events as they happen.

### 🎯 Weekly Goals
- **Week 17:** Kafka fundamentals, producers, consumers
- **Week 18:** Kafka Connect, Schema Registry, Kafka Streams
- **Week 19:** Flink or Spark Structured Streaming — stateful processing
- **Week 20:** Resume Project Sprint

---

### 📨 Apache Kafka — [[Apache Kafka]]

#### Core Concepts
- [ ] Topics, Partitions, Offsets, Retention policies
- [ ] Producers — `acks`, `linger.ms`, `batch.size`, compression
- [ ] Consumers — consumer groups, partition assignment, `auto.offset.reset`
- [ ] Brokers, Leaders, ISR (In-Sync Replicas)
- [ ] ZooKeeper vs KRaft (Kafka without ZooKeeper)

#### Advanced
- [ ] Kafka Connect — source/sink connectors (Debezium, S3 Sink)
- [ ] Schema Registry — Avro/Protobuf schema management
- [ ] Kafka Streams — lightweight stream processing in Java
- [ ] Exactly-once semantics (EOS) — idempotent producers, transactions
- [ ] Consumer lag monitoring with consumer group metrics
- [ ] Compacted topics for changelog / CDC patterns

#### Practice
- [ ] Run Kafka locally with Docker Compose
- [ ] Produce 1M events and measure consumer throughput
- [ ] Set up a Debezium connector for Postgres CDC

---

### 🌊 Apache Flink — [[Apache Flink]]

#### Core Concepts
- [ ] True streaming (event-by-event) vs Spark micro-batch
- [ ] DataStream API vs Table API / Flink SQL
- [ ] State backends — RocksDB (production), HashMapStateBackend
- [ ] Checkpointing — exactly-once fault tolerance
- [ ] Watermarks and event-time processing
- [ ] Windows — Tumbling, Sliding, Session, Global

#### Practice
- [ ] Build a windowed aggregation job (count events per 1-min tumbling window)
- [ ] Implement a stateful sessionization job
- [ ] Connect Flink to a Kafka source

---

### 🏆 Phase 4 Resume Project: Real-Time Event Processing System
> **What:** An end-to-end streaming pipeline — events flow from Kafka → Flink/Spark Streaming → Delta Lake → real-time dashboard (Grafana or Streamlit).

**Must-haves:**
- [ ] Kafka producer simulating e-commerce clickstream events
- [ ] Debezium CDC capturing changes from a Postgres orders table
- [ ] Flink or Structured Streaming job doing windowed aggregation
- [ ] Output to Delta Lake with sub-minute latency
- [ ] Grafana dashboard showing real-time event counts
- [ ] Dead-letter queue for malformed messages
- [ ] Docker Compose to spin up everything locally

---

## Phase 5 — GenAI Data Engineering
> **Weeks 21–24** | Goal: Build AI-powered data products — the skill that sets you apart.

### 🎯 Weekly Goals
- **Week 21:** LLM APIs, prompt engineering, LangChain basics
- **Week 22:** RAG architecture, vector databases, embeddings
- **Week 23:** LLM pipelines as data engineering problems (chunking, ingestion, evaluation)
- **Week 24:** Resume Project Sprint

---

### 🤖 LLM Fundamentals — [[LLMs]]
- [ ] LLM API usage — OpenAI, Anthropic Claude, Cohere
- [ ] Prompt engineering — system prompts, few-shot, chain-of-thought
- [ ] Token limits, context windows, cost estimation
- [ ] Structured output — JSON mode, function calling
- [ ] LangChain / LlamaIndex — chains, agents, tools

---

### 🧠 RAG (Retrieval-Augmented Generation) — [[RAG]]

#### Architecture
- [ ] Chunking strategies — fixed, recursive, semantic chunking
- [ ] Embedding models — `text-embedding-ada-002`, `bge-m3`, `nomic-embed`
- [ ] Vector databases — Pinecone, Weaviate, Chroma, pgvector, Qdrant
- [ ] Retrieval strategies — cosine similarity, MMR, hybrid search (BM25 + dense)
- [ ] Re-ranking — `cross-encoder` models (Cohere Rerank, BGE Reranker)
- [ ] Query rewriting and HyDE (Hypothetical Document Embeddings)

#### Data Engineering for RAG
- [ ] Building document ingestion pipelines (PDF, HTML, DOCX → chunks)
- [ ] Metadata filtering in vector search
- [ ] Embedding freshness — when to re-embed updated documents
- [ ] Evaluation — Ragas framework (context recall, faithfulness, answer relevancy)

---

### 🗃️ Vector Databases — [[Vector Databases]]
- [ ] **Pinecone** — managed, serverless, production-grade
- [ ] **Weaviate** — open-source, hybrid search built-in
- [ ] **Chroma** — local dev and testing
- [ ] **pgvector** — Postgres extension, great for existing PG stacks
- [ ] **Qdrant** — Rust-based, high performance
- [ ] HNSW index vs IVFFlat — trade-offs in recall vs speed

---

### 🔗 LLM-Powered Pipeline Patterns
- [ ] ETL with LLM enrichment (classify, tag, summarize records)
- [ ] Unstructured → Structured (extract JSON from free-text)
- [ ] Async batch processing with `asyncio` + LLM APIs (rate limiting, retries)
- [ ] LLM output validation with Pydantic / Instructor
- [ ] Guardrails — input/output filtering

---

### 🏆 Phase 5 Resume Project: AI-Powered Data Product
> **What:** A production-style RAG application where documents are ingested by a data pipeline, embedded and stored in a vector DB, and queried via a LLM-powered API.

**Must-haves:**
- [ ] Airflow DAG that ingests new documents daily (PDF/web scraping)
- [ ] Chunking + embedding pipeline with `LangChain` or `LlamaIndex`
- [ ] Vector DB (Pinecone or pgvector) as the retrieval layer
- [ ] FastAPI backend with `/chat` endpoint
- [ ] Ragas evaluation pipeline — tracked in MLflow or a simple CSV
- [ ] A simple Streamlit or Gradio UI
- [ ] Deployed to AWS Lambda or a small EC2 / Fly.io instance

---

## 🎯 Interview Prep Checklist — [[Interview Prep]]

### SQL
- [ ] Window functions — write ROW_NUMBER + PARTITION from memory
- [ ] Write a recursive CTE for org hierarchy
- [ ] Optimize a slow query given an EXPLAIN plan
- [ ] Explain B-Tree index vs composite index vs covering index

### System Design
- [ ] Design a pipeline for 1TB/day batch ingestion
- [ ] Design a real-time fraud detection system
- [ ] Design a data lakehouse from scratch (storage → compute → serving)
- [ ] Explain CAP theorem and its relevance to distributed data systems

### Spark
- [ ] Explain the DAG execution model end-to-end
- [ ] When does a shuffle happen? How do you avoid it?
- [ ] Explain broadcast join and when you'd use it
- [ ] What is AQE and how does it help?

### Kafka
- [ ] Exactly-once semantics — how does Kafka achieve it?
- [ ] What is consumer lag and how do you monitor and fix it?
- [ ] Explain the role of Schema Registry

### Cloud
- [ ] Architect a cost-effective lakehouse on AWS
- [ ] When would you use Glue vs EMR vs Lambda?
- [ ] Explain Redshift distribution styles (KEY, ALL, EVEN)

### Data Modeling
- [ ] Difference between Star and Snowflake schema
- [ ] Implement SCD Type 2 with dbt snapshots
- [ ] When would you use Data Vault over dimensional modeling?

### dbt
- [ ] Difference between `table`, `view`, `incremental` materializations
- [ ] How do you test data quality in dbt?
- [ ] What is the `is_incremental()` macro and how does it work?

---

## 🔗 Linked Notes
- [[SQL Practice Problems]]
- [[Spark Optimization Techniques]]
- [[System Design for Data Engineers]]
- [[Cloud Cost Optimization]]
- [[Data Engineering Interview Questions]]
- [[RAG Architecture Patterns]]
- [[dbt Best Practices]]

---

## 📌 Next Actions
> Update this section every Sunday evening.

- [ ] **This week's focus:** ___________________________
- [ ] **One LeetCode SQL problem per day** — log solutions in [[SQL Practice Log]]
- [ ] **One commit per day** — keep GitHub green
- [ ] **Weekly review:** What did I build? What confused me? What's next?
- [ ] **Book a mock interview** after Phase 2 and Phase 4 complete

---

_Last Updated: `{{date}}`_
_Tags: #data-engineering #roadmap #career #sql #spark #kafka #airflow #dbt #snowflake #databricks #genai #rag_