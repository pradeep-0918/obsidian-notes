# 🌊 Stream Processing — Complete Guide
#concept #streaming #kafka #flink #spark

> Process data as it arrives, not hours later. The difference between "yesterday's insights" and "right now."

---

## 🧠 The Mental Model

**Batch processing** = Filling a bathtub, then analyzing the water.
**Stream processing** = Analyzing the water as it flows through a pipe.

```
BATCH:
Events: ──[e1]──[e2]──[e3]──[e4]──[e5]──►
                                          │
                              Collect all │
                              Process all ▼
                               [Result]

STREAM:
Events: ──[e1]──[e2]──[e3]──[e4]──[e5]──►
                │    │    │    │    │
              [r1] [r2] [r3] [r4] [r5]  (results flow continuously)
```

---

## ⏰ Time in Streaming

Understanding time is critical. There are two clocks:

- **Event time** = When the event actually happened (on the device)
- **Processing time** = When your system saw the event

```
Event happens    Event arrives    Event processed
at 10:00:00      at 10:00:05      at 10:00:06
     │                │                │
  Event Time      ← 5s lag →    Processing Time
```

**Why this matters:**
- A mobile app event might be delayed by hours (offline device)
- Network latency adds milliseconds to seconds
- If you use processing time for "hourly aggregations", an event from 9:59 that arrives at 10:01 goes in the wrong bucket

**Solution: Watermarks**
```python
# Flink: Tell the system "events up to T-2min are guaranteed complete"
stream \
    .assign_timestamps_and_watermarks(
        WatermarkStrategy
            .for_bounded_out_of_orderness(Duration.of_seconds(120))
            .with_timestamp_assigner(EventTimestampAssigner())
    )
```

---

## 🪟 Windows — The Core Abstraction

Windows group infinite stream data into finite chunks for processing.

### Tumbling Windows
```
Non-overlapping, fixed size:

Time: ──0──1──2──3──4──5──6──7──8──9──►
       [──── Window 1 ────][──── Window 2 ────]
       
Events in 0-4 = Window 1 result
Events in 5-9 = Window 2 result
```

### Sliding Windows
```
Overlapping, fixed size and slide interval:

Time: ──0──1──2──3──4──5──6──7──►
       [──────────────]           Window 1 (0-4)
              [──────────────]    Window 2 (2-6)
                     [──────────] Window 3 (4-8)
                     
Use case: "Revenue in last 30 minutes, updated every 5 minutes"
```

### Session Windows
```
Dynamic, gap-based:

User activity: ─[click]──[click]──[click]──────────────[click]──[click]─
                ←─────── Session 1 ──────→  ← 30min gap → ← Session 2 →

Use case: "Group user activity into sessions with 30min inactivity gap"
```

---

## ⚡ Apache Flink Deep Dive

Flink is the gold standard for stateful stream processing.

```python
from pyflink.datastream import StreamExecutionEnvironment
from pyflink.datastream.connectors.kafka import KafkaSource, KafkaSink
from pyflink.common import WatermarkStrategy, Duration
from pyflink.datastream.window import TumblingEventTimeWindows, Time

env = StreamExecutionEnvironment.get_execution_environment()
env.set_parallelism(4)

# Source
source = KafkaSource.builder() \
    .set_bootstrap_servers("kafka:9092") \
    .set_topics("orders") \
    .set_group_id("flink-analytics") \
    .set_value_only_deserializer(SimpleStringSchema()) \
    .build()

stream = env.from_source(
    source,
    WatermarkStrategy.for_bounded_out_of_orderness(Duration.of_seconds(30)),
    "Kafka Source"
)

# Parse and process
from pyflink.datastream.functions import MapFunction, ReduceFunction

class ParseOrder(MapFunction):
    def map(self, value):
        import json
        data = json.loads(value)
        return (data['region'], float(data['amount']))

# Window aggregation
revenue_by_region = stream \
    .map(ParseOrder()) \
    .key_by(lambda x: x[0]) \  # Key by region
    .window(TumblingEventTimeWindows.of(Time.minutes(5))) \
    .reduce(lambda a, b: (a[0], a[1] + b[1]))  # Sum amounts

# Sink
from pyflink.datastream.connectors.kafka import KafkaSink
sink = KafkaSink.builder() \
    .set_bootstrap_servers("kafka:9092") \
    .set_record_serializer(
        KafkaRecordSerializationSchema.builder()
            .set_topic("revenue_by_region")
            .set_value_serialization_schema(SimpleStringSchema())
            .build()
    ) \
    .build()

revenue_by_region.sink_to(sink)
env.execute("Revenue by Region")
```

### Flink State Management

