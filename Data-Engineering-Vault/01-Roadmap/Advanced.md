# 🏆 Advanced Data Engineering Roadmap
#advanced #roadmap #career #architecture

> **Goal:** Design and lead production data platforms at scale. Become the person teams rely on for architecture decisions.
> **Prerequisites:** [[01-Roadmap/Intermediate]] completed

---

## 🎯 What "Advanced" Looks Like

Advanced data engineers:
- Design systems that handle **petabyte scale**
- Make **architectural trade-offs** with full understanding of consequences
- **Mentor** junior and intermediate engineers
- Build **data platforms** (not just pipelines)
- Understand **economics** of data infrastructure (cost optimization)
- Contribute to **open source** data tools or write internal frameworks

---

## 🏗️ Advanced Architecture Patterns

### Lambda Architecture

Combines batch and streaming for best of both worlds:

```
                    ┌──────────────────────────────────────┐
                    │         SPEED LAYER (Real-time)      │
Raw Data ──────────►│  Kafka → Flink/Spark Streaming       │──► Serving
Sources    │        │  Low latency, approximate results    │    Layer
           │        └──────────────────────────────────────┘    (merge)
           │                                                      │
           └───────►┌──────────────────────────────────────┐     │
                    │         BATCH LAYER (Accuracy)        │─────┘
                    │  S3/HDFS → Spark → Data Warehouse    │
                    │  High latency, exact results          │
                    └──────────────────────────────────────┘
```

**Problem:** Maintaining two codebases (batch + streaming) is expensive.

### Kappa Architecture

Use streaming for everything:
```
Raw Data ──► Kafka ──► Flink ──► Output Store
                 ↑
          (Replay from Kafka
           for reprocessing)
```

**When to use what:**
| Architecture | Use When | Avoid When |
|---|---|---|
| Lambda | Need both real-time AND exact historical | Team is small |
| Kappa | Streaming-first, Kafka can store enough history | Need complex batch aggregations |
| Delta Lake | Need ACID on data lake | No Databricks/Spark |

---

## 🔥 Advanced Spark

### Spark Internals You Must Know

```
Job → Stages → Tasks

One Action = One Job
One Shuffle = One Stage boundary
One partition = One Task
```

**Understanding the Catalyst Optimizer:**
```python
# Spark builds a logical plan, then optimizes it
df.filter(col("year") == 2024) \
  .join(other_df, "id") \
  .select("name", "revenue")

# Catalyst may reorder:
# 1. Push down filter (reduce data before join)
# 2. Project pushdown (only read needed columns from Parquet)
# 3. Broadcast join if other_df is small
```

**Handling Data Skew:**
```python
# Problem: 80% of data has key="unknown"
# Solution 1: Salt the key
import random

df = df.withColumn(
    "salted_key",
    F.concat(F.col("key"), F.lit("_"), (F.rand() * 10).cast("int"))
)

# Process salted, then aggregate again
result = df.groupBy("salted_key").agg(F.sum("value")) \
    .withColumn("key", F.split(F.col("salted_key"), "_")[0]) \
    .groupBy("key").agg(F.sum("sum(value)").alias("total"))

# Solution 2: Broadcast the skewed key's data separately
from pyspark.sql.functions import broadcast

normal_keys = df.filter(F.col("key") != "unknown")
skewed_keys = df.filter(F.col("key") == "unknown")

# Process separately
result_normal = normal_keys.join(lookup, "id")
result_skewed = skewed_keys.join(broadcast(small_subset), "id")
result = result_normal.union(result_skewed)
```

**Delta Lake for ACID on Data Lakes:**
```python
from delta.tables import DeltaTable

# MERGE (upsert) — replaces complex delete+insert patterns
delta_table = DeltaTable.forPath(spark, "s3://bucket/delta/customers")

delta_table.alias("target").merge(
    source=new_data.alias("source"),
    condition="target.customer_id = source.customer_id"
).whenMatchedUpdate(set={
    "email": "source.email",
    "segment": "source.segment",
    "updated_at": "source.updated_at"
}).whenNotMatchedInsert(values={
    "customer_id": "source.customer_id",
    "email": "source.email",
    "segment": "source.segment",
    "created_at": "source.created_at",
    "updated_at": "source.updated_at"
}).execute()

# Time Travel
df_yesterday = spark.read.format("delta") \
    .option("versionAsOf", 42) \
    .load("s3://bucket/delta/customers")
```

---

## 🌊 Advanced Flink — Stateful Streaming

Flink's superpower is **stateful stream processing** — it can remember state across events.

```python
from pyflink.datastream import StreamExecutionEnvironment
from pyflink.datastream.state import ValueStateDescriptor
from pyflink.common.typeinfo import Types

env = StreamExecutionEnvironment.get_execution_environment()

class FraudDetector(KeyedProcessFunction):
    """Detect fraud: flag if >3 transactions in 60 seconds."""
    
    def open(self, runtime_context):
        # State stored per customer_id
        self.transaction_count = runtime_context.get_state(
            ValueStateDescriptor("txn_count", Types.LONG())
        )
        self.window_start = runtime_context.get_state(
            ValueStateDescriptor("window_start", Types.LONG())
        )
    
    def process_element(self, transaction, ctx, out):
        current_time = ctx.timestamp()
        window = self.window_start.value()
        count = self.transaction_count.value() or 0
        
        # Reset if outside 60-second window
        if window is None or current_time - window > 60_000:
            self.window_start.update(current_time)
            self.transaction_count.update(1)
        else:
            count += 1
            self.transaction_count.update(count)
            
            if count > 3:
                out.collect(FraudAlert(
                    customer_id=transaction.customer_id,
                    transaction_id=transaction.id,
                    reason="High frequency transactions"
                ))
```

