# Project 01 - CSV Data Pipeline

#data-engineering #ETL #beginner #spark #pandas #aws #business-intelligence

---

## 📌 Overview

A foundational batch data pipeline that ingests CSV files from multiple sources, cleans and transforms the data, stores it in cloud storage, and generates business reports. This is the classic "hello world" of data engineering and the base architecture used in thousands of enterprise systems.

---

## 🎯 Scope

- **Data Ingestion**: Extract CSV files from APIs, FTP servers, and cloud storage
- **Processing**: Clean, transform, and standardize data (handle nulls, type mismatches, duplicates)
- **Storage**: Store structured data in queryable format
- **Visualization**: Generate reports for business insights

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | CSV Files, FTP, APIs, Cloud Storage | Raw data ingestion |
| Processing | Python (Pandas), PySpark | Data transformation |
| Storage | AWS S3 / Google Cloud Storage / HDFS | Scalable data store |
| Visualization | Power BI / Tableau | Business reporting |

---

## 🔄 Data Pipeline Architecture

```
[CSV Sources]
    │
    ├── FTP Server
    ├── REST APIs
    └── Cloud Storage (S3/GCS)
         │
         ▼
[Ingestion Layer]
    Python / Pandas script
    - File detection & download
    - Schema validation
         │
         ▼
[Processing Layer]
    PySpark / Pandas
    - Null handling
    - Deduplication
    - Type casting
    - Standardization
         │
         ▼
[Storage Layer]
    AWS S3 (Parquet/ORC format)
    - Partitioned by date/category
         │
         ▼
[Visualization Layer]
    Power BI / Tableau Dashboard
```

---

## 📊 Use Cases

- **Customer Order Analytics**: Track order volume, revenue, returns by time period
- **Sales Performance Tracking**: Regional and product-level sales dashboards
- **Data Warehousing for Business Intelligence**: Feed downstream data marts

---

## ⚠️ Challenges

1. **Inconsistent Data Formats**: Different CSV schemas across sources (different column names, date formats)
2. **Handling Large Datasets**: Single-machine Pandas fails at scale (>1GB files)
3. **Schema Mismatch**: New columns appear in upstream CSVs without warning
4. **Encoding Issues**: UTF-8 vs Latin-1 encoding conflicts
5. **Partial File Delivery**: Incomplete files transferred via FTP

---

## ✅ Solutions

- **Automate data cleaning pipelines** using Pandas with a schema registry (Great Expectations)
- **Use distributed computing (PySpark)** for processing large datasets efficiently
- **Apply schema enforcement** with tools like Apache Avro or JSON Schema to detect mismatches early
- **Implement file integrity checks** (checksum validation) before processing
- **Use retry logic** for FTP/API ingestion failures

---

## 🧠 Key Learnings

- Understanding the difference between **schema-on-read** vs **schema-on-write**
- Why **Parquet format** is preferred over CSV for storage (columnar compression, 10x faster queries)
- The importance of **idempotent pipelines** — running the same job twice should not produce duplicate records
- How **partitioning** in S3 (`year=2024/month=01/`) dramatically speeds up downstream queries

---

## 🏢 Industry Insights

### How Netflix Uses CSV-Origin Pipelines
Netflix ingests billions of user event records daily. Even at their scale, the journey started with CSV exports from legacy systems. They built an internal tool called **Metacat** to track schema changes automatically, solving the schema mismatch problem at massive scale.

### How Amazon Handles CSV at Scale
Amazon's internal fulfillment systems export CSV daily. They use **AWS Glue Crawlers** to automatically detect schema and register it in the Glue Data Catalog before any transformation runs.

### How Airbnb Standardizes Ingestion
Airbnb built **Minerva**, a metric framework. All CSV/flat-file data first passes through a **standardization layer** that enforces column naming conventions, data types, and null policies.

