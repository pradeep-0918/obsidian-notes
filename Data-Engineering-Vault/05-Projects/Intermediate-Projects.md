# 🚀 Intermediate Projects — Portfolio Guide
#project #intermediate #portfolio #spark #kafka

> These projects require orchestration, distributed processing, and cloud infrastructure.

---

## 🎯 Project 1: Real-Time Stock Market Pipeline

**What you'll build:** Ingest live stock data via WebSocket, process with Spark Streaming, visualize in Superset.

**Architecture:**
```
Yahoo Finance WebSocket → Kafka → Spark Streaming → Delta Lake → Superset
```

**Skills demonstrated:** Kafka, Spark Streaming, Delta Lake, real-time analytics

```python
# stock_producer.py - Produce to Kafka
import websocket
import json
from confluent_kafka import Producer

producer = Producer({'bootstrap.servers': 'localhost:9092'})

def on_message(ws, message):
    data = json.loads(message)
    for stock in data:
        producer.produce(
            topic='stock_prices',
            key=stock['id'],
            value=json.dumps({
                'symbol': stock['id'],
                'price': stock['p'],
                'volume': stock.get('v', 0),
                'timestamp': stock['t']
            })
        )
    producer.poll(0)

def on_open(ws):
    ws.send(json.dumps({"type": "subscribe", "symbol": "AAPL"}))
    ws.send(json.dumps({"type": "subscribe", "symbol": "GOOGL"}))

ws = websocket.WebSocketApp(
    f"wss://ws.finnhub.io?token={FINNHUB_API_KEY}",
    on_message=on_message,
    on_open=on_open
)
ws.run_forever()
```

```python
# spark_streaming.py - Process with Spark
from pyspark.sql import SparkSession
from pyspark.sql import functions as F
from pyspark.sql.types import *

spark = SparkSession.builder \
    .appName("StockStreaming") \
    .config("spark.sql.extensions", 
            "io.delta.sql.DeltaSparkSessionExtension") \
    .getOrCreate()

schema = StructType([
    StructField("symbol", StringType()),
    StructField("price", DoubleType()),
    StructField("volume", LongType()),
    StructField("timestamp", LongType())
])

stream = spark.readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "stock_prices") \
    .load() \
    .select(F.from_json(F.col("value").cast("string"), schema).alias("d")) \
    .select("d.*") \
    .withColumn("event_time", 
        F.to_timestamp(F.col("timestamp") / 1000)) \
    .withWatermark("event_time", "30 seconds")

# 1-minute OHLCV bars
ohlcv = stream \
    .groupBy(
        F.window("event_time", "1 minute"),
        "symbol"
    ) \
    .agg(
        F.first("price").alias("open"),
        F.max("price").alias("high"),
        F.min("price").alias("low"),
        F.last("price").alias("close"),
        F.sum("volume").alias("volume"),
        F.count("*").alias("tick_count")
    )

ohlcv.writeStream \
    .format("delta") \
    .option("checkpointLocation", "/tmp/checkpoint/ohlcv") \
    .outputMode("append") \
    .start("data/delta/stock_ohlcv")

spark.streams.awaitAnyTermination()
```

---

## 🎯 Project 2: dbt + Airflow Data Warehouse

**What you'll build:** Full data warehouse with dbt models orchestrated by Airflow.

**Architecture:**
```
PostgreSQL (OLTP) → Airbyte → Snowflake (raw) → dbt → Analytics Layer
                                                          ↑
                                                       Airflow
```

```yaml
# dbt project structure
models/
  staging/           # 1:1 with source tables, minimal transforms
    stg_orders.sql
    stg_customers.sql
    stg_products.sql
  intermediate/      # Business logic, JOINs
    int_order_items.sql
  marts/             # Business-facing, ready for BI
    finance/
      fct_orders.sql
      fct_refunds.sql
    marketing/
      fct_customer_cohorts.sql
      fct_campaign_performance.sql
    operations/
      fct_inventory.sql
```

```sql
-- models/staging/stg_orders.sql
{{ config(materialized='view') }}

SELECT
    order_id::BIGINT                    AS order_id,
    customer_id::BIGINT                 AS customer_id,
    LOWER(TRIM(status))                 AS status,
    order_date::DATE                    AS order_date,
    total_amount::NUMERIC               AS total_amount,
    _sdc_received_at                    AS _ingested_at
FROM {{ source('raw', 'orders') }}
WHERE order_id IS NOT NULL
  AND total_amount IS NOT NULL

-- models/marts/finance/fct_orders.sql
{{ config(
    materialized='incremental',
    unique_key='order_id',
    on_schema_change='sync_all_columns'
) }}

WITH orders AS (SELECT * FROM {{ ref('stg_orders') }}
    {% if is_incremental() %}
    WHERE order_date > (SELECT MAX(order_date) - INTERVAL '3 days' FROM {{ this }})
    {% endif %}
),
customers AS (SELECT * FROM {{ ref('stg_customers') }}),
order_items AS (SELECT * FROM {{ ref('int_order_items') }})

SELECT
    o.order_id,
    o.customer_id,
    c.segment,
    c.region,
    o.order_date,
    o.status,
    oi.gross_revenue,
    oi.discount_amount,
    oi.net_revenue,
    oi.item_count,
    ROW_NUMBER() OVER (
        PARTITION BY o.customer_id ORDER BY o.order_date
    ) = 1 AS is_first_order
FROM orders o
LEFT JOIN customers c USING (customer_id)
LEFT JOIN order_items oi USING (order_id)
```

---

*Back to [[05-Projects/Beginner-Projects]] | Next: [[05-Projects/Advanced-Projects]] | [[00-Index]]*
