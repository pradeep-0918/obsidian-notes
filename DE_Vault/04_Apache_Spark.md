# 04 — Apache Spark Deep Dive
#spark #pyspark #optimization #catalyst #tungsten #aqe #interview

**Roadmap Days:** 8–9 | [[DE_Vault/00_Master_Index]] | Prev → [[03_Linux_Shell]] | Next → [[05_Structured_Streaming]]

---

## 🗺️ Topic Map

```
Apache Spark
├── Architecture (Driver, Executor, DAG, Stage, Task)
├── Lazy Evaluation (Transformations vs Actions)
├── Catalyst Optimizer & Tungsten Engine
├── DataFrame API
├── Spark SQL
├── File Formats (Parquet, Delta, ORC)
├── Partitioning (repartition vs coalesce)
├── Broadcast Joins
├── Shuffle & Skew (Salting)
├── Caching & Persistence
├── AQE (Adaptive Query Execution)
├── Spark UI Reading
└── UDFs vs Pandas UDFs
```

---

## 1. Architecture — Draw It From Memory

```
┌─────────────────────────────────────────┐
│              Driver Program              │
│  ┌──────────┐  ┌────────────────────┐   │
│  │SparkContext│  │  DAG Scheduler     │   │
│  └──────────┘  │  (logical → physical│   │
│                │   plan, stages)     │   │
│                └────────────────────┘   │
└─────────────────┬───────────────────────┘
                  │ (submits tasks)
         ┌────────▼────────┐
         │  Cluster Manager │  (YARN / K8s / Standalone)
         └────────┬────────┘
    ┌─────────────┼─────────────┐
    ▼             ▼             ▼
┌────────┐  ┌────────┐  ┌────────┐
│Executor│  │Executor│  │Executor│
│  Core  │  │  Core  │  │  Core  │
│  Core  │  │  Core  │  │  Core  │
│ (tasks)│  │ (tasks)│  │ (tasks)│
└────────┘  └────────┘  └────────┘
```

### Key Concepts
| Term | Definition |
|------|-----------|
| **Driver** | JVM process that coordinates the job; holds SparkContext |
| **Executor** | JVM process on worker node; runs tasks, stores cached data |
| **DAG** | Directed Acyclic Graph of RDD transformations |
| **Stage** | Group of tasks with no shuffle between them |
| **Task** | Smallest unit of work; runs on one partition on one core |
| **Partition** | Chunk of data; more partitions = more parallelism |
| **Shuffle** | Data redistribution across executors (expensive!) |

> 💡 **One task = one partition = one core.** If you have 100 partitions and 50 cores, you need 2 waves of tasks. Optimal: `num_partitions ≈ 2-4x num_cores`.

---

## 2. Lazy Evaluation

### Transformations (lazy — build DAG, don't execute)
```python
# Narrow transformations (no shuffle, one partition → one partition)
df.filter()      df.select()     df.withColumn()
df.map()         df.flatMap()    df.sample()
df.union()       df.limit()      df.coalesce()   # < fewer partitions, no shuffle

# Wide transformations (require shuffle, many → many partitions)
df.groupBy()     df.agg()        df.join()
df.distinct()    df.orderBy()    df.repartition() # > or < partitions, always shuffles
df.cube()        df.rollup()     df.crossJoin()
```

### Actions (trigger execution)
```python
df.show()       df.collect()    df.count()
df.take(n)      df.first()      df.head()
df.write...     df.foreach()    df.toLocalIterator()
df.toPandas()   df.summary()    df.describe()
```

> ⚠️ **`.collect()` OOM risk:** Pulls all data to driver. Never use on large DataFrames. Use `.show()`, `.limit()`, or write to file instead.

> 💡 **Lazy evaluation benefit:** Spark can optimize the entire DAG before running. Catalyst can push filters down, reorder joins, and eliminate unnecessary computations.

---

## 3. Catalyst Optimizer & Tungsten Engine

