# 40 — Data Engineering Concepts

#advanced #dataengineering #interview

---

## 🧠 Introduction

Data Engineering is the discipline of **building and maintaining the data infrastructure** that enables data scientists and analysts to do their work. SQL is the foundation, but you also need to understand pipelines, warehouses, and architecture.

---

## 🏗️ The Data Engineering Stack

```
RAW DATA SOURCES
  ├── Databases (MySQL, PostgreSQL)
  ├── APIs (REST, GraphQL)
  ├── Files (CSV, JSON, Parquet)
  ├── Streams (Kafka, Kinesis)
  └── SaaS tools (Salesforce, Stripe)
          ↓
     ETL / ELT LAYER
  ├── Python scripts
  ├── Apache Airflow (orchestration)
  ├── dbt (transformations)
  └── Spark / Flink (large scale)
          ↓
   DATA WAREHOUSE / DATA LAKE
  ├── Snowflake / BigQuery / Redshift
  └── Delta Lake / Apache Iceberg
          ↓
  CONSUMPTION LAYER
  ├── BI Tools (Tableau, Looker, Power BI)
  ├── Data Science (Python notebooks)
  └── Operational dashboards
```

---

## 🔄 ETL vs ELT

| Aspect | ETL | ELT |
|--------|-----|-----|
| Full form | Extract-Transform-Load | Extract-Load-Transform |
| Transform where? | Before loading | After loading (in warehouse) |
| Best for | Traditional DWH, sensitive data | Cloud DWH (BigQuery, Snowflake) |
| Tool examples | SSIS, Informatica, Talend | dbt, Spark SQL |
| Flexibility | Less flexible | More flexible (raw data preserved) |

```
ETL:  Source → [Extract] → [Transform] → [Load] → Warehouse
ELT:  Source → [Extract] → [Load] → Warehouse → [Transform]
```

---

## 📐 Star Schema vs Snowflake Schema

### Star Schema (Denormalized — Preferred for DWH)

```
              DIM_DATE
                 |
DIM_CUSTOMER — FACT_ORDERS — DIM_PRODUCT
                 |
            DIM_LOCATION
```

```sql
-- Star schema: DIM tables join directly to FACT
-- Simple, fast, easy for BI tools
SELECT
    d.month_name,
    p.category,
    SUM(f.revenue) AS total_revenue
FROM fact_orders f
JOIN dim_date     d ON f.date_key     = d.date_key
JOIN dim_product  p ON f.product_sk   = p.product_sk
GROUP BY d.month_name, p.category;
```

### Snowflake Schema (Normalized DIM tables)

```
DIM_PRODUCT → DIM_CATEGORY (separate table for categories)
DIM_CUSTOMER → DIM_CITY → DIM_COUNTRY (hierarchy split)
```

> Star = fewer JOINs, faster queries. Snowflake = less storage redundancy. **Star schema wins for most DWH use cases.**

---

## 📦 Data Pipeline Design Patterns

### 1. Incremental Load (Most Common)

```sql
-- Load only new/changed records since last run
-- Requires a watermark column (updated_at, created_at, or sequence ID)

-- Track last loaded timestamp
SELECT MAX(last_loaded_at) FROM etl_watermarks WHERE table_name = 'orders';

-- Extract only new data
SELECT * FROM orders
WHERE updated_at > '2024-07-14 23:59:59'   -- last watermark
ORDER BY updated_at;

-- After load: update watermark
UPDATE etl_watermarks
SET last_loaded_at = NOW()
WHERE table_name = 'orders';
```

### 2. Full Refresh

```sql
-- Drop and reload (simple but expensive for large tables)
TRUNCATE TABLE fact_orders_daily;
INSERT INTO fact_orders_daily SELECT ... FROM orders WHERE DATE(order_date) = CURDATE() - 1;
```

### 3. Upsert (INSERT or UPDATE)

```sql
-- MySQL: INSERT ... ON DUPLICATE KEY UPDATE
INSERT INTO dim_customer (customer_id, name, city, tier, updated_at)
VALUES (1001, 'Priya', 'Mumbai', 'Gold', NOW())
ON DUPLICATE KEY UPDATE
    name       = VALUES(name),
    city       = VALUES(city),
    tier       = VALUES(tier),
    updated_at = VALUES(updated_at);

-- REPLACE INTO (deletes old + inserts new — use carefully)
REPLACE INTO dim_product (product_id, name, price) VALUES (101, 'iPhone 15', 79999);
```

---

## 🕐 Data Freshness & Scheduling

