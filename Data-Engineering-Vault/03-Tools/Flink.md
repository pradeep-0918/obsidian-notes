# 🌊 Apache Flink — Complete Guide
#tool #flink #streaming #stateful #realtime

> The most powerful stateful stream processing engine. When Spark Streaming isn't enough.

---

## 🧠 Why Flink vs Spark Streaming?

| Feature | Spark Structured Streaming | Apache Flink |
|---|---|---|
| Processing model | Micro-batch (default) | True streaming |
| Latency | ~100ms-1s | ~1-100ms |
| State management | Limited | Rich, scalable |
| Event time | Yes | Yes (stronger) |
| Exactly-once | Yes (with caveats) | Yes (native) |
| ML on streams | MLlib | Flink ML |
| SQL support | Strong | Strong |
| Maturity | Very mature | Very mature |

**Choose Flink when:**
- Sub-second latency required
- Complex stateful operations (user sessions, fraud detection)
- Need very accurate event-time processing
- Continuous (not micro-batch) semantics required

---

## 🏗️ Flink Architecture

```
┌───────────────────────────────────────────────────────┐
│                   FLINK CLUSTER                        │
│                                                       │
│  ┌─────────────────────────────────────────────┐     │
│  │            JobManager (Master)               │     │
│  │  - Schedules tasks                          │     │
│  │  - Coordinates checkpoints                  │     │
│  │  - Manages failures                         │     │
│  └─────────────────────────────────────────────┘     │
│                         │                            │
│     ┌───────────────────┼───────────────────┐        │
│     ▼                   ▼                   ▼        │
│  ┌────────┐         ┌────────┐         ┌────────┐    │
│  │TaskMgr │         │TaskMgr │         │TaskMgr │    │
│  │  Task  │         │  Task  │         │  Task  │    │
│  │  Task  │         │  Task  │         │  Task  │    │
│  │  State │         │  State │         │  State │    │
│  └────────┘         └────────┘         └────────┘    │
└───────────────────────────────────────────────────────┘

State backends:
- HashMapStateBackend (in-memory, fast, not persistent)
- RocksDBStateBackend (in-memory + disk, persistent, scalable)
```

---

## 🐍 Flink DataStream API

