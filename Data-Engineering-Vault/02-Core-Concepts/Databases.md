# 🗄️ Databases — Complete Guide
#concept #databases #sql #nosql

> The foundation of all data engineering. Choose the wrong database and you'll pay for it forever.

---

## 🧠 The Mental Model

Think of databases as different types of filing systems:

| Type | Analogy | Best For |
|------|---------|----------|
| Relational | Spreadsheets with rigid structure | Transactions, structured data |
| Document | Filing cabinet with flexible folders | JSON data, variable schemas |
| Key-Value | Dictionary / phone book | Caching, sessions, lookups |
| Column | Spreadsheet read column-by-column | Analytics, aggregations |
| Graph | A social network map | Relationships, recommendations |
| Time-Series | A stock price chart | Metrics, IoT, monitoring |

---

## 🏛️ Relational Databases

### What Makes Them Relational?

Data is stored in **tables** with defined **schemas**, and tables are linked via **foreign keys**.

The relational model provides **ACID guarantees:**
- **A**tomicity — All or nothing (transaction either completes or rolls back)
- **C**onsistency — Data always moves from valid state to valid state
- **I**solation — Concurrent transactions don't interfere with each other
- **D**urability — Committed data survives crashes

### PostgreSQL — The Gold Standard

```sql
-- Advanced PostgreSQL features you should know

-- 1. JSONB — flexible columns in a rigid table
CREATE TABLE products (
    id          SERIAL PRIMARY KEY,
    name        VARCHAR(255) NOT NULL,
    attributes  JSONB DEFAULT '{}'
);

INSERT INTO products (name, attributes) VALUES
    ('Laptop', '{"ram": "16GB", "cpu": "M2", "display": "13inch"}'),
    ('Phone', '{"storage": "256GB", "color": "black"}');

-- Query JSONB
SELECT name, attributes->>'ram' AS ram
FROM products
WHERE attributes->>'cpu' = 'M2';

-- Index JSONB
CREATE INDEX idx_products_attributes ON products USING GIN (attributes);

-- 2. Window Functions
SELECT
    customer_id,
    order_date,
    amount,
    SUM(amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS running_total,
    LAG(amount) OVER (PARTITION BY customer_id ORDER BY order_date) AS prev_order,
    RANK() OVER (PARTITION BY DATE_TRUNC('month', order_date) ORDER BY amount DESC) AS monthly_rank
FROM orders;

-- 3. CTEs for readable complex queries
WITH
customer_stats AS (
    SELECT
        customer_id,
        COUNT(*) AS order_count,
        SUM(amount) AS lifetime_value,
        AVG(amount) AS avg_order
    FROM orders
    GROUP BY customer_id
),
segments AS (
    SELECT
        customer_id,
        CASE
            WHEN lifetime_value > 10000 THEN 'VIP'
            WHEN lifetime_value > 1000  THEN 'Regular'
            ELSE 'New'
        END AS segment
    FROM customer_stats
)
SELECT c.name, c.email, s.segment, cs.lifetime_value
FROM customers c
JOIN customer_stats cs USING (customer_id)
JOIN segments s USING (customer_id)
ORDER BY cs.lifetime_value DESC;

-- 4. Partitioning for large tables
CREATE TABLE events (
    event_id    BIGSERIAL,
    event_date  DATE NOT NULL,
    event_type  VARCHAR(50),
    payload     JSONB
) PARTITION BY RANGE (event_date);

CREATE TABLE events_2024_01 PARTITION OF events
    FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');
CREATE TABLE events_2024_02 PARTITION OF events
    FOR VALUES FROM ('2024-02-01') TO ('2024-03-01');
```

### Query Optimization

```sql
-- EXPLAIN ANALYZE is your best friend
EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON)
SELECT c.name, SUM(o.amount)
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.created_at > '2024-01-01'
GROUP BY c.name;

-- Read the output:
-- Seq Scan = bad for large tables (needs index)
-- Index Scan = good
-- Bitmap Heap Scan = good for range queries
-- Hash Join = good for large tables
-- Nested Loop = good when one table is small
-- Sort + Limit = can be replaced by index

-- Create the right index
CREATE INDEX CONCURRENTLY idx_orders_created_customer
    ON orders (created_at DESC, customer_id)
    INCLUDE (amount);  -- Covering index - includes all needed columns
```

