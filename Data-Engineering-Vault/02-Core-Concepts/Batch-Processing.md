# ⚙️ Batch Processing — Complete Guide
#concept #batch #spark #hadoop #mapreduce

> Process large volumes of data in scheduled runs. The workhorse of data engineering.

---

## 🧠 The Mental Model

Batch processing is like doing laundry:
- You don't wash one sock at a time (streaming)
- You collect dirty clothes, then wash them all at once (batch)
- The washing machine handles everything in parallel (distributed)
- You know the results when the cycle is done (latency is OK)

**When batch is the right choice:**
- Data freshness of hours/days is acceptable
- Very complex transformations (ML features, multi-join aggregations)
- Cost-sensitive (batch is cheaper than streaming)
- Reprocessing historical data

---

## 🗺️ MapReduce — The Foundation

All modern batch systems are descendants of Google's MapReduce (2004).

```
Input Data
    │
    ▼
┌─────────────────────────────────────┐
│              MAP PHASE              │
│  Split data → Apply function per   │
│  chunk → Emit key-value pairs      │
└─────────────────────────────────────┘
    │
    ▼ (Shuffle & Sort by key)
┌─────────────────────────────────────┐
│            REDUCE PHASE             │
│  Group by key → Aggregate values   │
│  → Emit final results              │
└─────────────────────────────────────┘
    │
    ▼
Output Data
```

**Word count (classic MapReduce):**
```python
# Map: (line) → [(word, 1), (word, 1), ...]
def map_func(line):
    for word in line.split():
        yield (word.lower(), 1)

# Reduce: (word, [1, 1, 1, ...]) → (word, count)
def reduce_func(word, counts):
    yield (word, sum(counts))
```

---

## ✨ Apache Spark — Modern Batch Processing

Spark replaced Hadoop MapReduce by keeping data **in memory** between stages.

```
MapReduce:   Map → Disk → Shuffle → Disk → Reduce → Disk (3x disk I/O)
Spark:       Map → RAM  → Shuffle → RAM  → Reduce → Disk (1x disk I/O)

Result: Spark is 10-100x faster for iterative algorithms
```

### Spark Architecture

```
┌──────────────────────────────────────────────────────────┐
│                      DRIVER NODE                          │
│  SparkContext / SparkSession                             │
│  - Creates the DAG of operations                         │
│  - Divides work into tasks                               │
│  - Monitors executors                                    │
└──────────────────────┬───────────────────────────────────┘
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
┌─────────────┐ ┌─────────────┐ ┌─────────────┐
│  Executor 1  │ │  Executor 2  │ │  Executor 3  │
│  (Worker)   │ │  (Worker)   │ │  (Worker)   │
│  4 cores    │ │  4 cores    │ │  4 cores    │
│  16GB RAM   │ │  16GB RAM   │ │  16GB RAM   │
│  Tasks: 4   │ │  Tasks: 4   │ │  Tasks: 4   │
└─────────────┘ └─────────────┘ └─────────────┘
```

### PySpark Complete Example

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.types import *
from pyspark.sql.window import Window

# Initialize
spark = SparkSession.builder \
    .appName("CustomerAnalysis") \
    .config("spark.sql.adaptive.enabled", "true") \
    .config("spark.sql.adaptive.coalescePartitions.enabled", "true") \
    .getOrCreate()

# ── READING ──────────────────────────────────────────────────────

# Parquet (best format for analytics)
orders = spark.read.parquet("s3://bucket/data/orders/year=2024/")

# CSV with schema inference disabled (always define schema!)
schema = StructType([
    StructField("order_id", LongType(), nullable=False),
    StructField("customer_id", LongType(), nullable=False),
    StructField("amount", DoubleType(), nullable=True),
    StructField("status", StringType(), nullable=True),
    StructField("created_at", TimestampType(), nullable=True)
])
orders_csv = spark.read \
    .schema(schema) \
    .option("header", True) \
    .csv("s3://bucket/data/orders.csv")

# Database
products = spark.read.jdbc(
    url="jdbc:postgresql://host:5432/mydb",
    table="products",
    numPartitions=8,          # Parallel reads
    partitionColumn="id",
    lowerBound=1,
    upperBound=1000000,
    properties={"user": "user", "password": "pass", "driver": "org.postgresql.Driver"}
)

# ── TRANSFORMATIONS ───────────────────────────────────────────────

# Column operations
orders_enriched = orders \
    .filter(F.col("status").isin("completed", "delivered")) \
    .filter(F.col("amount") > 0) \
    .withColumn("revenue", F.col("amount") * (1 - F.col("discount_pct"))) \
    .withColumn("order_month", F.date_trunc("month", F.col("created_at"))) \
    .withColumn("is_large_order", F.col("amount") > 1000) \
    .withColumnRenamed("customer_id", "cust_id")

# Window functions
window_spec = Window.partitionBy("customer_id").orderBy("created_at")
customer_orders = orders \
    .withColumn("order_rank", F.row_number().over(window_spec)) \
    .withColumn("cumulative_spend", F.sum("amount").over(window_spec)) \
    .withColumn("prev_order_amount", F.lag("amount", 1).over(window_spec)) \
    .withColumn("days_since_last", 
        F.datediff(F.col("created_at"), F.lag("created_at", 1).over(window_spec))
    )

