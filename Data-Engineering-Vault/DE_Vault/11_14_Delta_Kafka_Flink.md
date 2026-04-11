# 11 — Delta Lake & Databricks
#delta-lake #databricks #acid #time-travel #merge #optimize #zorder #cdf

**Roadmap Day:** 19 | [[Data-Engineering-Vault/DE_Vault/00_Master_Index]] | Prev → [[10_dbt]] | Next → [[12_Apache_Kafka]]

---

## 🗺️ Topic Map

```
Delta Lake & Databricks
├── Delta Lake Architecture (transaction log)
├── ACID on Cloud Storage
├── MERGE INTO (upsert)
├── Time Travel
├── OPTIMIZE & ZORDER
├── Schema Enforcement & Evolution
├── Change Data Feed (CDF)
├── Liquid Clustering (Delta 3.x)
└── Unity Catalog (governance)
```

---

## 1. Delta Lake Architecture

```
Delta Table = Parquet files + _delta_log/ directory

s3://bucket/delta_table/
├── part-00000.parquet
├── part-00001.parquet
└── _delta_log/
    ├── 00000000000000000000.json   # version 0
    ├── 00000000000000000001.json   # version 1
    ├── 00000000000000000002.json   # version 2
    └── 00000000000000000010.checkpoint.parquet  # checkpoint every 10 commits
```

Each log entry records:
- **add**: new Parquet files added
- **remove**: files logically deleted (soft delete)
- **commitInfo**: operation type, timestamp, user, parameters

> 💡 **ACID via transaction log:** Serializable isolation through optimistic concurrency control on the log. Writers create a new log entry; if conflict detected, retry. Readers always see a consistent snapshot.

---

## 2. Core Delta Operations

```python
from delta.tables import DeltaTable
from pyspark.sql import functions as F

# Write Delta table
df.write.format("delta").mode("overwrite").partitionBy("date").save(path)

# Read
df = spark.read.format("delta").load(path)
spark.sql("CREATE TABLE events USING DELTA LOCATION 'path/'")

# MERGE INTO — upsert pattern
delta_table = DeltaTable.forPath(spark, path)

delta_table.alias("target").merge(
    updates_df.alias("source"),
    "target.id = source.id"
).whenMatchedUpdate(set={
    "email": "source.email",
    "updated_at": "source.updated_at"
}).whenNotMatchedInsertAll() \
  .whenNotMatchedBySourceDelete() \  # Delete rows in target not in source
  .execute()
```

---

## 3. Time Travel

```python
# Read at specific version
df_v0 = spark.read.format("delta").option("versionAsOf", 0).load(path)
df_yesterday = spark.read.format("delta") \
    .option("timestampAsOf", "2024-01-14 12:00:00").load(path)

# SQL syntax
spark.sql("SELECT * FROM events VERSION AS OF 5")
spark.sql("SELECT * FROM events TIMESTAMP AS OF '2024-01-14'")

# View history
delta_table.history().show(truncate=False)
spark.sql("DESCRIBE HISTORY events").show()

# Restore table to previous version
spark.sql("RESTORE TABLE events TO VERSION AS OF 3")
```

---

## 4. OPTIMIZE & ZORDER

```sql
-- Compact small files into larger ones
OPTIMIZE events;

-- ZORDER: co-locate related data in same files (like clustering key)
OPTIMIZE events ZORDER BY (user_id, event_date);

-- VACUUM: remove old Parquet files (default: 7 day retention)
VACUUM events RETAIN 168 HOURS;   -- 7 days
VACUUM events RETAIN 0 HOURS;     -- dangerous! removes all old files
```

> 💡 **ZORDER vs partition:** Partition = physical directory separation (good for low-cardinality like date). ZORDER = co-location within files (good for high-cardinality like user_id that you filter on). Use both.

> ⚠️ **VACUUM before ZORDER:** Run OPTIMIZE first to compact, then VACUUM to remove old small files. Don't VACUUM aggressively if Time Travel is needed.

---

## 5. Schema Enforcement & Evolution

```python
# Schema enforcement (default) — rejects writes with different schema
df_wrong_schema.write.format("delta").mode("append").save(path)
# AnalysisException: A schema mismatch detected

# Schema evolution — allow new columns
df_new_col.write.format("delta").mode("append") \
    .option("mergeSchema", "true").save(path)

# Overwrite with schema change
df.write.format("delta").mode("overwrite") \
    .option("overwriteSchema", "true").save(path)
```

