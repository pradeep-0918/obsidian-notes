# 🚀 Apache Kafka — Complete Guide
#tool #kafka #streaming #messaging

> The nervous system of modern data platforms. Every large-scale data team runs Kafka.

---

## 🧠 What is Kafka? (Feynman Explanation)

Imagine a newspaper publisher:
- **Publishers (Producers)** write articles and submit them
- **The printing press (Kafka Broker)** stores articles durably, organized by section
- **Readers (Consumers)** can subscribe to specific sections and read at their own pace
- **Key difference from traditional queues**: articles aren't discarded after reading — anyone can re-read them

Traditional message queue:
```
Producer → [Queue] → Consumer (message deleted after read)
```

Kafka:
```
Producer → [Kafka Topic (persisted log)] → Consumer A (reads at its own pace)
                                         → Consumer B (independent, can re-read)
                                         → Consumer C (started reading 3 days ago)
```

---

## 🏗️ Core Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                         KAFKA CLUSTER                             │
│                                                                  │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐                      │
│  │ Broker 1 │    │ Broker 2 │    │ Broker 3 │   ← 3+ for HA     │
│  │          │    │          │    │          │                    │
│  │ Topic A  │    │ Topic A  │    │ Topic A  │                    │
│  │ Part 0 ← Leader        Part 1 ← Leader                      │
│  │ Part 1 ← Replica       Part 0 ← Replica                     │
│  └─────────┘    └─────────┘    └─────────┘                      │
│                                                                  │
│  ZooKeeper (or KRaft) — Coordinates cluster metadata           │
└──────────────────────────────────────────────────────────────────┘
```

### Core Concepts

| Concept | Definition | Analogy |
|---|---|---|
| Topic | Named stream of records | A newspaper section |
| Partition | Ordered, immutable log | A printing machine |
| Offset | Position within a partition | Page number |
| Consumer Group | Set of consumers sharing partitions | A subscription plan |
| Producer | Writes records to topics | Reporter |
| Broker | Kafka server | Printing facility |
| Replication Factor | How many copies of each partition | Distribution centers |
| Retention | How long to keep messages | Archive policy |

---

## ⚙️ Setup & Configuration

### Docker Compose for Development

```yaml
version: '3.8'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:7.5.0
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181

  kafka:
    image: confluentinc/cp-kafka:7.5.0
    depends_on: [zookeeper]
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
      KAFKA_AUTO_CREATE_TOPICS_ENABLE: "false"
      KAFKA_LOG_RETENTION_HOURS: 168        # 7 days
      KAFKA_LOG_SEGMENT_BYTES: 1073741824   # 1GB segments
      KAFKA_NUM_PARTITIONS: 3

  schema-registry:
    image: confluentinc/cp-schema-registry:7.5.0
    depends_on: [kafka]
    ports:
      - "8081:8081"
    environment:
      SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS: kafka:9092
      SCHEMA_REGISTRY_HOST_NAME: schema-registry

  kafka-ui:
    image: provectuslabs/kafka-ui:latest
    depends_on: [kafka]
    ports:
      - "8080:8080"
    environment:
      KAFKA_CLUSTERS_0_NAME: local
      KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS: kafka:9092
```

### Topic Configuration

```bash
# Create topic with custom settings
kafka-topics.sh --create \
  --bootstrap-server localhost:9092 \
  --topic orders \
  --partitions 12 \
  --replication-factor 3 \
  --config retention.ms=604800000 \    # 7 days
  --config compression.type=snappy \
  --config min.insync.replicas=2 \
  --config cleanup.policy=delete

# List topics
kafka-topics.sh --list --bootstrap-server localhost:9092

# Describe topic
kafka-topics.sh --describe --topic orders --bootstrap-server localhost:9092

# Change retention (for existing topic)
kafka-configs.sh --alter \
  --bootstrap-server localhost:9092 \
  --entity-type topics \
  --entity-name orders \
  --add-config retention.ms=2592000000  # 30 days
