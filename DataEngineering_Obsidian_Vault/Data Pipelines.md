# 🔧 Data Pipelines

> **Level:** Intermediate | **Stage:** 2 of 5
> A data pipeline is the automated system that moves and transforms data from source to destination — the backbone of all data engineering work.

---

## 🤔 What is a Data Pipeline?

A data pipeline is a **series of automated steps** that:
1. Collect data from one or more sources
2. Process / transform it
3. Deliver it to a destination

> **Real-world analogy:** Think of a water pipeline — water flows in, gets filtered/treated, and comes out clean at the tap. Data pipelines do the same for data.

---

## 🧠 Key Concepts

### 1. Pipeline Anatomy
```
[Source] → [Ingest] → [Validate] → [Transform] → [Load] → [Monitor]
   API         Python     Checks       dbt/Spark     DW       Alerts
   DB          Fivetran   Constraints  SQL           S3       Logs
   File        Kafka      Schema       Python        Lake     Dashboards
```

### 2. Batch vs Streaming Pipelines

| | Batch | Streaming |
|-|-------|-----------|
| **Data moves** | In scheduled chunks | Continuously, in real-time |
| **Latency** | Minutes to hours | Milliseconds to seconds |
| **Tools** | Airflow, Spark, dbt | Kafka, Flink, Spark Streaming |
| **Use case** | Daily reports, ETL jobs | Fraud detection, live dashboards |
| **Complexity** | Lower | Higher |

### 3. Pipeline Stages in Detail

**Extract:**
```python
# From API
response = requests.get("https://api.example.com/orders")
raw_data = response.json()

# From database
df = pd.read_sql("SELECT * FROM orders WHERE date = CURRENT_DATE", engine)
```

**Validate:**
```python
# Basic validation
assert df.shape[0] > 0, "No data extracted!"
assert df["order_id"].is_unique, "Duplicate order IDs found!"
assert df["amount"].notnull().all(), "Null amounts detected!"
```

**Transform:**
```python
df["revenue"] = df["quantity"] * df["price"]
df["order_month"] = pd.to_datetime(df["order_date"]).dt.to_period("M")
```

**Load:**
```python
df.to_sql("fact_orders", engine, if_exists="append", index=False)
```

### 4. Idempotency ⭐
A pipeline is **idempotent** if running it multiple times produces the same result.

```python
# ❌ Not idempotent — appends duplicates each run
df.to_sql("orders", engine, if_exists="append")

# ✅ Idempotent — safe to re-run
df.to_sql("orders", engine, if_exists="replace")

# ✅ Better — upsert by key
# Use ON CONFLICT DO UPDATE in PostgreSQL
```

### 5. Pipeline Monitoring
Things to track:
- **Row counts:** Did we get expected number of records?
- **Null rates:** Are critical columns empty?
- **Latency:** Is the pipeline running on time?
- **Errors:** Did any step fail?

---

## 🏭 Real-World Pipeline Example

**Daily Sales Reporting Pipeline:**
```
08:00 AM — Airflow triggers pipeline
  ↓
Extract from Stripe API (last 24hrs of payments)
  ↓
Validate: row count > 0, no null payment_ids
  ↓
Transform: calculate revenue, categorize by product
  ↓
Load to Snowflake (fact_daily_payments table)
  ↓
dbt runs to update downstream models
  ↓
Dashboard refreshes in Tableau
  ↓
Slack alert: "✅ Pipeline completed — 1,247 records loaded"
```

---

## 🛠️ Tools & Technologies

| Category | Tools |
|----------|-------|
| Orchestration | [[Airflow]], Prefect, Dagster |
| Batch processing | [[Apache Spark]], dbt, Pandas |
| Streaming | [[Apache Kafka]], AWS Kinesis, Flink |
| Managed EL | Fivetran, Airbyte, Stitch |
| Data quality | Great Expectations, dbt tests |
| Monitoring | Datadog, PagerDuty, custom Slack alerts |

---

## ✅ Mini Practice Tasks

- [ ] Build a simple batch pipeline: API → CSV → SQLite using Python
- [ ] Add validation checks (row count, nulls) to an existing script
- [ ] Make your pipeline idempotent by using `if_exists="replace"`
- [ ] Add a try/except with logging to handle pipeline failures gracefully
- [ ] Sketch a pipeline diagram for a real-world use case you find interesting

---

## 🔗 Related Notes

- [[ETL vs ELT]] — The core pattern all pipelines implement
- [[Airflow]] — The most popular tool to orchestrate and schedule pipelines
- [[Apache Kafka]] — For streaming (real-time) pipelines
- [[Apache Spark]] — For large-scale batch processing inside pipelines
- [[Data Warehousing]] — The most common pipeline destination

---

## ➡️ Next Step

[[Data Warehousing]] — Learn where the data goes *after* your pipeline finishes loading it.
