# 🌊 Data Lakes vs Data Warehouses

> **Level:** Intermediate | **Stage:** 2 of 5
> Knowing *where* to store data — and *why* — is one of the most critical architectural decisions in Data Engineering.

---

## 🤔 The Core Difference

| | Data Lake | Data Warehouse |
|-|-----------|----------------|
| **Data type** | Raw, unstructured, semi-structured, structured | Structured, clean, processed |
| **Schema** | Schema-on-read (define when querying) | Schema-on-write (define at storage time) |
| **Storage cost** | Very cheap (object storage: S3, GCS) | More expensive (compute + storage) |
| **Query speed** | Slower (unless optimized) | Fast |
| **Users** | Data Engineers, Data Scientists | Data Analysts, Business Users |
| **Examples** | AWS S3, Azure Data Lake, GCS | Snowflake, BigQuery, Redshift |

---

## 🧠 Key Concepts

### 1. Data Lake
A **centralized repository** that stores data in its **raw, native format**.

```
S3 Bucket (Data Lake):
  /raw/
    orders/2024/01/orders_2024-01-15.json
    clickstream/2024/01/events_2024-01-15.parquet
    images/product_photos/
    logs/app_logs_2024-01-15.txt
```

**Key traits:**
- Store everything, figure out use later
- Great for ML training data, logs, unstructured data
- Needs a **catalog** (e.g., AWS Glue, Apache Hive Metastore) to know what's inside

### 2. Data Warehouse
A **structured, query-optimized** storage system for analytics.

```
Snowflake:
  RAW schema → STAGING schema → ANALYTICS schema
  All tables: defined columns, data types, constraints
```

### 3. The Lakehouse 🏠 (Modern Pattern)
**Best of both worlds:** Store raw data in a lake, but add warehouse-like features.

```
Delta Lake / Apache Iceberg / Apache Hudi
  ├── ACID transactions (no corrupt data)
  ├── Schema enforcement
  ├── Time travel (query data as it was yesterday)
  └── Fast queries (via Spark / Databricks / BigQuery)
```

**Tools:** Databricks (Delta Lake), Apache Iceberg (Netflix), Apache Hudi (Uber)

### 4. Medallion Architecture (Bronze / Silver / Gold)

```
Bronze (Raw):    Exact copy of source data, no changes
      ↓
Silver (Clean):  Deduplicated, validated, lightly transformed
      ↓
Gold (Curated):  Aggregated, business-ready, star schema
```

This pattern works for **both** lakes and lakehouses.

### 5. File Formats Matter

| Format | Type | Best For |
|--------|------|----------|
| CSV | Row | Small, simple data exchange |
| JSON | Semi-structured | APIs, nested data |
| Parquet | Columnar | Analytics (fast + compressed) |
| Delta | Columnar + ACID | Lakehouse tables |
| ORC | Columnar | Hive/Hadoop ecosystems |

> ✅ **Default choice:** Use **Parquet** for analytical workloads — it's compressed, fast, and widely supported.

---

## 🏭 Real-World Architecture

**E-commerce company data stack:**
```
[App DB (Postgres)] → Fivetran → S3 Bronze (raw JSON)
[Clickstream Events] → Kafka → S3 Bronze (raw Parquet)
[Images, PDFs]                → S3 Bronze (object storage)

S3 Bronze → Spark job → S3 Silver (cleaned Parquet)
S3 Silver → dbt → Snowflake Gold (analytics tables)
Snowflake Gold → Tableau/Looker → Business Dashboards
ML team → reads from S3 Silver directly for model training
```

---

## 🛠️ Tools & Technologies

| Tool | Category |
|------|----------|
| AWS S3, GCS, ADLS | Object storage (lake) |
| AWS Glue, Hive Metastore | Data catalog |
| Databricks | Lakehouse platform |
| Delta Lake, Iceberg, Hudi | Open table formats |
| Snowflake, BigQuery | Data warehouse |
| Apache Spark | Processing engine for lake data |

---

## ✅ Mini Practice Tasks

- [ ] Create an S3 bucket (or local folder) and organize it as Bronze/Silver/Gold
- [ ] Convert a CSV file to Parquet using pandas and compare file sizes
- [ ] Research Delta Lake and list 3 features that make it better than raw Parquet
- [ ] Draw an architecture diagram for a company of your choice (e-commerce, healthcare, etc.)

---

## 🔗 Related Notes

- [[Data Warehousing]] — Deep dive into the warehouse side
- [[Big Data Technologies]] — Hadoop is the original data lake technology
- [[DataEngineering_Obsidian_Vault/Apache Spark]] — Primary processing engine for data lakes
- [[Cloud for Data Engineering]] — Lakes live in cloud object storage
- [[ETL vs ELT]] — Pipelines move data between lake layers

---

## ➡️ Next Step

[[Big Data Technologies]] — Learn the distributed computing foundations that make lakes and large-scale processing possible.