### Catalyst Pipeline
```
SQL / DataFrame API
        ↓
Unresolved Logical Plan   (parse)
        ↓
Resolved Logical Plan     (analysis — check schema, types)
        ↓
Optimized Logical Plan    (Catalyst rules: predicate pushdown, constant folding)
        ↓
Physical Plans            (multiple options)
        ↓
Cost Model Selection      (choose best physical plan)
        ↓
Code Generation (Tungsten) → JVM bytecode
```

### Key Catalyst Optimizations
| Optimization | What It Does |
|-------------|-------------|
| Predicate Pushdown | Moves filters as early as possible (before joins, into file readers) |
| Column Pruning | Reads only needed columns from Parquet/ORC |
| Constant Folding | `WHERE x + 2 > 5` → `WHERE x > 3` at plan time |
| Join Reordering | Smaller tables joined first |
| Broadcast Join | Auto-broadcasts small tables |

### Tungsten
- Code generation: Spark generates JVM bytecode at runtime for expression evaluation
- Cache-aware computation: processes data in CPU cache-friendly order
- Off-heap memory: avoids JVM GC overhead for large data
- Vectorized execution (Spark 3.x): SIMD batch operations

> 💡 **Why Catalyst matters in interviews:** Shows you understand why Spark is often faster than hand-written MapReduce. The optimizer does things you can't manually.

---

## 4. DataFrame API

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.window import Window
from pyspark.sql.types import StructType, StructField, StringType, DoubleType, LongType

spark = SparkSession.builder \
    .appName("DE_Job") \
    .config("spark.sql.adaptive.enabled", "true") \
    .getOrCreate()

# Schema Definition (always define schema — don't infer in production)
schema = StructType([
    StructField("user_id", LongType(), nullable=False),
    StructField("event_type", StringType(), nullable=True),
    StructField("amount", DoubleType(), nullable=True),
    StructField("ts", LongType(), nullable=True),
])

df = spark.read.schema(schema).json("s3://bucket/events/")

# Core operations
df = df \
    .filter(F.col("amount") > 0) \
    .select("user_id", "event_type", "amount",
            F.from_unixtime("ts").alias("event_time")) \
    .withColumn("amount_usd", F.col("amount") / 100) \
    .withColumn("date", F.to_date("event_time")) \
    .dropDuplicates(["user_id", "event_type", "date"])

# Aggregations
summary = df.groupBy("user_id", "date") \
    .agg(
        F.count("*").alias("event_count"),
        F.sum("amount_usd").alias("total_spend"),
        F.countDistinct("event_type").alias("unique_events"),
        F.collect_set("event_type").alias("event_types"),
    )

# Window Functions
window = Window.partitionBy("user_id").orderBy("event_time")
df = df.withColumn("prev_amount", F.lag("amount").over(window)) \
       .withColumn("running_total", F.sum("amount").over(
           window.rowsBetween(Window.unboundedPreceding, Window.currentRow)
       ))

# Joins
enriched = df.join(users_df, "user_id", "left") \
             .join(F.broadcast(country_df), "country_code", "inner")
```

---

## 5. Partitioning

### repartition vs coalesce
| | `repartition(n)` | `coalesce(n)` |
|-|-----------------|---------------|
| Direction | Increase or decrease | Only decrease |
| Shuffle | Always shuffles | No shuffle (in most cases) |
| Distribution | Balanced | Unbalanced (merges local partitions) |
| Use case | Need evenly distributed data | Reduce before write (no shuffle) |
| Cost | High (shuffle) | Low |

```python
# Repartition — before heavy join/groupBy to distribute data
df = df.repartition(200, "user_id")  # partition by column for co-location

# Coalesce — before writing to reduce output files
df.coalesce(1).write.parquet("output/")  # single file output

# Check partition count
df.rdd.getNumPartitions()

