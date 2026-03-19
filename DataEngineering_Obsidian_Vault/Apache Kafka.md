# 📨 Apache Kafka

> **Level:** Intermediate-Advanced | **Stage:** 3 of 5
> Apache Kafka is the industry-standard platform for real-time data streaming — it connects systems at massive scale and near-zero latency.

---

## 🤔 What is Apache Kafka?

Kafka is a **distributed event streaming platform** that lets you publish and subscribe to streams of events in real time.

- **Producers** write events to Kafka
- **Consumers** read events from Kafka
- Kafka stores events **durably** (like a log file, not a queue)

> **Real-world analogy:** Kafka is like a **post office with unlimited storage**. Publishers drop letters (events) in mailboxes (topics). Subscribers pick up their mail whenever they want — and the post office keeps copies of all letters for 7 days.

---

## 🧠 Key Concepts

### 1. Core Architecture
```
[Producer App]  →  [Kafka Broker]  →  [Consumer App]
                      (Topic)
                   Partition 0: [event1][event2][event3]
                   Partition 1: [event4][event5][event6]
                   Partition 2: [event7][event8][event9]
```

### 2. Key Components

| Component | Description |
|-----------|-------------|
| **Topic** | A named stream of events (like a table in a DB) |
| **Partition** | Topics are split into partitions for parallelism |
| **Offset** | Position of a message within a partition (like a line number) |
| **Producer** | App that writes events to a topic |
| **Consumer** | App that reads events from a topic |
| **Consumer Group** | Multiple consumers sharing work on a topic |
| **Broker** | A Kafka server (multiple brokers = a cluster) |
| **ZooKeeper / KRaft** | Manages cluster metadata |

### 3. Producer (Python Example)
```python
from kafka import KafkaProducer
import json

producer = KafkaProducer(
    bootstrap_servers="localhost:9092",
    value_serializer=lambda v: json.dumps(v).encode("utf-8")
)

# Send an event
producer.send("user-clicks", {
    "user_id": 42,
    "event": "button_click",
    "page": "checkout",
    "timestamp": "2024-01-15T10:30:00Z"
})
producer.flush()
```

### 4. Consumer (Python Example)
```python
from kafka import KafkaConsumer
import json

consumer = KafkaConsumer(
    "user-clicks",
    bootstrap_servers="localhost:9092",
    group_id="analytics-service",
    value_deserializer=lambda m: json.loads(m.decode("utf-8")),
    auto_offset_reset="earliest"
)

for message in consumer:
    event = message.value
    print(f"User {event['user_id']} clicked on {event['page']}")
    # Process or store the event
```

### 5. Partitioning & Ordering
```
Topic: "orders" with 3 partitions

Producer sends with key="customer_id":
  customer_123 → always goes to Partition 0 (order guaranteed!)
  customer_456 → always goes to Partition 1
  customer_789 → always goes to Partition 2

Ordering is guaranteed WITHIN a partition, not across partitions.
```

### 6. Retention & Replay ⭐
Kafka stores messages for a configurable period (default: 7 days).
- **Replay:** Reprocess old events by resetting consumer offset
- **This is different from a queue** — messages aren't deleted after reading!

### 7. Kafka Connect
Pre-built connectors to pull/push data without custom code:
```
MySQL → Kafka (Source Connector)
Kafka → Snowflake (Sink Connector)
Kafka → S3 (Sink Connector)
```

---

## 🏭 Real-World Use Cases

| Use Case | How Kafka Helps |
|----------|----------------|
| Real-time fraud detection | Payment events → Kafka → ML model in milliseconds |
| Log aggregation | All app servers → Kafka → Elasticsearch / S3 |
| Event sourcing | Every user action stored as an event |
| Microservices communication | Service A → Kafka → Service B (decoupled) |
| Real-time dashboards | Events → Kafka → Spark Streaming → Dashboard |
| CDC (Change Data Capture) | DB changes → Debezium → Kafka → Warehouse |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Apache Kafka** | Core streaming platform |
| **Confluent Kafka** | Managed Kafka + Schema Registry |
| **AWS MSK** | Managed Kafka on AWS |
| **Kafka Connect** | Pre-built source/sink connectors |
| **Kafka Streams** | Stream processing inside Kafka |
| **Apache Flink** | Advanced stream processing (pairs with Kafka) |
| **Spark Structured Streaming** | Batch-style API for Kafka streams |
| **Debezium** | CDC connector (captures DB changes) |

---

## ✅ Mini Practice Tasks

- [ ] Run Kafka locally using Docker: `docker-compose up kafka zookeeper`
- [ ] Write a producer that sends 100 fake order events to a topic
- [ ] Write a consumer that reads and prints those events
- [ ] Create a topic with 3 partitions and observe which partition events go to
- [ ] Research: What is Debezium and how does CDC work?

---

## 🔗 Related Notes

- [[Big Data Technologies]] — Kafka fits in the Velocity part of Big Data
- [[Apache Spark]] — Spark Structured Streaming reads from Kafka
- [[Data Pipelines]] — Kafka is the backbone of streaming pipelines
- [[Airflow]] — Airflow can trigger jobs based on Kafka events
- [[Cloud for Data Engineering]] — AWS MSK, Confluent Cloud are managed Kafka services

---

## ➡️ Next Step

[[Airflow]] — Learn how to orchestrate and schedule all your pipelines (batch and streaming triggers).