---

## 📦 NoSQL Databases

### When to NOT Use Relational Databases

- Schema changes constantly (product catalog with varying attributes)
- Horizontal scaling needed beyond a single machine
- Extremely high write throughput (millions/sec)
- Data is naturally hierarchical or graph-shaped

### MongoDB — Document Database

```python
from pymongo import MongoClient

client = MongoClient("mongodb://localhost:27017/")
db = client["ecommerce"]
collection = db["products"]

# Insert — flexible schema, no migration needed
product = {
    "sku": "LAPTOP-001",
    "name": "MacBook Pro 14",
    "price": 1999.99,
    "specs": {
        "ram": "16GB",
        "storage": "512GB SSD",
        "cpu": "Apple M3 Pro"
    },
    "tags": ["laptop", "apple", "professional"],
    "variants": [
        {"color": "Space Gray", "stock": 15},
        {"color": "Silver", "stock": 8}
    ]
}
result = collection.insert_one(product)

# Query with rich operators
laptops = collection.find({
    "price": {"$gte": 1000, "$lte": 3000},
    "tags": "laptop",
    "specs.ram": {"$in": ["16GB", "32GB"]},
    "variants": {
        "$elemMatch": {"stock": {"$gt": 0}}
    }
})

# Aggregation pipeline
pipeline = [
    {"$match": {"tags": "laptop"}},
    {"$group": {
        "_id": "$specs.ram",
        "avg_price": {"$avg": "$price"},
        "count": {"$sum": 1}
    }},
    {"$sort": {"avg_price": -1}},
    {"$limit": 10}
]
results = list(collection.aggregate(pipeline))
```

**MongoDB indexes:**
```javascript
// Compound index for common query pattern
db.products.createIndex({"tags": 1, "price": 1})

// Text search index
db.products.createIndex({"name": "text", "description": "text"})

// TTL index for auto-expiring documents
db.sessions.createIndex({"createdAt": 1}, {expireAfterSeconds: 3600})
```

---

### Redis — Key-Value Cache & More

```python
import redis
import json

r = redis.Redis(host='localhost', port=6379, decode_responses=True)

# Simple key-value
r.set("user:123:name", "Alice", ex=3600)  # Expires in 1 hour
name = r.get("user:123:name")

# Hash (object-like)
r.hset("user:123", mapping={
    "name": "Alice",
    "email": "alice@example.com",
    "score": 1500
})
user = r.hgetall("user:123")

# Sorted Set (leaderboard)
r.zadd("leaderboard", {"alice": 1500, "bob": 1200, "carol": 1800})
top3 = r.zrevrange("leaderboard", 0, 2, withscores=True)

# List (queue / recent activity)
r.lpush("recent_orders:user:123", json.dumps({"order_id": "ORD-456", "amount": 99.99}))
recent = r.lrange("recent_orders:user:123", 0, 9)  # Last 10 orders

# Pub/Sub
pubsub = r.pubsub()
pubsub.subscribe("order_updates")
for message in pubsub.listen():
    if message["type"] == "message":
        handle_order_update(json.loads(message["data"]))

# Rate limiting pattern
def is_rate_limited(user_id: str, limit: int = 100, window: int = 60) -> bool:
    key = f"rate_limit:{user_id}"
    count = r.incr(key)
    if count == 1:
        r.expire(key, window)
    return count > limit
```

---

### Cassandra — Distributed Column Store

**The fundamental rule of Cassandra design:**
> **Design your tables based on your queries, NOT your data.**