# Partition tuning rule of thumb
# Each partition: 100-200 MB of data in memory
# Formula: total_data_size_MB / 150 = num_partitions
```

### Partition Pruning
```python
# Write with partitioning
df.write.partitionBy("year", "month", "day").parquet("s3://bucket/events/")

# Read with filter — Spark reads only matching partitions (no full scan!)
df = spark.read.parquet("s3://bucket/events/") \
         .filter((F.col("year") == 2024) & (F.col("month") == 1))
```

> 💡 **Partition column cardinality:** High cardinality (user_id) → millions of tiny files. Use date-based partitioning (year/month/day) for time-series data.

---

## 6. Broadcast Joins

```python
from pyspark.sql.functions import broadcast

# Auto-broadcast threshold (default 10MB)
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", "50MB")

# Manual broadcast hint
result = large_df.join(broadcast(small_lookup_df), "country_code")

# Check if broadcast is used in plan
result.explain(True)  # look for "BroadcastHashJoin"

# Disable auto-broadcast (force SortMergeJoin)
spark.conf.set("spark.sql.autoBroadcastJoinThreshold", "-1")
```

> 💡 **When to broadcast:** The "small" table fits in executor memory. Rule: < 200-500MB (depending on cluster). Broadcasting avoids shuffle entirely.

> ⚠️ **Driver OOM:** Broadcast collects the small table to the driver first, then sends to all executors. If it's too large, driver crashes. Watch driver memory.

---

## 7. Shuffle & Skew — Salting

### What triggers shuffle?
- `groupBy`, `agg`, `distinct`, `orderBy`
- `join` (unless broadcast)
- `repartition`
- `cogroup`, `intersection`, `subtract`

### Diagnosing Skew
1. Open Spark UI → Stages → look for one task taking 10x longer than others
2. Check task metrics: "Shuffle Read" of one task >> others

### Salting Fix for Skewed Joins
```python
import random

# Add random salt to skewed key (0-9)
SALT_BUCKETS = 10

skewed_df = skewed_df.withColumn(
    "salted_key",
    F.concat(F.col("skewed_col"), F.lit("_"), (F.rand() * SALT_BUCKETS).cast("int"))
)

# Explode the small table to match all salts
from pyspark.sql.functions import array, explode, lit
small_df = small_df.withColumn("salt_array", array([lit(i) for i in range(SALT_BUCKETS)])) \
                   .withColumn("salt", explode("salt_array")) \
                   .withColumn("salted_key", F.concat(F.col("join_key"), F.lit("_"), F.col("salt"))) \
                   .drop("salt_array", "salt")

result = skewed_df.join(small_df, "salted_key")
```

> 💡 **AQE Skew Join:** Spark 3.x AQE can auto-handle skew joins without manual salting! `spark.sql.adaptive.skewJoin.enabled = true` (default). But knowing manual salting = interview gold.

---

## 8. Caching & Persistence

```python
from pyspark import StorageLevel

# cache() = persist(MEMORY_AND_DISK)
df.cache()
df.persist()

# Explicit storage levels
df.persist(StorageLevel.MEMORY_ONLY)          # Fastest, OOM risk
df.persist(StorageLevel.MEMORY_AND_DISK)      # Spills to disk
df.persist(StorageLevel.MEMORY_ONLY_SER)      # Serialized = less memory
df.persist(StorageLevel.DISK_ONLY)            # Slowest, for large DFs
df.persist(StorageLevel.MEMORY_AND_DISK_2)    # Replicated (fault tolerant)

# Always unpersist when done
df.unpersist()