```

---

## 🐍 Python Producer

```python
from confluent_kafka import Producer
from confluent_kafka.schema_registry import SchemaRegistryClient
from confluent_kafka.schema_registry.avro import AvroSerializer
import json
import time
import logging

logger = logging.getLogger(__name__)

class OrderProducer:
    def __init__(self, bootstrap_servers: str, topic: str):
        self.topic = topic
        self.producer = Producer({
            'bootstrap.servers': bootstrap_servers,
            # Reliability settings
            'acks': 'all',                    # Wait for all replicas
            'enable.idempotence': True,        # Exactly-once semantics
            'max.in.flight.requests.per.connection': 5,
            # Performance settings
            'batch.size': 1048576,             # 1MB batches
            'linger.ms': 10,                   # Wait 10ms to batch
            'compression.type': 'snappy',
            # Retry settings
            'retries': 10,
            'retry.backoff.ms': 100,
        })
        self.errors = []
    
    def delivery_callback(self, err, msg):
        if err:
            logger.error(f"Delivery failed for key {msg.key()}: {err}")
            self.errors.append(err)
        else:
            logger.debug(
                f"Delivered message to {msg.topic()}[{msg.partition()}]"
                f" at offset {msg.offset()}"
            )
    
    def send_order(self, order: dict):
        """Send a single order event."""
        self.producer.produce(
            topic=self.topic,
            key=str(order['order_id']),           # Ensures order ordering per customer
            value=json.dumps(order),
            headers={'source': 'order-service', 'version': '2.0'},
            callback=self.delivery_callback
        )
        # Poll for callbacks (non-blocking)
        self.producer.poll(0)
    
    def send_batch(self, orders: list[dict]):
        """Send a batch of orders."""
        for order in orders:
            self.send_order(order)
        
        # Wait for all to be delivered
        remaining = self.producer.flush(timeout=30)
        if remaining > 0:
            raise RuntimeError(f"{remaining} messages were not delivered!")
        
        if self.errors:
            raise RuntimeError(f"Delivery errors: {self.errors}")
        
        logger.info(f"Successfully sent {len(orders)} orders")
    
    def __del__(self):
        self.producer.flush()

# Usage
producer = OrderProducer("localhost:9092", "orders")
producer.send_batch([
    {"order_id": 1001, "customer_id": 42, "amount": 99.99, "status": "created"},
    {"order_id": 1002, "customer_id": 43, "amount": 149.99, "status": "created"},
])
```

---

## 🐍 Python Consumer

```python
from confluent_kafka import Consumer, KafkaError, TopicPartition
import json
import signal
import logging

logger = logging.getLogger(__name__)

class OrderConsumer:
    def __init__(self, bootstrap_servers: str, group_id: str, topics: list):
        self.running = True
        self.consumer = Consumer({
            'bootstrap.servers': bootstrap_servers,
            'group.id': group_id,
            'auto.offset.reset': 'earliest',
            # NEVER enable auto-commit in production
            'enable.auto.commit': False,
            # Session management
            'session.timeout.ms': 30000,
            'heartbeat.interval.ms': 10000,
            'max.poll.interval.ms': 300000,
            # Performance
            'fetch.min.bytes': 1024,
            'fetch.max.wait.ms': 500,
            'max.partition.fetch.bytes': 1048576,
        })
        self.consumer.subscribe(topics, on_assign=self.on_assign)
        
        # Graceful shutdown
        signal.signal(signal.SIGTERM, self._shutdown)
        signal.signal(signal.SIGINT, self._shutdown)
    
    def on_assign(self, consumer, partitions):
        logger.info(f"Assigned partitions: {partitions}")
    
    def _shutdown(self, signum, frame):
        logger.info("Shutting down consumer...")
        self.running = False
    
    def process_message(self, order: dict):
        """Override this method in subclasses."""
        raise NotImplementedError
    
    def run(self, batch_size: int = 100):
        """Main consumption loop with batch processing."""
        batch = []
        
        try:
            while self.running:
                # Poll with timeout
                msg = self.consumer.poll(timeout=1.0)
                
                if msg is None:
                    # No message, process any accumulated batch
                    if batch:
                        self._process_batch(batch)
                        batch = []
                    continue
                
                if msg.error():
                    if msg.error().code() == KafkaError.PARTITION_EOF:
                        logger.info(f"Reached end of partition {msg.partition()}")
                    else:
                        raise KafkaError(msg.error())
                    continue
                
                # Deserialize
                order = json.loads(msg.value().decode('utf-8'))
                batch.append((msg, order))
                
                # Process batch when full
                if len(batch) >= batch_size:
                    self._process_batch(batch)
                    batch = []
        
        finally:
            if batch:
                self._process_batch(batch)
            self.consumer.close()
            logger.info("Consumer closed")
    
    def _process_batch(self, batch: list):
        """Process a batch and commit offsets."""
        messages, orders = zip(*batch)
        
        try:
            # Process all orders in batch
            for order in orders:
                self.process_message(order)
            
            # Commit only after successful processing
            self.consumer.commit()
            logger.info(f"Committed batch of {len(batch)} messages")
            
        except Exception as e:
            logger.error(f"Error processing batch: {e}")
            # Route to DLQ
            self._send_to_dlq(orders, str(e))
            # Still commit to avoid infinite retry loops
            # (or don't commit and implement retry logic)
            raise


