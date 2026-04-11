# 🚀 Intermediate Data Engineering Roadmap
#intermediate #roadmap #career

> **Goal:** Build production-grade pipelines, master distributed systems, and become a reliable team member.
> **Prerequisites:** [[01-Roadmap/Beginner]] completed

---

## 🎯 What "Intermediate" Means

At this stage you should be able to:
- Design and implement a multi-step data pipeline
- Choose the right tool for a given problem
- Debug pipeline failures independently
- Write readable, maintainable data code
- Collaborate with data scientists and analysts

**The gap most intermediates struggle with:**
> Going from "it works on my laptop" to "it runs reliably in production at scale."

---

## 📅 Month 4–5: Distributed Processing with Spark

### Why Spark?

When your data outgrows a single machine (typically >10GB), you need distributed processing. Spark is the industry standard.

**The mental model:**
```
Single Machine (pandas):          Spark Cluster:
┌──────────────────┐              ┌──────┐  ┌──────┐  ┌──────┐
│   Your Laptop    │              │Node 1│  │Node 2│  │Node 3│
│  RAM: 16GB       │    →→→       │Worker│  │Worker│  │Worker│
│  CPU: 8 cores    │              └──────┘  └──────┘  └──────┘
└──────────────────┘                    ↑
                                   Driver Node
                                 (coordinates all)
```

**Core Spark concepts:**

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.types import *

# Create session (entry point to everything)
spark = SparkSession.builder \
    .appName("SalesAnalysis") \
    .config("spark.sql.shuffle.partitions", "200") \
    .getOrCreate()

# Read data (lazy - nothing happens yet)
df = spark.read.parquet("s3://my-bucket/sales/year=2024/")

# Transform (still lazy)
result = df \
    .filter(F.col("status") == "completed") \
    .withColumn("revenue", F.col("quantity") * F.col("price")) \
    .groupBy("region", F.date_trunc("month", F.col("order_date")).alias("month")) \
    .agg(
        F.sum("revenue").alias("total_revenue"),
        F.count("order_id").alias("order_count"),
        F.avg("revenue").alias("avg_order_value")
    ) \
    .orderBy("region", "month")

# Action (this triggers computation)
result.write \
    .mode("overwrite") \
    .partitionBy("region") \
    .parquet("s3://my-bucket/analytics/monthly_sales/")
```

**Spark optimization tricks:**
```python
# 1. Broadcast joins for small tables (avoids shuffle)
from pyspark.sql.functions import broadcast
result = large_df.join(broadcast(small_lookup_df), on="product_id")

# 2. Cache frequently reused DataFrames
df.cache()
df.count()  # Materialize cache

# 3. Partition pruning - always filter on partition columns first
df_filtered = df.filter(F.col("year") == 2024).filter(F.col("region") == "US")

# 4. Use appropriate data types
df = df.withColumn("id", F.col("id").cast(LongType()))

# 5. Avoid UDFs when possible (they break optimization)
# BAD:
from pyspark.sql.functions import udf
compute_udf = udf(lambda x: x * 2, DoubleType())
df.withColumn("doubled", compute_udf(F.col("value")))

# GOOD: Use built-in functions
df.withColumn("doubled", F.col("value") * 2)
```

See [[03-Tools/Spark]] for full Spark deep dive.

---

## 📅 Month 5–6: Apache Kafka — Event Streaming

### The Mental Model

Kafka is a **distributed commit log**. Think of it as:
- A very fast, reliable, ordered queue
- Every event is stored durably on disk
- Multiple consumers can read the same event stream independently
- Events can be replayed from any point in time

```
           ┌─────────────────────────────────────────────┐
           │                  KAFKA CLUSTER               │
           │                                             │
Producers  │  Topic: "orders"                            │  Consumers
──────────►│  Partition 0: [msg1][msg2][msg3][msg4]──►  │──────────► Inventory Service
──────────►│  Partition 1: [msg1][msg2][msg3][msg4]──►  │──────────► Analytics Pipeline
──────────►│  Partition 2: [msg1][msg2][msg3][msg4]──►  │──────────► Email Service
           │                                             │
           └─────────────────────────────────────────────┘
```

**Python Kafka producer:**
```python
from confluent_kafka import Producer
import json

producer = Producer({'bootstrap.servers': 'localhost:9092'})

def delivery_report(err, msg):
    if err:
        print(f'Message delivery failed: {err}')

def produce_order(order: dict):
    producer.produce(
        topic='orders',
        key=str(order['order_id']).encode('utf-8'),
        value=json.dumps(order).encode('utf-8'),
        callback=delivery_report
    )
    producer.poll(0)

