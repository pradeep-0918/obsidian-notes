# 🗄️ SQL Intro

> [!info] About This Note A complete introduction to SQL — its origin, purpose, and a full roadmap to becoming a **Data Engineer**.

---

## 📌 Tags

`#sql` `#database` `#data-engineering` `#roadmap` `#beginner`

---

## 🏛️ Origin of SQL

- **Full Name:** Structured Query Language
- **Born:** Early 1970s at **IBM Research** (San Jose, California)
- **Inventors:** **Donald D. Chamberlin** and **Raymond F. Boyce**
- **Original Name:** SEQUEL _(Structured English Query Language)_ — later shortened to **SQL** due to a trademark conflict
- **Inspired by:** Edgar F. Codd's landmark 1970 paper — _"A Relational Model of Data for Large Shared Data Banks"_
- **First Commercial DB:** Oracle (1979), IBM DB2, and later Microsoft SQL Server
- **Standardized:** ANSI standard in **1986**, ISO in **1987**

> [!quote] Fun Fact SQL has been around for **50+ years** and is still the #1 language used for data. It outlasted dozens of competitors.

---

## 🎯 Purpose of SQL

SQL is designed to **communicate with relational databases**. It allows you to:

|Purpose|What it does|
|---|---|
|**Query Data**|Retrieve rows/columns from tables|
|**Manipulate Data**|Insert, update, delete records|
|**Define Structure**|Create and alter tables, schemas|
|**Control Access**|Grant/revoke permissions to users|
|**Aggregate & Analyze**|Group, count, sum, average data|

### Why SQL Still Dominates

- Human-readable, English-like syntax
- Works across almost every database (MySQL, PostgreSQL, BigQuery, Snowflake, etc.)
- Foundation of all data tools — dashboards, pipelines, ML feature stores
- Scales from hobby projects to petabyte-scale data warehouses

---

## 🗺️ Data Engineer Roadmap

> [!tip] How to use this roadmap Follow the phases in order. Don't skip Phase 1 even if you know some SQL — the depth matters.

---

### ✅ Phase 0 — Foundations (Before SQL)

- [ ] Understand how computers store data (files, memory, disk)
- [ ] Learn basic **Linux/Terminal** commands (`ls`, `cd`, `grep`, `pipe`)
- [ ] Understand what a **relational database** is vs a spreadsheet
- [ ] Install **PostgreSQL** or use a free cloud DB (Supabase, Neon)

---

### ✅ Phase 1 — Core SQL (4–6 weeks)

#### 1.1 Basic Queries

- [x] `SELECT`, `FROM`, `WHERE`
- [ ] `ORDER BY`, `LIMIT`, `DISTINCT`
- [ ] Filtering with `AND`, `OR`, `NOT`, `IN`, `BETWEEN`, `LIKE`

#### 1.2 Aggregations

- [ ] `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`
- [ ] `GROUP BY`, `HAVING`

#### 1.3 Joins

- [ ] `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `FULL OUTER JOIN`
- [ ] Self joins, Cross joins
- [ ] Understanding foreign keys and relationships

#### 1.4 Data Manipulation (DML)

- [ ] `INSERT INTO`, `UPDATE`, `DELETE`
- [ ] Transactions — `COMMIT`, `ROLLBACK`

#### 1.5 Data Definition (DDL)

- [ ] `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`
- [ ] Data types: `INT`, `VARCHAR`, `TEXT`, `DATE`, `BOOLEAN`, `NUMERIC`
- [ ] Constraints: `PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, `UNIQUE`, `DEFAULT`

#### 1.6 Subqueries & CTEs

- [ ] Subqueries in `WHERE`, `FROM`, `SELECT`
- [ ] Common Table Expressions — `WITH cte AS (...)`
- [ ] Recursive CTEs

#### 1.7 Window Functions ⭐ _(critical for DE)_

- [ ] `ROW_NUMBER()`, `RANK()`, `DENSE_RANK()`
- [ ] `LAG()`, `LEAD()`
- [ ] `SUM() OVER (PARTITION BY ...)`
- [ ] `NTILE()`, `PERCENT_RANK()`

---

### ✅ Phase 2 — Database Internals (2–4 weeks)

- [ ] How indexes work (B-tree, Hash)
- [ ] `EXPLAIN` / `EXPLAIN ANALYZE` — query execution plans
- [ ] Query optimization basics
- [ ] Normalization — 1NF, 2NF, 3NF
- [ ] ACID properties — Atomicity, Consistency, Isolation, Durability
- [ ] Views, Materialized Views
- [ ] Stored Procedures & Functions

