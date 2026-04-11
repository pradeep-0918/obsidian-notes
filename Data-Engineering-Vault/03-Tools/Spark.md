# ⚡ Apache Spark — Complete Guide
#tool #spark #batch #streaming #distributed

> The unified analytics engine. Runs on a laptop or 10,000 nodes. The most important tool in data engineering.

---

## 🧠 Why Spark Won

Before Spark (Hadoop MapReduce era):
- Every job wrote intermediate results to disk (slow)
- One programming model (map then reduce)
- Java only (verbose)
- No interactive analysis

Spark solved all of this:
- **In-memory processing** → 10-100x faster
- **Rich API** → SQL, DataFrames, ML, Streaming — same engine
- **Multiple languages** → Python, Scala, Java, R
- **Interactive shell** → Explore data instantly

---

## 🏗️ Spark Architecture Deep Dive

```
┌──────────────────────────────────────────────────────────────────┐
│                        SPARK APPLICATION                          │
│                                                                  │
│  ┌─────────────────────────────────────────┐                    │
│  │               DRIVER PROGRAM             │                    │
│  │  - Your main() / PySpark script         │                    │
│  │  - Creates SparkContext/SparkSession    │                    │
│  │  - Builds logical plan (DAG)            │                    │
│  │  - Schedules tasks on executors         │                    │
│  │  - Collects results                     │                    │
│  └──────────────────┬──────────────────────┘                    │
│                     │ (via Cluster Manager)                      │
│                     │                                            │
│          ┌──────────┼──────────┐                                 │
│          ▼          ▼          ▼                                 │
│  ┌──────────────┐ ┌──────────┐ ┌──────────────┐                │
│  │  Executor 1  │ │Executor 2│ │  Executor 3  │                │
│  │  ┌──────────┐│ │┌────────┐│ │┌────────────┐│                │
│  │  │ Task 1   ││ ││Task 5  ││ ││  Task 9    ││                │
│  │  │ Task 2   ││ ││Task 6  ││ ││  Task 10   ││                │
│  │  │ Task 3   ││ ││Task 7  ││ ││  Task 11   ││                │
│  │  │ Task 4   ││ ││Task 8  ││ ││  Task 12   ││                │
│  │  └──────────┘│ │└────────┘│ │└────────────┘│                │
│  │  Memory: 16GB│ │          │ │              │                │
│  └──────────────┘ └──────────┘ └──────────────┘                │
└──────────────────────────────────────────────────────────────────┘

Cluster Managers: YARN, Kubernetes, Spark Standalone, Mesos
```

### Lazy Evaluation — The Key Insight

```python
# This does NOTHING (lazy):
df = spark.read.parquet("s3://bucket/data/")
filtered = df.filter(df.age > 25)
joined = filtered.join(other_df, "id")
aggregated = joined.groupBy("region").agg(F.sum("revenue"))

# THIS triggers execution (actions):
aggregated.show()        # Action
aggregated.count()       # Action
aggregated.collect()     # Action
aggregated.write.save()  # Action

# Why? Spark can optimize the ENTIRE plan before executing
# e.g., push filter down to parquet, broadcast small tables
```

---

## 💡 SparkSession Configuration

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("ProductionJob") \
    .master("yarn") \
    \
    .config("spark.executor.instances", "20") \
    .config("spark.executor.cores", "4") \
    .config("spark.executor.memory", "16g") \
    .config("spark.driver.memory", "8g") \
    \
    .config("spark.sql.adaptive.enabled", "true") \
    .config("spark.sql.adaptive.coalescePartitions.enabled", "true") \
    .config("spark.sql.adaptive.skewJoin.enabled", "true") \
    .config("spark.sql.adaptive.localShuffleReader.enabled", "true") \
    \
    .config("spark.sql.shuffle.partitions", "400") \
    .config("spark.default.parallelism", "400") \
    \
    .config("spark.serializer", "org.apache.spark.serializer.KryoSerializer") \
    .config("spark.sql.parquet.filterPushdown", "true") \
    .config("spark.sql.parquet.mergeSchema", "false") \
    \
    .config("spark.hadoop.fs.s3a.multipart.size", "134217728") \
    .config("spark.hadoop.fs.s3a.fast.upload", "true") \
    \
    .getOrCreate()