```sql
-- Model for: "Get all orders for customer X ordered by date"
CREATE TABLE orders_by_customer (
    customer_id  UUID,
    order_date   TIMESTAMP,
    order_id     UUID,
    status       TEXT,
    total        DECIMAL,
    PRIMARY KEY  (customer_id, order_date, order_id)
) WITH CLUSTERING ORDER BY (order_date DESC);

-- Model for: "Get all orders by status for today" (different query = different table)
CREATE TABLE orders_by_status_date (
    status      TEXT,
    order_date  DATE,
    order_id    UUID,
    customer_id UUID,
    total       DECIMAL,
    PRIMARY KEY ((status, order_date), order_id)
);
```

**Cassandra data model rules:**
1. Partition key = how data is distributed across nodes
2. Clustering columns = how data is sorted within a partition
3. No JOINs — denormalize everything
4. No ALLOW FILTERING in production
5. No unbounded partitions (watch partition size)

---

## ⏱️ Time-Series Databases

### InfluxDB

```python
from influxdb_client import InfluxDBClient, Point
from influxdb_client.client.write_api import SYNCHRONOUS

client = InfluxDBClient(url="http://localhost:8086", token="my-token", org="my-org")
write_api = client.write_api(write_options=SYNCHRONOUS)

# Write metrics
point = Point("system_metrics") \
    .tag("host", "server-01") \
    .tag("region", "us-east") \
    .field("cpu_usage", 45.2) \
    .field("memory_mb", 8192) \
    .field("disk_io_mbps", 120.5)

write_api.write(bucket="monitoring", record=point)

# Query with Flux language
query_api = client.query_api()
query = """
from(bucket: "monitoring")
    |> range(start: -1h)
    |> filter(fn: (r) => r._measurement == "system_metrics")
    |> filter(fn: (r) => r.host == "server-01")
    |> aggregateWindow(every: 5m, fn: mean, createEmpty: false)
    |> yield(name: "mean")
"""
result = query_api.query(query)
```

---

## 📊 ClickHouse — OLAP Powerhouse

```sql
-- ClickHouse is extremely fast for analytics
-- Example: Billion row table, sub-second queries

CREATE TABLE events (
    event_date    Date,
    event_time    DateTime,
    user_id       UInt64,
    event_type    LowCardinality(String),  -- Optimized for repeated values
    properties    String  -- JSON as string
)
ENGINE = MergeTree()
PARTITION BY toYYYYMM(event_date)
ORDER BY (event_date, user_id, event_time)
TTL event_date + INTERVAL 1 YEAR;  -- Auto-delete old data

-- Count events per type per day — lightning fast
SELECT
    event_date,
    event_type,
    COUNT() AS event_count,
    uniqCombined(user_id) AS unique_users  -- Approximate distinct count
FROM events
WHERE event_date BETWEEN '2024-01-01' AND '2024-01-31'
GROUP BY event_date, event_type
ORDER BY event_date, event_count DESC;
```

---

## 🔄 Database Comparison Matrix

| Database | Type | Consistency | Scale | Best Use Case | Avoid When |
|---|---|---|---|---|---|
| PostgreSQL | Relational | Strong ACID | Vertical | Transactions, OLTP | Need petabyte analytics |
| MySQL | Relational | Strong ACID | Vertical | Web apps, WordPress | Complex analytics |
| MongoDB | Document | Tunable | Horizontal | Flexible schemas, catalogs | Need ACID transactions |
| Redis | Key-Value | Eventual | Horizontal | Caching, sessions | Need persistence |
| Cassandra | Column | Eventual | Horizontal | High write, IoT, logs | Need ad-hoc queries |
| ClickHouse | Column | Strong | Horizontal | Real-time analytics | Frequent updates |
| Neo4j | Graph | Strong | Vertical | Social networks, recommendations | Non-relational data |
| InfluxDB | Time-series | Strong | Horizontal | Metrics, monitoring | General purpose |
| Elasticsearch | Document+Search | Eventual | Horizontal | Full-text search, logs | Primary data store |

---

## 🔗 Related Notes

- [[02-Core-Concepts/Data-Ingestion]] — How to get data into these databases
- [[04-System-Design/Data-Warehouse]] — Using databases for analytics
- [[03-Tools/Kafka]] — Streaming data into databases
- [[09-Cheat-Sheets]] — SQL cheat sheet

---

*Back to [[00-Index]]*
