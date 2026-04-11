# 🚀 Projects for Data Engineering

> **Level:** All Levels | **Stage:** 5 of 5
> Portfolio projects are your *proof of skill*. These 5 projects cover the full DE spectrum and are designed to impress interviewers.

---

## 🤔 Why Projects Matter

- Interviewers want to see **real work**, not just theory
- Projects demonstrate you can handle **ambiguity** and **real data**
- Your GitHub repo is your resume as a Data Engineer

> **Strategy:** Pick 2-3 projects, do them thoroughly, and document them well. Depth beats breadth.

---

## 🛠️ Project 1 — Batch ETL Pipeline (Beginner)

**Title:** `Public Data ETL Pipeline`

**Goal:** Build a complete batch pipeline from a public API to a local database.

**Stack:** Python · Pandas · PostgreSQL · Airflow (or cron)

**Steps:**
1. Extract data from a public API (e.g., Open Notify ISS API, CoinGecko, OpenWeatherMap)
2. Clean and transform with Pandas
3. Load into PostgreSQL using SQLAlchemy
4. Schedule with Airflow or a cron job
5. Create a simple SQL report from the loaded data

**What to show:**
- Python script with logging and error handling
- Idempotent loading (no duplicates on re-run)
- Airflow DAG with retry logic
- GitHub repo with README

**Key concepts demonstrated:** [[ETL vs ELT]] · [[Python for Data Engineering]] · [[Data Pipelines]] · [[Data-Engineering-Vault/DataEngineering_Obsidian_Vault/Airflow]]

---

## 🛠️ Project 2 — Data Warehouse + dbt (Intermediate)

**Title:** `E-commerce Analytics Warehouse`

**Goal:** Design a star schema warehouse and build dbt models on top of it.

**Stack:** Python · dbt · PostgreSQL or DuckDB · (optional: Metabase for visualization)

**Dataset:** Kaggle's [Brazilian E-commerce (Olist)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) — free, realistic, multi-table

**Steps:**
1. Load raw CSV files into PostgreSQL (staging tables)
2. Design a star schema: `fact_orders`, `dim_customers`, `dim_products`, `dim_dates`
3. Write dbt models to transform raw → staging → marts
4. Add dbt tests (not_null, unique, relationships)
5. Create a simple dashboard in Metabase or export to CSV for charts

**What to show:**
- Clean dbt project structure (`models/staging/`, `models/marts/`)
- dbt tests passing
- Star schema diagram (use dbdiagram.io)
- GitHub repo with dbt docs

**Key concepts demonstrated:** [[Data Modeling]] · [[Data Warehousing]] · [[SQL for Data Engineering]] · [[ETL vs ELT]]

---

## 🛠️ Project 3 — Cloud Data Lake (Intermediate)

**Title:** `S3 Data Lake with Athena`

**Goal:** Build a multi-layer data lake on AWS (or simulate locally with MinIO).

**Stack:** Python · AWS S3 (or MinIO) · AWS Glue Catalog · AWS Athena · Parquet

**Steps:**
1. Ingest raw data (CSV/JSON) to S3 Bronze layer
2. Process with Python/Pandas or PySpark → write Parquet to Silver layer
3. Register tables in AWS Glue Catalog
4. Query with Athena (SQL on S3)
5. Automate with Airflow or AWS Step Functions

**What to show:**
- Bronze / Silver / Gold folder structure in S3
- Parquet files with partitioning (`year=2024/month=01/`)
- Athena query showing performance with partition pruning
- Cost estimate for the pipeline

**Key concepts demonstrated:** [[Data Lakes vs Data Warehouses]] · [[Cloud for Data Engineering]] · [[Data-Engineering-Vault/DataEngineering_Obsidian_Vault/Apache Spark]]

---

## 🛠️ Project 4 — Streaming Pipeline with Kafka (Advanced)

**Title:** `Real-Time Event Processing Pipeline`

**Goal:** Simulate a real-time clickstream and process events as they arrive.

**Stack:** Python · Apache Kafka (Docker) · Spark Structured Streaming or Python consumer · PostgreSQL

**Steps:**
1. Write a **Kafka Producer** that generates fake user events (clicks, purchases) every second
2. Write a **Kafka Consumer** that reads events and aggregates them (e.g., events per user per minute)
3. Store aggregated results in PostgreSQL
4. Add a simple dashboard or log output showing real-time counts

**What to show:**
- Docker Compose with Kafka + Zookeeper
- Producer and consumer scripts
- Real-time console output showing processed events
- GitHub repo with clear README and architecture diagram

**Key concepts demonstrated:** [[Apache Kafka]] · [[Data Pipelines]] · [[Data-Engineering-Vault/DataEngineering_Obsidian_Vault/Apache Spark]]

---

## 🛠️ Project 5 — End-to-End Lakehouse (Advanced / Capstone)

**Title:** `Full Lakehouse Pipeline — From Ingestion to Dashboard`

**Goal:** Combine everything into a production-style architecture.

**Stack:** Python · Airflow · Apache Spark (or dbt) · Delta Lake or Snowflake · Metabase or Superset

**Architecture:**
```
[Public API / Kaggle Dataset]
        ↓ (Python ingestion)
[S3 / Local — Bronze Layer]
        ↓ (Spark or dbt)
[Silver Layer — cleaned Parquet/Delta]
        ↓ (dbt models)
[Gold Layer — star schema in Snowflake or DuckDB]
        ↓
[Metabase Dashboard]

Orchestrated by: Airflow
Monitored by: Airflow UI + custom alerts
```

**What to show:**
- End-to-end working pipeline
- Architecture diagram
- Data quality checks at each layer
- Airflow DAG with all stages
- Dashboard with 3+ meaningful charts

**Key concepts demonstrated:** ALL topics in this vault

---

## 📋 Project Checklist (For Each Project)

- [ ] Clean GitHub repo with descriptive README
- [ ] Architecture diagram
- [ ] Requirements.txt / environment setup instructions
- [ ] Logging and error handling
- [ ] At least one data quality check
- [ ] Sample output / screenshots in README
- [ ] What you learned / what you'd improve next

---

## 🔗 Related Notes

- [[Data Engineering Roadmap]] — See where projects fit in your learning
- [[Interview Preparation]] — Projects are your best interview talking points
- All technical notes for implementation guidance

---

## ➡️ Next Step

[[Interview Preparation]] — Turn your project experience into compelling interview answers and prepare for technical questions.