class WarehouseOrderConsumer(OrderConsumer):
    def __init__(self, *args, db_connection, **kwargs):
        super().__init__(*args, **kwargs)
        self.db = db_connection
    
    def process_message(self, order: dict):
        self.db.upsert_order(order)
```

---

## 🔄 Consumer Groups & Partitions

```
Topic: "orders" (6 partitions)

Consumer Group A (3 consumers):
┌──────────────────────────────────────────────┐
│ Consumer A1 → Partition 0, Partition 1       │
│ Consumer A2 → Partition 2, Partition 3       │
│ Consumer A3 → Partition 4, Partition 5       │
└──────────────────────────────────────────────┘

Consumer Group B (6 consumers — maximum parallelism):
┌──────────────────────────────────────────────┐
│ Consumer B1 → Partition 0                    │
│ Consumer B2 → Partition 1                    │
│ ...                                          │
│ Consumer B6 → Partition 5                    │
└──────────────────────────────────────────────┘

Consumer Group C (8 consumers — 2 idle):
┌──────────────────────────────────────────────┐
│ Consumer C1 → Partition 0                    │
│ ...                                          │
│ Consumer C6 → Partition 5                    │
│ Consumer C7 → IDLE (no partition)            │
│ Consumer C8 → IDLE (no partition)            │
└──────────────────────────────────────────────┘

Key rule: Max parallelism = number of partitions
```

---

## 📊 Kafka Monitoring

Key metrics to watch:

```python
# Consumer lag (most important!)
# Lag = Latest offset - Consumer committed offset
# High lag = Consumer can't keep up with producers

# Check lag from command line:
# kafka-consumer-groups.sh --describe --group my-group --bootstrap-server localhost:9092

# Metrics to monitor:
CRITICAL_METRICS = {
    "consumer.lag": "< 1000 messages per partition",
    "broker.disk.usage": "< 75%",
    "under.replicated.partitions": "== 0 (any > 0 = danger)",
    "active.controller.count": "== 1 (must be exactly 1)",
    "network.io.bytes": "< 80% of bandwidth",
    "request.error.rate": "< 0.1%"
}
```

---

## 🔧 Kafka Best Practices

| Practice | Recommendation |
|---|---|
| Partitions | Start with 3-12 per topic; scale based on throughput |
| Replication | Production: 3 (min.insync.replicas=2) |
| Message size | <1MB (default); tune if needed |
| Retention | 7 days default; longer for replay use cases |
| Keys | Use business keys (customer_id, order_id) for ordering |
| Serialization | Use Avro + Schema Registry in production |
| Consumers | Disable auto-commit; commit after processing |
| Monitoring | Alert on consumer lag immediately |

---

*Related: [[02-Core-Concepts/Stream-Processing]] | [[02-Core-Concepts/Data-Ingestion]] | [[03-Tools/Flink]] | Back to [[00-Index]]*