---

## 📐 Data Platform Design

### The Modern Data Stack

```
┌─────────────────────────────────────────────────────────────┐
│                    CONSUMPTION LAYER                        │
│         BI Tools (Superset/Tableau) | ML | APIs            │
├─────────────────────────────────────────────────────────────┤
│                    SEMANTIC LAYER                           │
│              dbt metrics | Cube.dev | LookML               │
├─────────────────────────────────────────────────────────────┤
│                   TRANSFORMATION                            │
│                dbt | Spark | Flink                          │
├────────────────────┬────────────────────────────────────────┤
│   DATA WAREHOUSE   │         DATA LAKE                      │
│  Snowflake/BQ/RS   │    S3 + Delta/Iceberg/Hudi             │
├────────────────────┴────────────────────────────────────────┤
│                    INGESTION LAYER                          │
│        Fivetran | Airbyte | Kafka | dlt | custom           │
├─────────────────────────────────────────────────────────────┤
│                     DATA SOURCES                            │
│    OLTP DBs | APIs | SaaS | Events | Files | IoT           │
└─────────────────────────────────────────────────────────────┘
```

### Data Mesh Architecture

> Shift from centralized data team to domain-owned data products

**Principles:**
1. **Domain ownership** — each team owns their data products
2. **Data as a product** — data is treated like software products
3. **Self-serve platform** — central team provides tooling, not data
4. **Federated governance** — shared standards, local autonomy

```
Domain: Orders         Domain: Customers       Domain: Marketing
┌──────────────┐       ┌──────────────┐       ┌──────────────┐
│ orders_team  │       │ cust_team    │       │ mktg_team    │
│              │       │              │       │              │
│ fct_orders   │──────►│ customer_ltv │       │ campaign_roi │
│ order_funnel │       │ churn_score  │◄──────│ attribution  │
└──────────────┘       └──────────────┘       └──────────────┘
        ↑                      ↑                      ↑
        └──────────────────────┴──────────────────────┘
                    Central Data Platform
              (Catalog, Governance, Tooling, Compute)
```

---

## 💰 Cost Optimization at Scale

This is what separates senior engineers from junior ones — understanding the economics.

**S3 Cost Optimization:**
```python
# 1. Use Parquet + Snappy compression (typical 10x reduction vs CSV)
df.write.mode("overwrite") \
    .option("compression", "snappy") \
    .parquet("s3://bucket/data/")

# 2. Partition strategically (enable partition pruning)
df.write.partitionBy("year", "month", "day") \
    .parquet("s3://bucket/events/")

# 3. Right-size file sizes (avoid too many small files)
# Target: 128MB-1GB per file
df.coalesce(100).write.parquet(...)  # Reduce partition count

# 4. Use S3 Intelligent-Tiering for uncertain access patterns
# 5. Use Glacier for cold data (>90 days old, rarely accessed)
```

**Spark Cost Optimization:**
```python
# 1. Right-size your cluster
# Rule of thumb: ~4 cores per executor, 4x core memory

# 2. Use Spot/Preemptible instances for batch jobs (60-80% cheaper)
# On AWS EMR: use spot for task nodes, on-demand for core nodes

# 3. Adaptive Query Execution (AQE) — auto-tune at runtime
spark.conf.set("spark.sql.adaptive.enabled", "true")
spark.conf.set("spark.sql.adaptive.coalescePartitions.enabled", "true")

# 4. Dynamic partition pruning
spark.conf.set("spark.sql.optimizer.dynamicPartitionPruning.enabled", "true")
```

---

## 🔒 Data Security & Governance

### Column-Level Security
```sql
-- Snowflake: Dynamic Data Masking
CREATE MASKING POLICY email_mask AS
    (val STRING) RETURNS STRING ->
    CASE
        WHEN current_role() IN ('ANALYST') THEN '***@***.***'
        WHEN current_role() IN ('DATA_ENGINEER', 'ADMIN') THEN val
        ELSE '***REDACTED***'
    END;

ALTER TABLE customers
MODIFY COLUMN email SET MASKING POLICY email_mask;
```

### Data Lineage
```python
# OpenLineage — track data lineage automatically
from openlineage.airflow import DAG

# Lineage is captured automatically for supported operators
# Visualize in Marquez or DataHub
```

---

## 📊 Observability & Data Quality at Scale

```python
# Great Expectations for data validation
import great_expectations as gx

context = gx.get_context()
batch = context.sources.pandas_default.read_csv("orders.csv")

# Define expectations
batch.expect_column_to_exist("order_id")
batch.expect_column_values_to_not_be_null("order_id")
batch.expect_column_values_to_be_unique("order_id")
batch.expect_column_values_to_be_between("revenue", 0, 1_000_000)
batch.expect_column_values_to_match_regex("email", r"[^@]+@[^@]+\.[^@]+")

# Validate
results = batch.validate()
if not results["success"]:
    raise ValueError(f"Data quality failed: {results}")
```

---

## ✅ Advanced Checklist

- [ ] Design a complete data platform architecture
- [ ] Implement Delta Lake / Apache Iceberg for ACID operations
- [ ] Build stateful Flink applications
- [ ] Implement data mesh principles in an organization
- [ ] Optimize Spark jobs for cost and performance at TB scale
- [ ] Set up data observability (lineage, quality, monitoring)
- [ ] Mentor junior engineers
- [ ] Present architecture decisions to stakeholders

---

*Previous: [[01-Roadmap/Intermediate]] | Back to [[00-Index]]*