spark.sparkContext.setLogLevel("WARN")
```

---

## 🔧 Advanced DataFrame Operations

```python
from pyspark.sql import functions as F
from pyspark.sql.window import Window
from pyspark.sql.types import *

# ── SCHEMA OPERATIONS ─────────────────────────────────────────────

# Print schema
df.printSchema()

# Define explicit schema (always do this for CSV/JSON!)
schema = StructType([
    StructField("id", LongType(), nullable=False),
    StructField("name", StringType(), nullable=True),
    StructField("metadata", MapType(StringType(), StringType()), nullable=True),
    StructField("tags", ArrayType(StringType()), nullable=True),
])

# ── COLUMN OPERATIONS ─────────────────────────────────────────────

df2 = df \
    .withColumn("full_name", F.concat_ws(" ", F.col("first"), F.col("last"))) \
    .withColumn("age_group", F.when(F.col("age") < 25, "youth")
                              .when(F.col("age") < 65, "adult")
                              .otherwise("senior")) \
    .withColumn("revenue_log", F.log(F.col("revenue") + 1)) \
    .withColumn("date", F.to_date(F.col("timestamp_str"), "yyyy-MM-dd HH:mm:ss")) \
    .withColumn("year", F.year("date")) \
    .withColumn("month", F.month("date")) \
    .withColumn("day_of_week", F.dayofweek("date")) \
    .withColumn("is_weekend", F.col("day_of_week").isin([1, 7])) \
    .withColumn("tags_count", F.size(F.col("tags"))) \
    .withColumn("first_tag", F.col("tags")[0]) \
    .withColumn("exploded_tag", F.explode_outer(F.col("tags")))

# ── COMPLEX AGGREGATIONS ──────────────────────────────────────────

agg_df = df.groupBy("region", "category").agg(
    F.count("*").alias("total_orders"),
    F.sum("revenue").alias("total_revenue"),
    F.avg("revenue").alias("avg_revenue"),
    F.stddev("revenue").alias("stddev_revenue"),
    F.min("revenue").alias("min_revenue"),
    F.max("revenue").alias("max_revenue"),
    F.percentile_approx("revenue", [0.25, 0.5, 0.75]).alias("percentiles"),
    F.collect_list("product_id").alias("products"),
    F.collect_set("customer_id").alias("unique_customers"),
    F.first("status").alias("first_status"),
    F.last("status").alias("last_status"),
    F.sum(F.when(F.col("status") == "refunded", 1).otherwise(0)).alias("refund_count"),
)

# ── WINDOW FUNCTIONS ──────────────────────────────────────────────

# Different window specs
w_customer = Window.partitionBy("customer_id").orderBy("order_date")
w_customer_unbounded = Window.partitionBy("customer_id").orderBy("order_date") \
    .rowsBetween(Window.unboundedPreceding, Window.currentRow)
w_rolling_7d = Window.partitionBy("region") \
    .orderBy(F.col("order_date").cast("long")) \
    .rangeBetween(-7 * 86400, 0)  # 7 days in seconds

windowed_df = df \
    .withColumn("order_rank", F.rank().over(w_customer)) \
    .withColumn("row_num", F.row_number().over(w_customer)) \
    .withColumn("cumulative_revenue", F.sum("revenue").over(w_customer_unbounded)) \
    .withColumn("prev_order_amount", F.lag("revenue", 1, 0.0).over(w_customer)) \
    .withColumn("next_order_amount", F.lead("revenue", 1, 0.0).over(w_customer)) \
    .withColumn("rolling_7d_revenue", F.sum("revenue").over(w_rolling_7d)) \
    .withColumn("pct_of_customer_total", 
        F.col("revenue") / F.sum("revenue").over(Window.partitionBy("customer_id")))