---

## 6. Change Data Feed (CDF)

```sql
-- Enable CDF
ALTER TABLE events SET TBLPROPERTIES (delta.enableChangeDataFeed = true);

-- Read changes between versions
SELECT * FROM table_changes('events', 0, 5);
-- Extra columns: _change_type (insert/update_preimage/update_postimage/delete)
--                _commit_version, _commit_timestamp
```

---

## 🃏 Interview Cheatsheet
- **How does Delta provide ACID?** Optimistic concurrency on `_delta_log`. Atomic log commits = atomicity. Log-based versioning = isolation.
- **`OPTIMIZE ZORDER BY`?** Co-locates related data in same files → speeds up selective queries by reducing files scanned.
- **Delta vs Parquet?** Delta = Parquet + transaction log + ACID + Time Travel + schema enforcement + MERGE support.
- **When to VACUUM?** After OPTIMIZE or bulk deletes. Keep ≥7 days retention if using Time Travel.
- **CDF use case?** Feed downstream systems incrementally — only send changed rows, not full table scans.

---
---

# 12 — Apache Kafka
#kafka #streaming #producer #consumer #schema-registry #debezium #exactly-once

**Roadmap Days:** 22–23 | [[Data-Engineering-Vault/DE_Vault/00_Master_Index]] | Prev → [[11_Delta_Lake_Databricks]] | Next → [[13_Apache_Flink]]

---

## 🗺️ Topic Map

```
Apache Kafka
├── Core Concepts (Topics, Partitions, Offsets, Brokers)
├── Producers (acks, idempotent, batching)
├── Consumers & Consumer Groups
├── Brokers, Leaders, ISR, Replication
├── KRaft Mode (no ZooKeeper)
├── Kafka Connect & Debezium (CDC)
├── Schema Registry (Avro)
├── Exactly-Once Semantics
├── Consumer Lag Monitoring
├── Compacted Topics
└── Dead Letter Queues
```

---

## 1. Core Architecture

```
Topic: "orders"
┌──────────────────────────────────────────────────────┐
│ Partition 0: [msg0] [msg1] [msg2] [msg3] ...         │ → offset
│ Partition 1: [msg0] [msg1] [msg2] ...                │
│ Partition 2: [msg0] [msg1] ...                       │
└──────────────────────────────────────────────────────┘

Consumer Group "analytics":
  Consumer A → reads Partition 0
  Consumer B → reads Partition 1
  Consumer C → reads Partition 2
  
(Max parallelism = number of partitions)
```

| Concept | Definition |
|---------|-----------|
| **Topic** | Logical stream of messages (like a table) |
| **Partition** | Ordered, immutable log within a topic |
| **Offset** | Sequential ID of a message within a partition |
| **Broker** | Kafka server that stores partitions |
| **Leader** | Broker responsible for a partition's reads/writes |
| **ISR** | In-Sync Replicas — replicas caught up to leader |
| **Consumer Group** | Group sharing partitions; each partition → one consumer |

---

## 2. Producers

```python
from confluent_kafka import Producer
import json

conf = {
    'bootstrap.servers': 'localhost:9092',
    'acks': 'all',              # wait for all ISR to acknowledge
    'linger.ms': 5,             # wait up to 5ms to batch messages
    'batch.size': 16384,        # batch up to 16KB
    'compression.type': 'snappy',
    'enable.idempotence': True,  # exactly-once at producer level
    'max.in.flight.requests.per.connection': 5,
}

producer = Producer(conf)

def delivery_callback(err, msg):
    if err:
        print(f"Delivery failed: {err}")
    else:
        print(f"Delivered to {msg.topic()}[{msg.partition()}]@{msg.offset()}")

producer.produce(
    topic='orders',
    key=str(order['user_id']),       # determines partition
    value=json.dumps(order),
    callback=delivery_callback
)
producer.flush()   # wait for all in-flight messages
```

### Producer Settings
| Setting | Value | Effect |
|---------|-------|--------|
| `acks=0` | Fire-and-forget | Fastest, may lose data |
| `acks=1` | Leader ack | Balanced |
| `acks=all` (-1) | All ISR ack | Slowest, safest |
| `enable.idempotence=true` | Dedup per session | Prevents duplicate on retry |
| `linger.ms` | Milliseconds | Batch more messages together |

---

## 3. Consumers & Consumer Groups