# Check if cached
df.is_cached
spark.catalog.isCached("table_name")
```

### When to Cache?
- ✅ DataFrame used 2+ times in the same job
- ✅ Iterative algorithms (ML training)
- ✅ Expensive upstream operations (joins, heavy aggregations) reused
- ❌ Used only once
- ❌ Data larger than available executor memory (use DISK_ONLY then)

---

## 9. AQE — Adaptive Query Execution (Spark 3.x)

```python
# Enable AQE (default true in Spark 3.2+)
spark.conf.set("spark.sql.adaptive.enabled", "true")
```

### What AQE Does
| Feature | Description | Config |
|---------|------------|--------|
| **Auto Coalesce** | Merges small shuffle partitions after shuffle | `spark.sql.adaptive.coalescePartitions.enabled` |
| **Skew Join Opt** | Splits skewed partitions automatically | `spark.sql.adaptive.skewJoin.enabled` |
| **Auto Broadcast** | Upgrades SortMergeJoin to BroadcastJoin if runtime size is small | `spark.sql.adaptive.localShuffleReader.enabled` |

> 💡 **AQE uses runtime statistics:** Unlike static optimizer (Catalyst), AQE re-plans mid-execution based on actual shuffle data sizes. Very powerful for unpredictable data distributions.

---

## 10. Reading the Spark UI

**Access:** `http://driver-host:4040`

### Key Tabs
| Tab | What to Look For |
|-----|----------------|
| **Jobs** | Job completion, failures |
| **Stages** | Tasks per stage, shuffle read/write, GC time |
| **Storage** | Cached DataFrames and memory usage |
| **Executors** | Task failures, GC time, spill to disk |
| **SQL** | Query plan visualization (physical plan) |
| **Environment** | Config values |

### Red Flags in Spark UI
- 🔴 One task runtime >> others → **data skew**
- 🔴 High GC time (>20%) → **too little executor memory or too many objects**
- 🔴 "Spill to Disk" → **not enough memory, reduce parallelism or add memory**
- 🔴 Many failed tasks → **OOM, bad data, or network issues**
- 🔴 Long "Scheduler Delay" → **too many small tasks**

---

## 11. UDFs vs Pandas UDFs

```python
from pyspark.sql.functions import udf, pandas_udf
from pyspark.sql.types import StringType
import pandas as pd

# Regular UDF — slow (Python ↔ JVM serialization per row)
@udf(returnType=StringType())
def classify_amount(amount: float) -> str:
    return "high" if amount > 100 else "low"

# Pandas UDF — vectorized, uses Apache Arrow, much faster
@pandas_udf(StringType())
def classify_amount_fast(amounts: pd.Series) -> pd.Series:
    return amounts.apply(lambda x: "high" if x > 100 else "low")

# Even better — use native Spark functions when possible
df.withColumn("category", F.when(F.col("amount") > 100, "high").otherwise("low"))
```

| Type | Speed | When to Use |
|------|-------|------------|
| Native Spark `functions` | ⚡⚡⚡ Fastest | Always prefer — Catalyst optimizes |
| Pandas UDF | ⚡⚡ Fast | Complex logic, using pandas/numpy |
| Regular UDF | ⚡ Slow | Last resort only |

> ⚠️ **UDF black box:** Spark cannot optimize inside UDFs. No predicate pushdown, no constant folding. Always try to express logic with native `pyspark.sql.functions` first.

---

## 🃏 Interview Cheatsheet
- **What triggers a shuffle?** Wide transformations: `groupBy`, `join` (non-broadcast), `orderBy`, `distinct`, `repartition`.
- **`repartition` vs `coalesce`?** repartition: full shuffle, balanced. coalesce: no shuffle, unbalanced, only decrease.
- **Broadcast join threshold?** Default 10MB, configurable. Avoids shuffle by broadcasting small table to all executors.
- **What is AQE?** Re-plans query mid-execution using runtime statistics. Auto-coalesces partitions, handles skew, upgrades joins.
- **`cache()` vs `persist(DISK_ONLY)`?** cache = MEMORY_AND_DISK. persist(DISK_ONLY) = never in memory, slowest but safe for huge data.
- **How to debug slow Spark job?** Spark UI → find stage with high task runtime variance (skew) or high GC time or disk spill.
- **Salting for skew?** Add random salt suffix to skewed key + explode small table × salt range, then join on salted key.