# ── JOINS ─────────────────────────────────────────────────────────

# Broadcast join (for small tables <10MB)
from pyspark.sql.functions import broadcast
result = large_df.join(broadcast(small_lookup), "product_id", "left")

# Anti-join (records in left NOT in right)
new_customers = current_customers.join(existing_customers, "id", "left_anti")

# Semi-join (records in left that exist in right)
active_orders = orders.join(active_customers, "customer_id", "left_semi")

# Approximate joins (for fuzzy matching)
# Use Levenshtein distance or soundex for name matching
from pyspark.sql.functions import soundex, levenshtein
df.withColumn("name_soundex", soundex("name"))
```

---

## 🔥 Spark Streaming

```python
# See [[02-Core-Concepts/Stream-Processing]] for full streaming guide

# Quick reference:
stream = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "kafka:9092") \
    .option("subscribe", "orders") \
    .load()

query = stream.writeStream \
    .format("delta") \
    .option("checkpointLocation", "s3://bucket/checkpoints/") \
    .outputMode("append") \
    .start()

query.awaitTermination()
```

---

## 🧪 Testing Spark Jobs

```python
import pytest
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

@pytest.fixture(scope="session")
def spark():
    return SparkSession.builder \
        .appName("tests") \
        .master("local[2]") \
        .config("spark.sql.shuffle.partitions", "2") \
        .getOrCreate()

def test_revenue_calculation(spark):
    # Arrange
    data = [
        (1, 100.0, 0.1),   # id, amount, discount
        (2, 200.0, 0.0),
        (3, 300.0, 0.2),
    ]
    df = spark.createDataFrame(data, ["id", "amount", "discount"])
    
    # Act
    result = calculate_revenue(df)
    
    # Assert
    rows = result.collect()
    assert rows[0]["revenue"] == pytest.approx(90.0)   # 100 * (1-0.1)
    assert rows[1]["revenue"] == pytest.approx(200.0)
    assert rows[2]["revenue"] == pytest.approx(240.0)  # 300 * (1-0.2)
    assert result.count() == 3

def calculate_revenue(df):
    return df.withColumn("revenue", F.col("amount") * (1 - F.col("discount")))
```

---

## 📊 Spark Optimization Cheatsheet

| Problem | Symptom | Solution |
|---|---|---|
| Data skew | One task takes 100x longer | Salt keys, broadcast small side |
| Too many small files | Slow S3 listing, slow reads | Coalesce before writing |
| OOM errors | Executor killed | Increase executor memory, reduce partition size |
| Slow shuffle | Long stage with network I/O | Reduce shuffle (better joins, AQE) |
| Full table scans | Reading all columns/partitions | Add filters, use partition pruning |
| Slow UDFs | Poor performance vs native | Replace with built-in functions |
| Long stage chains | No parallelism | Cache/checkpoint intermediate results |

---

## 🚀 Deployment

```bash
# Submit to YARN
spark-submit \
  --master yarn \
  --deploy-mode cluster \
  --num-executors 20 \
  --executor-cores 4 \
  --executor-memory 16g \
  --driver-memory 8g \
  --conf spark.sql.adaptive.enabled=true \
  --conf spark.yarn.maxAppAttempts=2 \
  --py-files dependencies.zip \
  my_spark_job.py \
  --date 2024-01-15 \
  --input s3://bucket/input/ \
  --output s3://bucket/output/

# Submit to Kubernetes
spark-submit \
  --master k8s://https://kubernetes-api-server:443 \
  --deploy-mode cluster \
  --conf spark.kubernetes.container.image=my-spark:3.5.0 \
  --conf spark.kubernetes.executor.volumes.persistentVolumeClaim... \
  my_spark_job.py
```

---

*Related: [[02-Core-Concepts/Batch-Processing]] | [[03-Tools/Hadoop]] | [[04-System-Design/ETL-Pipelines]] | Back to [[00-Index]]*