```python
from confluent_kafka import Consumer

conf = {
    'bootstrap.servers': 'localhost:9092',
    'group.id': 'analytics-group',
    'auto.offset.reset': 'earliest',      # start from beginning if no committed offset
    'enable.auto.commit': False,           # manual commit (safer)
}

consumer = Consumer(conf)
consumer.subscribe(['orders'])

try:
    while True:
        msg = consumer.poll(timeout=1.0)
        if msg is None:
            continue
        if msg.error():
            handle_error(msg.error())
            continue
        
        process_message(msg.value())
        consumer.commit(msg)              # commit after processing
        
finally:
    consumer.close()
```

> ⚠️ **`auto.offset.reset`:** `earliest` = process all historical data. `latest` = only new messages. Choose based on whether you need backfill.

> 💡 **Manual commit pattern:** Commit AFTER processing, not before. Ensures at-least-once delivery. For exactly-once, need transactional writes.

---

## 4. Replication & Fault Tolerance

```
Topic partition 0, replication factor 3:
  Broker 1: Leader   (handles all reads + writes)
  Broker 2: Follower (replicates, in ISR)
  Broker 3: Follower (replicates, in ISR)

Leader failure → one ISR promoted to new leader automatically
min.insync.replicas = 2: Producer needs 2 brokers to ack (with acks=all)
```

---

## 5. Exactly-Once Semantics (EOS)

```python
# Transactional producer
conf = {
    ...
    'enable.idempotence': True,
    'transactional.id': 'etl-producer-1',  # unique per producer instance
}
producer = Producer(conf)
producer.init_transactions()

try:
    producer.begin_transaction()
    producer.produce(topic='output', value=processed_data)
    producer.send_offsets_to_transaction(offsets, group_metadata)
    producer.commit_transaction()
except Exception:
    producer.abort_transaction()
```

> 💡 **EOS requires:** Idempotent producer + Transactions + Consumer `isolation.level=read_committed` (skip uncommitted messages).

---

## 6. Kafka Connect & Debezium

```json
// Debezium PostgreSQL Source Connector config
{
  "name": "postgres-cdc-connector",
  "config": {
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "database.hostname": "postgres",
    "database.port": "5432",
    "database.user": "debezium",
    "database.password": "secret",
    "database.dbname": "orders_db",
    "database.server.name": "postgres",
    "table.include.list": "public.orders,public.customers",
    "slot.name": "debezium_slot",
    "plugin.name": "pgoutput",
    "topic.prefix": "cdc"
  }
}
```
Output topic: `cdc.public.orders` — receives every INSERT/UPDATE/DELETE as a message.

---

## 7. Schema Registry + Avro

```python
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroSerializer

schema_registry_conf = {'url': 'http://localhost:8081'}
schema_registry_client = SchemaRegistryClient(schema_registry_conf)

order_schema = """
{
  "type": "record",
  "name": "Order",
  "fields": [
    {"name": "order_id", "type": "long"},
    {"name": "amount", "type": "double"},
    {"name": "status", "type": "string"}
  ]
}
"""
```

### Schema Evolution Rules
| Change | BACKWARD | FORWARD | FULL |
|--------|---------|---------|------|
| Add optional field | ✅ | ✅ | ✅ |
| Add required field | ❌ | ✅ | ❌ |
| Remove field | ✅ | ❌ | ❌ |
| Rename field | ❌ | ❌ | ❌ |

---

## 8. Consumer Lag Monitoring

```bash
# CLI
kafka-consumer-groups.sh \
  --bootstrap-server localhost:9092 \
  --describe \
  --group analytics-group

# Output: TOPIC | PARTITION | CURRENT-OFFSET | LOG-END-OFFSET | LAG
# LAG = LOG-END-OFFSET - CURRENT-OFFSET
```

> 💡 **Lag alert threshold:** Depends on business SLA. Alert if lag grows continuously (consumer not keeping up) or exceeds X hours of data.

---

## 🃏 Interview Cheatsheet
- **`acks=all` + `min.insync.replicas=2`?** Producer waits for 2 brokers to acknowledge. ISR must have ≥2 replicas.
- **Consumer group rebalancing?** Triggered by: consumer join/leave, partition count change, heartbeat timeout. All consumers pause during rebalance.
- **Compacted topic?** Kafka retains only the latest message per key. Used for CDC changelog / key-value store semantics.
- **EOS end-to-end?** Idempotent producer + transactional producer + consumer `read_committed` isolation.
- **Debezium CDC?** Reads PostgreSQL WAL → publishes change events to Kafka. Captures INSERT/UPDATE/DELETE including soft deletes.
- **Dead letter queue?** Route "poison" messages (failed to process) to a separate topic for inspection/retry.