### Production Best Practices
- Always store **raw files untouched** in a "raw" S3 bucket before processing
- Use **immutable storage** (S3 versioning) for audit trails
- Log every ingestion run with metadata (file name, row count, processing time)
- Implement **dead-letter queues** for files that fail validation

---

## 🚀 Optimization Techniques

- Convert CSV → **Parquet** immediately after ingestion (60–80% storage savings)
- Use **Spark's `read.csv()` with schema inference disabled** and provide explicit schema
- **Partition data** by ingestion date in S3 for efficient Athena queries
- Use **broadcast joins** in Spark when joining a large table with a small lookup table
- Enable **Adaptive Query Execution (AQE)** in Spark 3.x for dynamic optimization

---

## 🔍 Deep Dive

### Step-by-Step Architecture Explanation

**Step 1 — File Detection**
Use a Python watcher or AWS S3 Event Notification + Lambda to detect new file arrivals. This avoids polling and enables event-driven ingestion.

**Step 2 — Schema Validation**
Before any processing, validate the incoming file against the expected schema using Great Expectations or Pydantic. Reject invalid files immediately and route them to a dead-letter path.

**Step 3 — Transformation**
For files < 500MB: use Pandas. For files > 500MB: use PySpark on a cluster (AWS EMR / Dataproc). Key transformations:
```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, to_date, trim, upper

spark = SparkSession.builder.appName("CSV_Pipeline").getOrCreate()
df = spark.read.option("header", "true").option("inferSchema", "false").schema(schema).csv("s3://raw-bucket/orders/")
df_clean = df.withColumn("order_date", to_date(col("order_date"), "yyyy-MM-dd")) \
             .withColumn("customer_name", trim(upper(col("customer_name")))) \
             .dropDuplicates(["order_id"])
df_clean.write.mode("overwrite").partitionBy("year", "month").parquet("s3://processed-bucket/orders/")
```

**Step 4 — Storage**
Write to S3 in Parquet format, partitioned by date. Register the table in AWS Glue Data Catalog so it's queryable via Athena.

**Step 5 — Visualization**
Connect Power BI or Tableau directly to Athena or the S3 Parquet location.

### Why Each Tool Is Used
- **Pandas**: Quick prototyping, small-to-medium datasets, rich transformation API
- **PySpark**: Horizontal scaling across cluster, handles TB-scale data
- **AWS S3**: Infinitely scalable, cheap object storage, native integration with AWS ecosystem
- **Parquet**: Columnar format — only reads required columns, heavily compressed

### Alternatives
| Original Tool | Alternative | When to Use Alternative |
|--------------|-------------|------------------------|
| PySpark | Dask | When you need Pandas API but need some scaling |
| PySpark | DuckDB | When data is < 100GB and you want in-process SQL |
| AWS S3 | GCS / Azure ADLS | When on GCP or Azure ecosystem |
| Power BI | Metabase / Superset | Open-source, self-hosted option |

---

## 💡 Tips & Tricks

- **Performance**: Use `spark.conf.set("spark.sql.shuffle.partitions", "200")` appropriately (default 200 is too high for small datasets)
- **Debugging**: Always check `.printSchema()` and `.show(5)` at each transformation step
- **Cost**: Use S3 Intelligent-Tiering to automatically move old data to cheaper storage classes
- **Scaling**: When a single Spark job takes >1 hour, consider breaking it into micro-batches with Airflow

---

## 🔗 Related Projects

- [[Project 03 - Retail POS ETL Pipeline]] — Same CSV pattern but with Airflow orchestration
- [[Project 07 - Excel & Google Sheets Pipeline]] — Extension to other flat-file formats
- [[Project 10 - Multi-Department CSV Integration]] — Multi-source CSV at enterprise scale
- [[ETL vs ELT]] — Understand the conceptual difference
- [[Big Data Tools Overview]] — Spark and Pandas deep dive

---

*Tags: #data-engineering #ETL #beginner #spark #pandas #aws #csv #business-intelligence*