```python
from pyflink.datastream.state import ValueStateDescriptor, ListStateDescriptor
from pyflink.datastream.functions import KeyedProcessFunction

class UserSessionAggregator(KeyedProcessFunction):
    """Aggregate user actions into sessions."""
    
    def open(self, runtime_context):
        # State persisted per key (user_id)
        self.session_events = runtime_context.get_list_state(
            ListStateDescriptor("events", Types.STRING())
        )
        self.session_start = runtime_context.get_state(
            ValueStateDescriptor("session_start", Types.LONG())
        )
        self.timer_registered = runtime_context.get_state(
            ValueStateDescriptor("timer", Types.LONG())
        )
    
    def process_element(self, event, ctx):
        # Add event to session
        self.session_events.add(json.dumps(event))
        
        # Set session start if new session
        if not self.session_start.value():
            self.session_start.update(event['timestamp'])
        
        # Register a timer to close session after 30min inactivity
        inactivity_deadline = ctx.timestamp() + 30 * 60 * 1000
        
        # Clear existing timer if any
        if self.timer_registered.value():
            ctx.timer_service().delete_event_time_timer(
                self.timer_registered.value()
            )
        
        ctx.timer_service().register_event_time_timer(inactivity_deadline)
        self.timer_registered.update(inactivity_deadline)
    
    def on_timer(self, timestamp, ctx, out):
        """Session ended - emit aggregated result."""
        events = list(self.session_events.get())
        out.collect({
            'user_id': ctx.get_current_key(),
            'session_start': self.session_start.value(),
            'session_end': timestamp,
            'event_count': len(events),
            'events': events
        })
        # Clear state
        self.session_events.clear()
        self.session_start.clear()
        self.timer_registered.clear()
```

---

## ⚡ Spark Streaming

Spark Streaming (Structured Streaming) integrates seamlessly with your batch Spark code.

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.types import *

spark = SparkSession.builder \
    .appName("OrderStreamProcessor") \
    .config("spark.sql.streaming.checkpointLocation", "s3://bucket/checkpoints/") \
    .getOrCreate()

# Schema for incoming JSON
schema = StructType([
    StructField("order_id", StringType()),
    StructField("customer_id", StringType()),
    StructField("amount", DoubleType()),
    StructField("region", StringType()),
    StructField("timestamp", TimestampType())
])

# Read from Kafka
raw_stream = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "kafka:9092") \
    .option("subscribe", "orders") \
    .option("startingOffsets", "latest") \
    .load()

# Parse JSON
orders = raw_stream \
    .select(F.from_json(F.col("value").cast("string"), schema).alias("data")) \
    .select("data.*") \
    .withWatermark("timestamp", "2 minutes")

# Windowed aggregation
revenue_stream = orders \
    .groupBy(
        F.window("timestamp", "5 minutes", "1 minute"),
        "region"
    ) \
    .agg(
        F.sum("amount").alias("total_revenue"),
        F.count("order_id").alias("order_count")
    )

# Write to multiple sinks
# Sink 1: Real-time dashboard (console for dev)
console_query = revenue_stream.writeStream \
    .outputMode("update") \
    .format("console") \
    .option("truncate", False) \
    .start()

# Sink 2: Delta Lake (persistent)
delta_query = revenue_stream.writeStream \
    .outputMode("append") \
    .format("delta") \
    .option("path", "s3://bucket/delta/revenue_windows/") \
    .trigger(processingTime="1 minute") \
    .start()

spark.streams.awaitAnyTermination()
```

---

## 🔄 Delivery Guarantees

This is critical to understand before building production pipelines:

| Guarantee | Meaning | Use When |
|---|---|---|
| At-most-once | May lose messages, never duplicates | Loss OK (metrics approximations) |
| At-least-once | Never loses, may duplicate | Business events (dedup at destination) |
| Exactly-once | Never loses, never duplicates | Financial transactions |

**Exactly-once in practice:**
```python
# Idempotent writes (exactly-once effect without exactly-once semantics)
def upsert_to_postgres(records):
    """UPSERT is inherently idempotent."""
    cursor.executemany("""
        INSERT INTO orders (order_id, amount, status)
        VALUES (%(order_id)s, %(amount)s, %(status)s)
        ON CONFLICT (order_id)
        DO UPDATE SET
            amount = EXCLUDED.amount,
            status = EXCLUDED.status,
            updated_at = NOW()
    """, records)
```

---

## 🔀 Stream-Stream Joins

```python
# Join two streams (e.g., orders + payments)
orders_stream = spark.readStream.format("kafka") \
    .option("subscribe", "orders").load() \
    .withWatermark("timestamp", "10 minutes")

payments_stream = spark.readStream.format("kafka") \
    .option("subscribe", "payments").load() \
    .withWatermark("timestamp", "10 minutes")

joined = orders_stream.join(
    payments_stream,
    on=[
        orders_stream.order_id == payments_stream.order_id,
        # Event time condition (within 10 minutes of each other)
        orders_stream.timestamp.between(
            payments_stream.timestamp - F.expr("INTERVAL 10 MINUTES"),
            payments_stream.timestamp + F.expr("INTERVAL 10 MINUTES")
        )
    ],
    how="inner"
)
```

---

## 📊 Streaming Architecture Patterns

### Pattern 1: Event-Driven Microservices
```
Order Service → Kafka[orders] → Inventory Service
                             → Email Service
                             → Analytics Pipeline
```

### Pattern 2: CQRS (Command Query Responsibility Segregation)
```
Write Path:  API → Kafka → Event Store
Read Path:   Kafka → Flink → Read Model (Redis/ES) → API
```

### Pattern 3: Streaming ETL
```
Source DB → CDC (Debezium) → Kafka → Flink → Data Warehouse
```

---

*Related: [[03-Tools/Kafka]] | [[03-Tools/Flink]] | [[04-System-Design/Real-Time-Architecture]] | Back to [[00-Index]]*