---
---

# 13 — Apache Flink
#flink #streaming #stateful #watermarks #windows #checkpointing

**Roadmap Day:** 24 | [[Data-Engineering-Vault/DE_Vault/00_Master_Index]] | Prev → [[12_Apache_Kafka]] | Next → [[14_GenAI_RAG_Pipelines]]

---

## 🗺️ Topic Map

```
Apache Flink
├── True Streaming vs Micro-batch
├── DataStream API
├── Flink SQL
├── Windows (Tumbling, Sliding, Session)
├── Watermarks (event time vs processing time)
├── State Backends (RocksDB, HashMap)
├── Checkpointing (exactly-once, 2-phase commit)
└── Kafka → Flink → Sink pattern
```

---

## 1. Flink vs Spark Streaming

| | Apache Flink | Spark Structured Streaming |
|--|------------|--------------------------|
| Model | True streaming (event-by-event) | Micro-batch (default, 100ms+) |
| Latency | ~1ms | ~100ms–1s |
| State management | Native, first-class | External (RDD state) |
| Exactly-once | Yes (2PC with sinks) | Yes (with Kafka) |
| SQL | Flink SQL | Spark SQL |
| Maturity | Production-grade | Production-grade |
| Use case | Low-latency, complex stateful | Batch + streaming unified |

---

## 2. Flink Windows

```java
// Tumbling window (fixed, non-overlapping)
stream.keyBy(event -> event.userId)
      .window(TumblingEventTimeWindows.of(Time.minutes(1)))
      .aggregate(new CountAggregate())

// Sliding window (overlapping)
stream.keyBy(...)
      .window(SlidingEventTimeWindows.of(Time.minutes(5), Time.minutes(1)))
      .sum("amount")

// Session window (gap-based — closes after inactivity)
stream.keyBy(event -> event.userId)
      .window(EventTimeSessionWindows.withGap(Time.minutes(30)))
      .process(new SessionProcessor())
```

---

## 3. Watermarks (Event Time)

```java
// Assign timestamps + watermarks
DataStream<Event> stream = env
    .fromSource(kafkaSource, WatermarkStrategy
        .<Event>forBoundedOutOfOrderness(Duration.ofSeconds(30))
        .withTimestampAssigner((event, ts) -> event.timestamp),
    "Kafka Source");
```

> 💡 **Watermark = event time - max_out_of_orderness.** Window fires when watermark passes window end time. Late events beyond watermark are dropped (or go to side output).

---

## 4. State Backends

| Backend | Storage | Best For |
|---------|---------|---------|
| **HashMapStateBackend** | JVM heap | Small state, low latency |
| **EmbeddedRocksDBStateBackend** | Off-heap (RocksDB) | Large state, persistent |

```java
env.setStateBackend(new EmbeddedRocksDBStateBackend());
env.getCheckpointConfig().setCheckpointStorage("s3://bucket/flink-checkpoints/");
```

---

## 5. Checkpointing — Exactly-Once

```java
// Enable checkpointing
env.enableCheckpointing(60_000);  // every 60 seconds
env.getCheckpointConfig().setCheckpointingMode(CheckpointingMode.EXACTLY_ONCE);
env.getCheckpointConfig().setMinPauseBetweenCheckpoints(30_000);
env.getCheckpointConfig().setTolerableCheckpointFailureNumber(3);
```

> 💡 **Two-phase commit (2PC):** For exactly-once sink delivery, Flink uses 2PC — pre-commit on checkpoint, commit when checkpoint completes. Supported by Kafka sink, JDBC sink.

---

## 🃏 Interview Cheatsheet
- **Flink vs Spark?** Flink: true streaming, ~1ms latency, native state. Spark SS: micro-batch, simpler, unified batch+stream API.
- **Tumbling vs Session window?** Tumbling: fixed duration, non-overlapping. Session: gap-based, closes after inactivity period.
- **RocksDB state backend?** Stores state on disk (off-heap). Handles state larger than JVM heap. Required for production stateful jobs.
- **Exactly-once with Kafka?** Flink reads with tracked offsets + writes with 2PC transactional Kafka producer. Checkpoint = commit point.