# Produce events
for order in get_new_orders():
    produce_order(order)

producer.flush()
```

**Python Kafka consumer:**
```python
from confluent_kafka import Consumer

consumer = Consumer({
    'bootstrap.servers': 'localhost:9092',
    'group.id': 'analytics-pipeline',
    'auto.offset.reset': 'earliest',
    'enable.auto.commit': False,  # Manual commits for reliability
})

consumer.subscribe(['orders'])

try:
    while True:
        msg = consumer.poll(1.0)
        if msg is None:
            continue
        if msg.error():
            handle_error(msg.error())
            continue
        
        order = json.loads(msg.value().decode('utf-8'))
        process_order(order)
        consumer.commit()  # Commit only after successful processing
        
finally:
    consumer.close()
```

See [[03-Tools/Kafka]] for complete Kafka guide.

---

## 📅 Month 6–7: Data Modeling & Warehousing

### Dimensional Modeling (Star Schema)

The most important pattern for analytics databases:

```
                    ┌──────────────────┐
                    │   dim_date       │
                    │──────────────────│
                    │ date_key (PK)    │
                    │ date             │
                    │ year, quarter    │
                    │ month, day       │
                    │ is_weekend       │
                    └────────┬─────────┘
                             │
┌──────────────┐    ┌────────┴─────────┐    ┌──────────────┐
│ dim_product  │    │   fact_sales     │    │ dim_customer  │
│──────────────│    │─────────────────-│    │──────────────│
│ product_key  ├────┤ product_key (FK) │    │ customer_key │
│ product_name │    │ customer_key(FK) ├────┤ customer_name│
│ category     │    │ date_key (FK)    │    │ segment      │
│ brand        │    │ store_key (FK)   │    │ region       │
│ price_tier   │    │ quantity         │    │ lifetime_val │
└──────────────┘    │ unit_price       │    └──────────────┘
                    │ discount         │
                    │ revenue          │
                    └─────────────────-┘
```

**Slowly Changing Dimensions (SCD Type 2):**
```sql
-- Track historical changes to customer data
CREATE TABLE dim_customer (
    customer_key    BIGSERIAL PRIMARY KEY,     -- Surrogate key
    customer_id     BIGINT NOT NULL,           -- Natural/business key
    email           VARCHAR(255),
    segment         VARCHAR(50),
    region          VARCHAR(50),
    -- SCD Type 2 columns
    effective_from  DATE NOT NULL,
    effective_to    DATE,                      -- NULL = current record
    is_current      BOOLEAN DEFAULT TRUE,
    -- Hash for change detection
    row_hash        VARCHAR(64)
);

-- Update procedure for SCD2
CREATE OR REPLACE PROCEDURE upsert_customer_scd2(
    p_customer_id BIGINT,
    p_email VARCHAR,
    p_segment VARCHAR,
    p_region VARCHAR
)
LANGUAGE plpgsql AS $$
DECLARE
    v_new_hash VARCHAR(64);
    v_old_hash VARCHAR(64);
BEGIN
    v_new_hash := MD5(p_email || p_segment || p_region);
    
    SELECT row_hash INTO v_old_hash
    FROM dim_customer
    WHERE customer_id = p_customer_id AND is_current = TRUE;
    
    IF v_old_hash IS NULL THEN
        -- New customer
        INSERT INTO dim_customer (customer_id, email, segment, region, effective_from, row_hash)
        VALUES (p_customer_id, p_email, p_segment, p_region, CURRENT_DATE, v_new_hash);
        
    ELSIF v_old_hash != v_new_hash THEN
        -- Changed customer - expire old record
        UPDATE dim_customer
        SET effective_to = CURRENT_DATE - 1, is_current = FALSE
        WHERE customer_id = p_customer_id AND is_current = TRUE;
        
        -- Insert new current record
        INSERT INTO dim_customer (customer_id, email, segment, region, effective_from, row_hash)
        VALUES (p_customer_id, p_email, p_segment, p_region, CURRENT_DATE, v_new_hash);
    END IF;
END;
$$;
```

---

## 📅 Month 7–8: Advanced Airflow Patterns

### Production Airflow Patterns

```python
from airflow import DAG
from airflow.operators.python import PythonOperator, BranchPythonOperator
from airflow.sensors.external_task import ExternalTaskSensor
from airflow.utils.trigger_rule import TriggerRule
from datetime import datetime, timedelta