# Aggregations
monthly_metrics = orders_enriched \
    .groupBy("order_month", "region") \
    .agg(
        F.sum("revenue").alias("total_revenue"),
        F.count("order_id").alias("order_count"),
        F.countDistinct("customer_id").alias("unique_customers"),
        F.avg("revenue").alias("avg_order_value"),
        F.percentile_approx("revenue", 0.5).alias("median_revenue"),
        F.percentile_approx("revenue", [0.25, 0.75]).alias("iqr_bounds")
    )

# Joins
customer_summary = orders_enriched.join(
    customers.select("customer_id", "name", "segment", "region"),
    on="customer_id",
    how="left"
)

# ── WRITING ───────────────────────────────────────────────────────

# Parquet with partitioning
monthly_metrics.write \
    .mode("overwrite") \
    .partitionBy("order_month") \
    .option("compression", "snappy") \
    .parquet("s3://bucket/analytics/monthly_metrics/")

# Delta Lake (ACID)
customer_summary.write \
    .format("delta") \
    .mode("overwrite") \
    .option("overwriteSchema", "true") \
    .save("s3://bucket/delta/customer_summary/")

# Database
monthly_metrics.write \
    .mode("append") \
    .jdbc(
        url="jdbc:postgresql://host:5432/analytics",
        table="monthly_metrics",
        properties={"user": "user", "password": "pass"}
    )
```

---

## 🧮 Spark SQL

```python
# Register as temp view
orders.createOrReplaceTempView("orders")
customers.createOrReplaceTempView("customers")

# Write complex SQL
result = spark.sql("""
    WITH order_stats AS (
        SELECT
            customer_id,
            COUNT(*) AS order_count,
            SUM(amount) AS lifetime_value,
            MIN(created_at) AS first_order,
            MAX(created_at) AS last_order,
            DATEDIFF(NOW(), MAX(created_at)) AS days_since_last_order
        FROM orders
        WHERE status = 'completed'
        GROUP BY customer_id
    ),
    customer_segments AS (
        SELECT
            customer_id,
            lifetime_value,
            CASE
                WHEN lifetime_value > 10000 AND days_since_last_order < 30 THEN 'Champions'
                WHEN lifetime_value > 5000 AND days_since_last_order < 90 THEN 'Loyal'
                WHEN days_since_last_order < 30 THEN 'Recent'
                WHEN days_since_last_order > 365 THEN 'Lost'
                ELSE 'At Risk'
            END AS rfm_segment
        FROM order_stats
    )
    SELECT
        c.name,
        c.email,
        cs.rfm_segment,
        os.lifetime_value,
        os.order_count,
        os.days_since_last_order
    FROM customers c
    JOIN order_stats os USING (customer_id)
    JOIN customer_segments cs USING (customer_id)
    ORDER BY os.lifetime_value DESC
""")
```

---

## 🔧 Performance Tuning

### Partitioning Strategy

```python
# Check current partitions
print(f"Partitions: {df.rdd.getNumPartitions()}")

# Rule of thumb: 128MB-256MB per partition
# For a 1TB dataset: 1000GB / 128MB = ~8000 partitions

# Repartition (full shuffle)
df_repartitioned = df.repartition(200, F.col("customer_id"))

# Coalesce (no shuffle, reduce only)
df_smaller = df.coalesce(50)

# Custom partitioner for joins
df_for_join = df.repartition(200, F.col("join_key"))
other_df_for_join = other.repartition(200, F.col("join_key"))
result = df_for_join.join(other_df_for_join, "join_key")
```

### Caching and Persistence

```python
from pyspark import StorageLevel

# Cache in memory (default)
df.cache()

# Persist with specific storage level
df.persist(StorageLevel.MEMORY_AND_DISK)  # Spill to disk if needed
df.persist(StorageLevel.DISK_ONLY)         # Cold storage (slow but saves RAM)

# Always materialize cache before using it
df.cache()
count = df.count()  # Triggers caching
print(f"Cached {count} rows")

# Clean up when done
df.unpersist()
```

### Broadcast Variables

```python
# Large lookup dictionary
product_categories = {1: "Electronics", 2: "Clothing", ...}

# Without broadcast: sent to each task (100s of copies)
# With broadcast: sent once per executor (~10 copies)
bc_categories = spark.sparkContext.broadcast(product_categories)

from pyspark.sql.functions import udf

@udf(returnType=StringType())
def get_category(product_id):
    return bc_categories.value.get(product_id, "Unknown")

df.withColumn("category", get_category(F.col("product_id")))
```

---

## 📊 Batch vs Stream Decision Matrix

| Criteria | Batch | Stream |
|---|---|---|
| Latency needed | Hours to days | Seconds to minutes |
| Data volume | Any | Any |
| Complexity | Very high | Medium |
| Cost | Lower | Higher |
| Reprocessing | Easy | Complex |
| Exactly-once | Easy | Hard |
| Example | Daily sales report | Fraud detection |

---

*Related: [[03-Tools/Spark]] | [[03-Tools/Hadoop]] | [[04-System-Design/ETL-Pipelines]] | Back to [[00-Index]]*