```python
from pyflink.datastream import StreamExecutionEnvironment, TimeCharacteristic
from pyflink.datastream.connectors.kafka import KafkaSource, KafkaSink
from pyflink.common import WatermarkStrategy, Duration, SimpleStringSchema
from pyflink.datastream.functions import MapFunction, FilterFunction, \
    KeyedProcessFunction, ProcessWindowFunction
from pyflink.datastream.window import TumblingEventTimeWindows, SlidingEventTimeWindows
from pyflink.common.time import Time
import json

# ── SETUP ─────────────────────────────────────────────────────────

env = StreamExecutionEnvironment.get_execution_environment()
env.set_parallelism(4)
env.enable_checkpointing(60000)  # Checkpoint every 60 seconds
env.get_checkpoint_config().set_checkpoint_storage("s3://bucket/flink-checkpoints/")

# ── KAFKA SOURCE ──────────────────────────────────────────────────

kafka_source = KafkaSource.builder() \
    .set_bootstrap_servers("kafka:9092") \
    .set_topics("orders") \
    .set_group_id("flink-order-processor") \
    .set_starting_offsets(KafkaOffsetsInitializer.latest()) \
    .set_value_only_deserializer(SimpleStringSchema()) \
    .build()

# Assign timestamps and watermarks from event data
watermark_strategy = WatermarkStrategy \
    .for_bounded_out_of_orderness(Duration.of_seconds(30)) \
    .with_timestamp_assigner(
        # Extract timestamp from message
        lambda event_str, record_timestamp: 
            json.loads(event_str).get('timestamp', record_timestamp)
    )

stream = env.from_source(kafka_source, watermark_strategy, "Kafka Orders")

# ── TRANSFORMATIONS ───────────────────────────────────────────────

class ParseOrder(MapFunction):
    def map(self, value: str) -> dict:
        return json.loads(value)

class ValidOrder(FilterFunction):
    def filter(self, order: dict) -> bool:
        return (
            order.get('amount', 0) > 0
            and order.get('customer_id') is not None
            and order.get('status') in ('created', 'confirmed')
        )

class EnrichOrder(MapFunction):
    def map(self, order: dict) -> dict:
        order['revenue'] = order['amount'] * (1 - order.get('discount', 0))
        order['is_large'] = order['revenue'] > 1000
        return order

# Apply transformations
orders = stream \
    .map(ParseOrder()) \
    .filter(ValidOrder()) \
    .map(EnrichOrder())

# ── KEYED STREAM (state per key) ──────────────────────────────────

# Key by customer — ensures same customer goes to same task
customer_stream = orders.key_by(lambda o: o['customer_id'])

# ── WINDOWING ─────────────────────────────────────────────────────

class RevenueAggregator(ProcessWindowFunction):
    def process(self, key, context, elements, out):
        total = sum(e['revenue'] for e in elements)
        count = sum(1 for _ in elements)
        out.collect({
            'customer_id': key,
            'window_start': context.window().start,
            'window_end': context.window().end,
            'total_revenue': total,
            'order_count': count,
        })

# 5-minute tumbling window per customer
windowed = customer_stream \
    .window(TumblingEventTimeWindows.of(Time.minutes(5))) \
    .process(RevenueAggregator())

# ── STATEFUL PROCESSING ───────────────────────────────────────────

from pyflink.datastream.state import ValueStateDescriptor, ListStateDescriptor
from pyflink.common.typeinfo import Types

class FraudDetector(KeyedProcessFunction):
    """Detect fraud patterns in real-time."""
    
    def open(self, runtime_context):
        self.recent_amounts = runtime_context.get_list_state(
            ListStateDescriptor("recent_amounts", Types.DOUBLE())
        )
        self.last_order_time = runtime_context.get_state(
            ValueStateDescriptor("last_time", Types.LONG())
        )
    
    def process_element(self, order, ctx, out):
        current_time = ctx.timestamp()
        last_time = self.last_order_time.value() or 0
        
        # Add to recent amounts
        self.recent_amounts.add(order['amount'])
        self.last_order_time.update(current_time)
        
        # Get recent amounts (last N)
        amounts = list(self.recent_amounts.get())
        
        # Alert if: > 5 orders in 60 seconds
        if current_time - last_time < 60_000 and len(amounts) > 5:
            out.collect({
                'alert_type': 'HIGH_FREQUENCY',
                'customer_id': order['customer_id'],
                'order_count': len(amounts),
                'timestamp': current_time
            })
        
        # Alert if: sudden large order (> 10x average)
        if len(amounts) > 3:
            avg = sum(amounts[:-1]) / len(amounts[:-1])
            if order['amount'] > avg * 10:
                out.collect({
                    'alert_type': 'AMOUNT_SPIKE',
                    'customer_id': order['customer_id'],
                    'amount': order['amount'],
                    'avg_historical': avg,
                    'timestamp': current_time
                })
        
        # Clean up old state (keep last 10 orders)
        if len(amounts) > 10:
            self.recent_amounts.clear()
            for a in amounts[-10:]:
                self.recent_amounts.add(a)

fraud_alerts = customer_stream.process(FraudDetector())

# ── SIDE OUTPUTS (route different outputs) ────────────────────────

from pyflink.datastream.output_tag import OutputTag

large_order_tag = OutputTag("large-orders")

class SplitBySize(ProcessFunction):
    def process_element(self, order, ctx, out):
        if order['revenue'] > 1000:
            ctx.output(large_order_tag, order)
        else:
            out.collect(order)

split_stream = orders.process(SplitBySize())
large_orders = split_stream.get_side_output(large_order_tag)
regular_orders = split_stream  # Main output

# ── SINKS ─────────────────────────────────────────────────────────

# Kafka sink
kafka_sink = KafkaSink.builder() \
    .set_bootstrap_servers("kafka:9092") \
    .set_record_serializer(
        KafkaRecordSerializationSchema.builder()
            .set_topic("fraud-alerts")
            .set_value_serialization_schema(SimpleStringSchema())
            .build()
    ) \
    .set_delivery_guarantee(DeliveryGuarantee.AT_LEAST_ONCE) \
    .build()

fraud_alerts.map(lambda a: json.dumps(a)).sink_to(kafka_sink)

# Execute
env.execute("Order Processing with Fraud Detection")
```

---

## 🔄 Flink SQL

```sql
-- Create Kafka source table
CREATE TABLE orders (
    order_id    BIGINT,
    customer_id BIGINT,
    amount      DOUBLE,
    region      STRING,
    `timestamp` TIMESTAMP(3),
    WATERMARK FOR `timestamp` AS `timestamp` - INTERVAL '30' SECOND
) WITH (
    'connector' = 'kafka',
    'topic' = 'orders',
    'properties.bootstrap.servers' = 'kafka:9092',
    'format' = 'json'
);

-- Create output sink table
CREATE TABLE revenue_by_region (
    window_start TIMESTAMP(3),
    window_end   TIMESTAMP(3),
    region       STRING,
    total_revenue DOUBLE,
    order_count  BIGINT
) WITH (
    'connector' = 'jdbc',
    'url' = 'jdbc:postgresql://db:5432/analytics',
    'table-name' = 'revenue_by_region'
);

-- Streaming window aggregation
INSERT INTO revenue_by_region
SELECT
    window_start,
    window_end,
    region,
    SUM(amount) AS total_revenue,
    COUNT(*) AS order_count
FROM TABLE(
    TUMBLE(TABLE orders, DESCRIPTOR(`timestamp`), INTERVAL '5' MINUTES)
)
GROUP BY window_start, window_end, region;
```

---

*Related: [[02-Core-Concepts/Stream-Processing]] | [[03-Tools/Kafka]] | Back to [[00-Index]]*