---

### ✅ Phase 3 — Python for Data Engineering (4–6 weeks)

- [ ] Python basics — loops, functions, dictionaries, list comprehensions
- [ ] **pandas** — read, transform, filter DataFrames
- [ ] **SQLAlchemy** — connect Python to databases
- [ ] **psycopg2** — raw PostgreSQL driver
- [ ] Read/write CSV, JSON, Parquet files
- [ ] Error handling, logging

---

### ✅ Phase 4 — Data Warehousing (3–4 weeks)

- [ ] OLTP vs OLAP — what's the difference?
- [ ] Star Schema vs Snowflake Schema
- [ ] Fact tables and Dimension tables
- [ ] Slowly Changing Dimensions (SCD Type 1, 2, 3)
- [ ] Cloud Data Warehouses:
    - [ ] **BigQuery** (Google)
    - [ ] **Snowflake**
    - [ ] **Amazon Redshift**
    - [ ] **Databricks SQL**

---

### ✅ Phase 5 — ETL / ELT Pipelines (4–6 weeks)

- [ ] What is ETL vs ELT?
- [ ] **dbt (data build tool)** — transform data in the warehouse
    - [ ] Models, Tests, Documentation
    - [ ] `ref()`, `source()`, incremental models
- [ ] **Apache Airflow** — orchestrate pipeline schedules
    - [ ] DAGs, Operators, Sensors, XCom
- [ ] Pipeline design patterns — idempotency, backfill, partitioning

---

### ✅ Phase 6 — Big Data & Distributed Systems (4–6 weeks)

- [ ] Understand distributed computing concepts (partitioning, shuffling)
- [ ] **Apache Spark** (PySpark)
    - [ ] RDDs, DataFrames, Spark SQL
    - [ ] Transformations vs Actions
    - [ ] Reading Parquet, Delta Lake
- [ ] **Apache Kafka** — real-time streaming basics
    - [ ] Topics, Producers, Consumers
    - [ ] Kafka Connect, Kafka Streams
- [ ] Batch vs Streaming pipelines

---

### ✅ Phase 7 — Cloud & Infrastructure (3–4 weeks)

- [ ] Pick one cloud — **AWS**, **GCP**, or **Azure**
- [ ] **AWS** path: S3, Glue, Redshift, Lambda, IAM
- [ ] **GCP** path: GCS, Dataflow, BigQuery, Cloud Composer
- [ ] Infrastructure as Code basics — **Terraform**
- [ ] Docker — containerize your pipelines
- [ ] CI/CD basics for data pipelines (GitHub Actions)

---

### ✅ Phase 8 — Data Quality & Best Practices (ongoing)

- [ ] **Great Expectations** or **dbt tests** for data validation
- [ ] Data lineage and observability
- [ ] Idempotent pipeline design
- [ ] Cost optimization in cloud warehouses
- [ ] Documentation habits — README, data dictionaries

---

## 📚 Recommended Resources

|Resource|Type|Best For|
|---|---|---|
|[SQLZoo](https://sqlzoo.net/)|Interactive|SQL Basics|
|[Mode SQL Tutorial](https://mode.com/sql-tutorial)|Tutorial|Analytics SQL|
|[dbt Learn](https://courses.getdbt.com/)|Course|dbt & ELT|
|[Fundamentals of Data Engineering](https://www.oreilly.com/library/view/fundamentals-of-data/9781098108298/) — Joe Reis|Book|DE Concepts|
|[The Data Engineering Cookbook](https://github.com/andkret/Cookbook)|Free PDF|Full Roadmap|
|[Leetcode (Database)](https://leetcode.com/problemset/database/)|Practice|SQL Interview Prep|
|[DataTalks.Club DE Zoomcamp](https://github.com/DataTalksClub/data-engineering-zoomcamp)|Free Bootcamp|Hands-on projects|

---

## 🔗 Related Notes

- [[Database Fundamentals]]
- [[Python for Data Engineering]]
- [[dbt Notes]]
- [[Airflow Notes]]
- [[Spark & PySpark]]
- [[Data Warehouse Design]]


## 🧠 Key Mental Models

> [!important] Think of SQL like asking questions to a table Every SQL query is just: _"From these tables, give me this data, filtered this way, grouped this way."_

> [!tip] The DE mindset A Data Engineer doesn't just query data — they **build the infrastructure that makes querying possible** reliably, at scale, and on schedule.

---

_Created: {{date}}_ _Status: 🌱 Seedling_