```
Real-time      → Streaming (Kafka + Flink/Spark Streaming)
Near real-time → Micro-batch every 5-15 minutes
Hourly         → Cron job / Airflow DAG every hour
Daily          → ETL job at 2 AM (after day's data is settled)
Weekly/Monthly → Aggregate reports, billing cycles
```

---

## 🔍 Data Quality Checks (Must Know)

```sql
-- 1. Completeness: check for NULLs in critical columns
SELECT
    COUNT(*) AS total,
    SUM(ISNULL(customer_id)) AS missing_customer,
    SUM(ISNULL(amount))      AS missing_amount,
    SUM(ISNULL(order_date))  AS missing_date
FROM orders_staging;

-- 2. Validity: check business rules
SELECT COUNT(*) AS invalid_amounts
FROM orders WHERE amount <= 0;

SELECT COUNT(*) AS future_orders
FROM orders WHERE order_date > CURDATE();

-- 3. Uniqueness: check for duplicates
SELECT order_id, COUNT(*) AS cnt FROM orders
GROUP BY order_id HAVING cnt > 1;

-- 4. Referential integrity: orphaned records
SELECT COUNT(*) AS orphaned_orders
FROM orders o
LEFT JOIN users u ON o.user_id = u.user_id
WHERE u.user_id IS NULL;

-- 5. Freshness: check data is up to date
SELECT DATEDIFF(CURDATE(), MAX(order_date)) AS hours_since_last_record
FROM orders;
-- Alert if > 26 hours for a daily pipeline
```

---

## 📊 Fact Table Design

```sql
-- Additive facts: can SUM across all dimensions
revenue, quantity, cost → fully additive

-- Semi-additive facts: can SUM across some dimensions
account_balance → SUM across accounts OK, but NOT across time periods
inventory_level → Same issue

-- Non-additive facts: cannot SUM
ratio, percentage, average → store components (numerator/denominator) instead
-- Store: total_items and total_value → calculate avg at query time
```

---

## ⚡ Performance in Data Warehouses

```sql
-- For DWH queries, these optimizations matter:

-- 1. Partition fact tables by date
PARTITION BY RANGE (date_key) (
    PARTITION p2024 VALUES LESS THAN (20250101)
);

-- 2. Cluster/sort key alignment (Snowflake/BigQuery concept)
-- ORDER BY date_key on inserts to improve micro-partition pruning

-- 3. Pre-aggregate for common metrics
CREATE TABLE agg_daily_sales AS
SELECT date_key, product_sk, SUM(revenue) AS revenue
FROM fact_orders GROUP BY date_key, product_sk;

-- 4. Materialized views for expensive recurring queries
-- (Snowflake, BigQuery native feature; MySQL: manual refresh table)
```

---

## 💼 Interview Questions

1. **What is the difference between ETL and ELT?**
   > ETL transforms data before loading (on a separate server). ELT loads raw data first, then transforms inside the warehouse (leveraging its compute). ELT is preferred for cloud data warehouses.

2. **What is a data pipeline? Describe its stages.**
   > A series of processes that move data from source to destination: Extract (pull from source), Transform (clean, enrich, aggregate), Load (write to target), Validate (check quality), Monitor (alert on failure).

3. **What is the difference between a star schema and a snowflake schema?**
   > Star: denormalized dimension tables, fewer JOINs, faster queries. Snowflake: normalized dimensions, more JOINs, less storage. Star schema is preferred for most OLAP workloads.

4. **How do you implement idempotent pipeline runs?**
   > Use `INSERT ... ON DUPLICATE KEY UPDATE`, or delete-then-insert for the same time window. Running the pipeline twice should produce the same result.

5. **What is an SCD and why does it matter in data warehousing?**
   > Slowly Changing Dimension — handles attribute changes in dimension data over time. Matters because historical reports must use the value that was true at the time of the event, not the current value.

6. **Scenario:** *"Your daily pipeline loads data at 3 AM. By 9 AM, analysts report the data is wrong. What's your checklist?"*
   > 1. Check ETL logs for errors. 2. Check source data quality (NULL counts, duplicates). 3. Verify watermark/incremental logic didn't miss rows. 4. Check if source schema changed. 5. Validate row counts match source. 6. Check for duplicate loads (idempotency issue).

---

## 🔗 Related Topics

- [[32 - Slowly Changing Dimensions]] — SCD patterns
- [[33 - Python JDBC Connectivity]] — Python ETL implementation
- [[34 - Data Engineering Projects]] — End-to-end projects
- [[27 - Partitioning]] — DWH table partitioning
- [[31 - Normalization]] — OLTP schema vs OLAP schema
- [[38 - Query Optimization Patterns]] — Optimizing DWH queries