# Pattern 1: Dynamic task generation
def create_region_dag():
    regions = ['US', 'EU', 'APAC']
    
    with DAG('regional_pipeline', ...) as dag:
        start = DummyOperator(task_id='start')
        end = DummyOperator(task_id='end', trigger_rule=TriggerRule.ALL_DONE)
        
        for region in regions:
            process = PythonOperator(
                task_id=f'process_{region.lower()}',
                python_callable=process_region,
                op_kwargs={'region': region},
            )
            start >> process >> end

# Pattern 2: Sensor for data arrival
from airflow.sensors.s3_key_sensor import S3KeySensor

wait_for_data = S3KeySensor(
    task_id='wait_for_upstream_data',
    bucket_name='my-bucket',
    bucket_key='data/{{ ds }}/upstream_complete.flag',
    poke_interval=300,  # Check every 5 minutes
    timeout=7200,       # Timeout after 2 hours
)

# Pattern 3: Branching logic
def check_data_quality(**context):
    record_count = get_record_count(context['ds'])
    if record_count < 1000:
        return 'alert_low_volume'
    return 'proceed_to_transform'

branch = BranchPythonOperator(
    task_id='quality_check',
    python_callable=check_data_quality,
)
```

---

## 📅 Month 8: dbt — Transformations as Code

dbt (data build tool) lets you write SQL transformations with software engineering best practices.

**Why dbt is a game-changer:**
```
Traditional approach:              dbt approach:
- Manual SQL scripts               - Version controlled models
- No testing                       - Built-in tests
- No documentation                 - Auto-generated docs
- No lineage tracking              - Full DAG visualization
- Hard to debug                    - Easy to debug with ref()
```

**dbt model example:**
```sql
-- models/marts/finance/fct_orders.sql
{{ config(
    materialized='incremental',
    unique_key='order_id',
    partition_by={'field': 'order_date', 'data_type': 'date'},
    cluster_by=['customer_id', 'status']
) }}

WITH orders AS (
    SELECT * FROM {{ ref('stg_orders') }}
    {% if is_incremental() %}
    WHERE order_date > (SELECT MAX(order_date) FROM {{ this }})
    {% endif %}
),

customers AS (
    SELECT * FROM {{ ref('dim_customers') }}
),

order_items AS (
    SELECT
        order_id,
        SUM(quantity * unit_price) AS subtotal,
        SUM(discount_amount) AS total_discount,
        COUNT(DISTINCT product_id) AS unique_products
    FROM {{ ref('stg_order_items') }}
    GROUP BY order_id
)

SELECT
    o.order_id,
    o.customer_id,
    c.segment AS customer_segment,
    o.order_date,
    o.status,
    oi.subtotal,
    oi.total_discount,
    oi.subtotal - oi.total_discount AS revenue,
    oi.unique_products
FROM orders o
LEFT JOIN customers c USING (customer_id)
LEFT JOIN order_items oi USING (order_id)
```

**dbt schema tests:**
```yaml
# models/marts/finance/schema.yml
models:
  - name: fct_orders
    description: "One row per order with financial metrics"
    columns:
      - name: order_id
        tests:
          - not_null
          - unique
      - name: status
        tests:
          - accepted_values:
              values: ['pending', 'shipped', 'delivered', 'cancelled']
      - name: revenue
        tests:
          - not_null
          - dbt_utils.expression_is_true:
              expression: ">= 0"
```

---

## ✅ Intermediate Checklist

- [ ] Can write and optimize PySpark jobs
- [ ] Understand Kafka producers, consumers, and consumer groups
- [ ] Can design a star schema for a business problem
- [ ] Implement SCD Type 2 in a data warehouse
- [ ] Write production-ready Airflow DAGs with error handling
- [ ] Use dbt for SQL transformations
- [ ] Have 2-3 portfolio projects demonstrating these skills
- [ ] Comfortable debugging distributed system failures

---

## 🛠️ Tools to Master at This Level

| Category | Tool | Priority |
|----------|------|----------|
| Processing | Apache Spark | ⭐⭐⭐ Critical |
| Streaming | Apache Kafka | ⭐⭐⭐ Critical |
| Orchestration | Apache Airflow | ⭐⭐⭐ Critical |
| Transformation | dbt | ⭐⭐⭐ Critical |
| Cloud | AWS (S3, Glue, Redshift) | ⭐⭐⭐ Critical |
| Streaming | Apache Flink | ⭐⭐ Important |
| Warehouse | Snowflake | ⭐⭐ Important |
| Containerization | Docker | ⭐⭐ Important |

---

*Next: [[01-Roadmap/Advanced]] | Previous: [[01-Roadmap/Beginner]] | Back to [[00-Index]]*
