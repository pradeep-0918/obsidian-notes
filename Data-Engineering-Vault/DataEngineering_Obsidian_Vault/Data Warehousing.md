# 🏢 Data Warehousing

> **Level:** Intermediate | **Stage:** 2 of 5
> A data warehouse is the central repository where clean, structured data is stored for analysis — the heart of modern analytics infrastructure.

---

## 🤔 What is a Data Warehouse?

A data warehouse (DW) is a **centralized storage system** optimized for:
- **Reading & querying** large amounts of historical data
- **Analytical workloads** (not transaction processing)
- **Integrating** data from multiple sources

> **Real-world analogy:** A regular database is like your desk drawer — great for quick access. A data warehouse is like a well-organized archive room — built for finding patterns across years of records.

---

## 🧠 Key Concepts

### 1. OLTP vs OLAP

| | OLTP (Transactional) | OLAP (Analytical) |
|-|---------------------|-------------------|
| **Purpose** | Day-to-day operations | Business intelligence |
| **Queries** | Simple, fast, row-level | Complex, slow, aggregated |
| **Data size** | GB | TB to PB |
| **Examples** | MySQL (app DB) | Snowflake, BigQuery |
| **Schema** | Normalized (3NF) | Denormalized (star schema) |

### 2. Data Warehouse Architecture (Layers)

```
[Raw / Staging Layer]    ← Raw data, no transformations
        ↓
[Integration Layer]      ← Cleaned, conformed, joined
        ↓
[Presentation Layer]     ← Star schema, ready for analysts/BI
```

Also called: **Bronze → Silver → Gold** (Medallion Architecture)

### 3. Columnar Storage ⭐
Warehouses store data **by column** instead of by row.

```
Row store (MySQL):      [row1: id, name, city, age] [row2: ...]
Column store (BigQuery): [id: 1,2,3...] [name: ...] [city: ...]
```

**Why it matters:** If you query `SELECT SUM(revenue) FROM orders`, columnar storage only reads the `revenue` column — 10–100x faster!

### 4. Partitioning & Clustering
```sql
-- Partition by date (BigQuery)
CREATE TABLE orders
PARTITION BY DATE(order_date)
AS SELECT * FROM raw_orders;

-- Query only reads the relevant partition
SELECT * FROM orders WHERE order_date = '2024-01-15';
```

### 5. Materialized Views
Pre-computed query results stored as a table — fast to query, automatically refreshed.
```sql
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT DATE_TRUNC('month', order_date) AS month, SUM(amount) AS revenue
FROM orders
GROUP BY 1;
```

---

## 🏭 Real-World Warehouse Setup

```
Sources: Postgres (app DB) + Stripe API + Salesforce CRM
    ↓ (via Fivetran)
Raw Layer (Snowflake):
  raw.postgres_orders, raw.stripe_payments, raw.salesforce_accounts
    ↓ (via dbt)
Staging Layer:
  stg_orders, stg_payments, stg_accounts
    ↓ (via dbt)
Mart Layer:
  mart_revenue (star schema) → Tableau/Looker dashboard
```

---

## 🛠️ Tools & Technologies

| Tool | Type | Best For |
|------|------|----------|
| **Snowflake** | Cloud DW | Separation of storage/compute, SQL-heavy teams |
| **BigQuery** | Cloud DW (Google) | Serverless, pay-per-query, great with GCP |
| **Redshift** | Cloud DW (AWS) | AWS-native workloads |
| **Databricks** | Lakehouse | Mix of data lake + warehouse (Delta Lake) |
| **DuckDB** | Local DW | Fast local analytics, great for development |
| **dbt** | Transformation | Build models on top of any warehouse |

---

## ✅ Mini Practice Tasks

- [ ] Create a free Snowflake trial account and run your first query
- [ ] Load a CSV into BigQuery and run an aggregation query
- [ ] Design a 3-layer architecture (raw/staging/mart) for a use case you choose
- [ ] Create a partitioned table and compare query performance with and without partition filter
- [ ] Build a simple dbt project with staging + mart models

---

## 🔗 Related Notes

- [[Data Modeling]] — Star schema is the standard modeling approach for DWs
- [[ETL vs ELT]] — The pipeline pattern that feeds data into the warehouse
- [[Data Lakes vs Data Warehouses]] — Know when to use each
- [[SQL for Data Engineering]] — Primary query language for all warehouses
- [[Cloud for Data Engineering]] — Most modern DWs live in the cloud

---

## ➡️ Next Step

[[Data Lakes vs Data Warehouses]] — Understand the trade-offs between storing structured vs raw data.
