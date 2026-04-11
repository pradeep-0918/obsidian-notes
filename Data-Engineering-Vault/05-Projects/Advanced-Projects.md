# 🏆 Advanced Projects — Portfolio Guide
#project #advanced #portfolio #architecture

---

## 🎯 Project 1: End-to-End Lakehouse Platform

**What you'll build:** A production-grade lakehouse with CDC ingestion, Delta Lake storage, dbt transformations, and Superset dashboards.

**Architecture:**
```
PostgreSQL (OLTP)
    │
    ▼ CDC (Debezium)
Kafka Topics
    │
    ├──► Flink (Real-time metrics) ──► Redis ──► Live Dashboard
    │
    └──► Spark Structured Streaming ──► Delta Lake (Bronze)
                                              │
                                        dbt models
                                              │
                                        Delta Lake (Silver/Gold)
                                              │
                                        Apache Superset
```

**Estimated time:** 3-4 weeks | **Resume impact:** Very High

```yaml
# docker-compose.yml for full stack
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: ecommerce
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    command: >
      postgres
      -c wal_level=logical
      -c max_replication_slots=10
      -c max_wal_senders=10

  debezium:
    image: debezium/connect:2.4
    environment:
      BOOTSTRAP_SERVERS: kafka:9092
      GROUP_ID: debezium
      CONFIG_STORAGE_TOPIC: debezium_config
      OFFSET_STORAGE_TOPIC: debezium_offsets

  kafka:
    image: confluentinc/cp-kafka:7.5.0

  spark-master:
    image: bitnami/spark:3.5
    environment:
      SPARK_MODE: master

  superset:
    image: apache/superset:3.0.0
    ports:
      - "8088:8088"
```

---

## 🎯 Project 2: ML Feature Store Pipeline

**What you'll build:** A feature store that computes ML features in batch and real-time, serving them via a low-latency API.

```
┌──────────────────────────────────────────────────────────┐
│                    FEATURE STORE                          │
│                                                          │
│  ── BATCH FEATURES (Spark, daily) ──                    │
│  customer_30d_spend, churn_score, rfm_segment           │
│  ──────────────────────────────────────────             │
│  ── REAL-TIME FEATURES (Flink, <1s) ──                  │
│  session_duration, cart_value, recent_clicks            │
│  ──────────────────────────────────────────             │
│  ── SERVING (Redis, <5ms) ──                            │
│  GET /features?customer_id=123&features=all             │
└──────────────────────────────────────────────────────────┘
```

```python
# feature_store.py
import redis
import json
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

class FeatureStore:
    """
    Manages batch and real-time feature computation and serving.
    """
    def __init__(self, redis_host="redis", spark=None):
        self.redis = redis.Redis(host=redis_host, port=6379)
        self.spark = spark or SparkSession.builder.getOrCreate()
    
    def compute_batch_features(self, run_date: str):
        """Compute expensive batch features daily."""
        orders = self.spark.read.format("delta").load("data/delta/orders")
        
        features = orders \
            .filter(F.col("order_date") >= F.date_sub(F.lit(run_date), 30)) \
            .groupBy("customer_id") \
            .agg(
                F.sum("net_revenue").alias("spend_30d"),
                F.count("order_id").alias("orders_30d"),
                F.avg("net_revenue").alias("avg_order_30d"),
                F.max("order_date").alias("last_order_date"),
                F.datediff(F.lit(run_date), F.max("order_date")).alias("days_since_order")
            ) \
            .withColumn("is_churning",
                F.col("days_since_order") > 60
            )
        
        # Write to Redis
        for row in features.collect():
            key = f"features:{row.customer_id}"
            value = {
                "spend_30d": float(row.spend_30d or 0),
                "orders_30d": int(row.orders_30d),
                "avg_order_30d": float(row.avg_order_30d or 0),
                "days_since_order": int(row.days_since_order or 999),
                "is_churning": bool(row.is_churning),
                "computed_at": run_date
            }
            self.redis.setex(key, 86400 * 2, json.dumps(value))  # TTL: 2 days
        
        print(f"Wrote {features.count()} customer features to Redis")
    
    def get_features(self, customer_id: str) -> dict:
        """Serve features with <5ms latency."""
        key = f"features:{customer_id}"
        data = self.redis.get(key)
        if data:
            return json.loads(data)
        return {"customer_id": customer_id, "is_new": True}
    
    def update_realtime_feature(self, customer_id: str, feature: str, value):
        """Update a real-time feature (called from Flink)."""
        key = f"features:rt:{customer_id}"
        self.redis.hset(key, feature, json.dumps(value))
        self.redis.expire(key, 3600)  # TTL: 1 hour
```

---

## 📋 Advanced Project Ideas (Quick List)

| Project | Key Tech | Difficulty | Resume Impact |
|---|---|---|---|
| CDC + Lakehouse | Debezium, Kafka, Delta | ⭐⭐⭐⭐⭐ | Very High |
| Feature Store | Spark, Redis, Flink | ⭐⭐⭐⭐⭐ | Very High |
| Data Mesh PoC | Multiple domains, data catalog | ⭐⭐⭐⭐⭐ | High |
| Cost Monitor | Athena, Lambda, Slack | ⭐⭐⭐ | Medium |
| Data Quality Platform | GE, Airflow, Slack | ⭐⭐⭐⭐ | High |
| Stream-stream join | Kafka, Flink, Delta | ⭐⭐⭐⭐ | High |

---

*Back to [[05-Projects/Intermediate-Projects]] | [[00-Index]]*
