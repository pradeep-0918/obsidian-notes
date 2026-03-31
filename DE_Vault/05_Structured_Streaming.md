# 05 — Spark Structured Streaming
#spark #streaming #watermark #kafka #checkpointing

**Roadmap Day:** 10 | [[DE_Vault/00_Master_Index]] | Prev → [[04_Apache_Spark]] | Next → [[06_Apache_Airflow]]

---

## 🗺️ Topic Map

```
Structured Streaming
├── Streaming Model (unbounded DataFrame)
├── Micro-batch vs Continuous Processing
├── Sources (Kafka, file, rate)
├── Output Modes (Append, Update, Complete)
├── Watermarking (late data)
├── Windowed Aggregations
├── Stateful Operations
├── Checkpointing & Fault Tolerance
└── foreachBatch (custom sinks)
```

---

## 1. Streaming Model

Structured Streaming treats a stream as an **unbounded DataFrame** — same API as batch, runs incrementally.

```python
# Read stream
stream_df = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "events") \
    .option("startingOffsets", "latest") \
    .load()

# Parse Kafka value (binary → string → JSON)
from pyspark.sql.types import StructType, StructField, StringType, LongType
from pyspark.sql import functions as F

schema = StructType([
    StructField("user_id", StringType()),
    StructField("event_type", StringType()),
    StructField("ts", LongType()),
])

parsed = stream_df \
    .select(F.from_json(F.col("value").cast("string"), schema).alias("data")) \
    .select("data.*") \
    .withColumn("event_time", F.from_unixtime("ts").cast("timestamp"))
```

---

## 2. Output Modes

| Mode | Use Case | Requirement |
|------|---------|------------|
| **Append** | New rows only, immutable past | No aggregation, or aggregation + watermark |
| **Update** | Only changed rows | Any aggregation |
| **Complete** | Entire result table | Only with aggregations (small result sets) |

---

## 3. Windowed Aggregations + Watermarking

```python
# Event-time windowed aggregation with watermark
result = parsed \
    .withWatermark("event_time", "10 minutes") \  # tolerate 10min late data
    .groupBy(
        F.window("event_time", "5 minutes"),       # 5-min tumbling window
        F.col("event_type")
    ) \
    .agg(F.count("*").alias("event_count"))

# Sliding window (every 1 min, covers 5 min)
result = parsed.groupBy(
    F.window("event_time", "5 minutes", "1 minute"),  # windowDuration, slideDuration
    "user_id"
).count()
```

> 💡 **Watermark = late data threshold.** Data arriving more than 10 minutes late is dropped. Watermark also determines when state is cleaned up — without it, state grows forever (OOM risk).

> 💡 **`Append` mode + watermark:** Only emits results after the window is guaranteed complete (watermark passes window end). Introduces latency but gives clean final results.

---

## 4. Checkpointing

```python
query = result.writeStream \
    .format("parquet") \
    .option("checkpointLocation", "s3://bucket/checkpoints/events/") \
    .option("path", "s3://bucket/output/events/") \
    .outputMode("append") \
    .trigger(processingTime="1 minute") \  # micro-batch every 1 min
    .start()

query.awaitTermination()
```

> 💡 **Checkpoint stores:** (1) Kafka offsets (exactly-once source tracking), (2) aggregation state (for stateful ops), (3) metadata (query config). Recovery restarts from last checkpoint.

---

## 5. foreachBatch — Custom Sinks

```python
def write_to_postgres_and_s3(df, batch_id):
    # Write to multiple destinations
    df.write.mode("append").jdbc(url, table, properties)
    df.write.mode("append").parquet(f"s3://bucket/batch_{batch_id}/")
    print(f"Processed batch {batch_id}: {df.count()} rows")

stream.writeStream \
    .foreachBatch(write_to_postgres_and_s3) \
    .outputMode("update") \
    .start()
```

> 💡 **`foreachBatch`:** Gives you a regular batch DataFrame — you can use all batch operations including joins, complex writes, and multiple sinks. Most flexible output option.

---

## 🃏 Interview Cheatsheet
- **Append vs Update vs Complete?** Append: only new final rows. Update: changed rows (any agg). Complete: full result table (only for small agg results).
- **Why watermark?** Controls late data handling AND triggers state cleanup. Without it, stateful ops hold state forever.
- **Micro-batch vs Continuous?** Micro-batch: Spark 2.0+, latency ~100ms. Continuous: Spark 2.3+, latency ~1ms, limited ops.
- **`foreachBatch` use?** When you need multiple sinks, batch-only ops (JDBC upserts), or complex write logic.
