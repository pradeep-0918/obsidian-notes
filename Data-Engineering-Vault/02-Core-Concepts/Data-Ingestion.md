# 📥 Data Ingestion — Complete Guide
#concept #ingestion #etl #cdc #streaming

> Getting data from where it lives to where you need it. Sounds simple. It's not.

---

## 🧠 The Mental Model

Data ingestion is like a postal service:
- **Sources** = Senders (databases, APIs, files)
- **Ingestion layer** = Post offices (Kafka, Airbyte, etc.)
- **Destinations** = Recipients (data warehouse, data lake)
- **Reliability** = Guaranteed delivery (at-least-once, exactly-once)

**The three hard problems in ingestion:**
1. **Volume** — How do you move terabytes efficiently?
2. **Variety** — APIs, databases, files, events, all at once?
3. **Velocity** — Do you need it now (streaming) or later (batch)?

---

## 🔄 Ingestion Patterns

### 1. Full Load (Batch)

Simple but expensive. Every run = move everything.

```python
# Full load pattern
def full_load(source_table: str, dest_table: str):
    df = spark.read.jdbc(
        url=source_jdbc_url,
        table=source_table,
        properties={"user": user, "password": password}
    )
    
    df.write \
        .mode("overwrite") \   # Drop and replace
        .saveAsTable(dest_table)
```

**Use when:**
- Table is small (<1M rows)
- No reliable "updated_at" column
- Initial historical load

**Avoid when:**
- Table is large (too slow, too expensive)
- You need near-real-time data

---

### 2. Incremental Load

Only move new/changed data. Requires a watermark.

```python
from datetime import datetime, timedelta

def incremental_load(
    source_table: str,
    dest_table: str,
    watermark_column: str = "updated_at"
):
    # Get last successful load timestamp
    last_watermark = get_last_watermark(dest_table)
    current_watermark = datetime.utcnow()
    
    # Extract only changed records
    df = spark.read.jdbc(
        url=source_jdbc_url,
        table=f"""(
            SELECT * FROM {source_table}
            WHERE {watermark_column} > '{last_watermark}'
            AND {watermark_column} <= '{current_watermark}'
        ) AS incremental""",
        properties=conn_props
    )
    
    # Upsert (merge) into destination
    df.createOrReplaceTempView("source")
    spark.sql(f"""
        MERGE INTO {dest_table} AS target
        USING source
        ON target.id = source.id
        WHEN MATCHED THEN UPDATE SET *
        WHEN NOT MATCHED THEN INSERT *
    """)
    
    # Save watermark for next run
    save_watermark(dest_table, current_watermark)
```

**Challenges with incremental:**
- Deletes are invisible (use CDC instead)
- Clock skew can cause missed records
- "updated_at" columns aren't always reliable

---

### 3. Change Data Capture (CDC)

The gold standard for real-time database replication. Reads the **transaction log** of the source database.

```
MySQL/PostgreSQL → Binary Log → Debezium → Kafka → Target
```

**How it works:**
```
Every database change writes to a transaction log first.
Debezium tails this log and converts changes to events:

INSERT → {"op": "c", "after": {"id": 1, "name": "Alice"}}
UPDATE → {"op": "u", "before": {"name": "Alice"}, "after": {"name": "Alicia"}}
DELETE → {"op": "d", "before": {"id": 1, "name": "Alicia"}}
```

**Setting up Debezium for PostgreSQL:**
```json
{
  "name": "postgres-connector",
  "config": {
    "connector.class": "io.debezium.connector.postgresql.PostgresConnector",
    "database.hostname": "postgres",
    "database.port": "5432",
    "database.user": "debezium",
    "database.password": "secret",
    "database.dbname": "mydb",
    "plugin.name": "pgoutput",
    "publication.name": "debezium_pub",
    "slot.name": "debezium_slot",
    "table.include.list": "public.orders,public.customers",
    "topic.prefix": "myapp",
    "transforms": "unwrap",
    "transforms.unwrap.type": "io.debezium.transforms.ExtractNewRecordState"
  }
}
```

**Processing CDC events in Python:**
```python
from confluent_kafka import Consumer
import json

def process_cdc_events():
    consumer = Consumer({
        'bootstrap.servers': 'kafka:9092',
        'group.id': 'cdc-sink-warehouse',
        'auto.offset.reset': 'earliest'
    })
    consumer.subscribe(['myapp.public.orders'])
    
    while True:
        msg = consumer.poll(1.0)
        if msg is None:
            continue
        
        event = json.loads(msg.value())
        operation = event.get('op')  # 'c', 'u', 'd', 'r' (read/snapshot)
        
        if operation in ('c', 'r'):  # Insert or snapshot
            upsert_to_warehouse(event['after'])
        elif operation == 'u':  # Update
            upsert_to_warehouse(event['after'])
        elif operation == 'd':  # Delete
            soft_delete_in_warehouse(event['before']['id'])
        
        consumer.commit()
```

---

## 🛠️ Ingestion Tools Compared

### Apache Kafka

The backbone of real-time data pipelines. Not just a queue — a **durable, distributed, ordered log**.

```
                 ┌─────────────────────────────────┐
Producers ──────►│          Kafka Cluster           │──────► Consumers
                 │  ┌──────────────────────────┐   │
                 │  │     Topic: "orders"       │   │
                 │  │  Partition 0: [1][2][3]   │   │
                 │  │  Partition 1: [1][2][3]   │   │
                 │  │  Partition 2: [1][2][3]   │   │
                 │  └──────────────────────────┘   │
                 └─────────────────────────────────┘
```

See [[03-Tools/Kafka]] for complete guide.

---

### Airbyte — Open Source EL(T)

Best for: SaaS data sources (Salesforce, HubSpot, Google Ads, etc.)

```python
# Airbyte via Python SDK (unofficial)
import requests

AIRBYTE_URL = "http://localhost:8000/api/v1"

# Create a connection
connection = requests.post(f"{AIRBYTE_URL}/connections/create", json={
    "sourceId": "source-uuid",
    "destinationId": "destination-uuid",
    "syncCatalog": {
        "streams": [{
            "stream": {"name": "orders", "namespace": "public"},
            "config": {
                "syncMode": "incremental",
                "cursorField": ["updated_at"],
                "destinationSyncMode": "append_dedup",
                "primaryKey": [["id"]]
            }
        }]
    },
    "scheduleType": "cron",
    "scheduleData": {"cron": {"cronExpression": "0 */6 * * *", "cronTimeZone": "UTC"}}
})
```

---

### dlt — Python-Native Data Loading

Perfect for custom pipelines with minimal boilerplate:

```python
import dlt
import requests

# Define a source
@dlt.source
def github_source(owner: str, repo: str):
    
    @dlt.resource(write_disposition="merge", primary_key="id")
    def issues():
        url = f"https://api.github.com/repos/{owner}/{repo}/issues"
        while url:
            response = requests.get(url)
            yield response.json()
            url = response.links.get("next", {}).get("url")
    
    return issues

# Run the pipeline
pipeline = dlt.pipeline(
    pipeline_name="github_data",
    destination="bigquery",
    dataset_name="raw_github"
)

load_info = pipeline.run(github_source("apache", "spark"))
print(load_info)
```

---

### Apache Sqoop (Batch, Legacy)

For bulk transfer between Hadoop and relational databases:

```bash
# Import from MySQL to HDFS
sqoop import \
  --connect jdbc:mysql://mysql-server/mydb \
  --username myuser \
  --password mypassword \
  --table orders \
  --where "created_at > '2024-01-01'" \
  --target-dir /data/warehouse/orders \
  --as-parquetfile \
  --num-mappers 8 \
  --split-by order_id

# Import with incremental mode
sqoop import \
  --connect jdbc:mysql://mysql-server/mydb \
  --table orders \
  --incremental lastmodified \
  --check-column updated_at \
  --last-value "2024-01-01 00:00:00" \
  --target-dir /data/warehouse/orders_incremental
```

---

## ⚡ Real-Time vs Batch Ingestion

```
Decision Framework:

How fresh does the data need to be?
        │
        ├─ Within seconds/minutes? → Streaming (Kafka + Flink/Spark Streaming)
        │
        ├─ Within hours? → Micro-batch (Spark Streaming 5-min windows)
        │
        ├─ Daily is fine? → Batch (Airflow + Spark)
        │
        └─ Monthly is fine? → Simple ETL (Python + SQL)

How much data?
        ├─ <1GB → Python + pandas is fine
        ├─ 1GB-1TB → Spark on a small cluster
        └─ >1TB → Spark on auto-scaling cluster
```

---

## 🔒 Ingestion Best Practices

### 1. Idempotency

Your pipeline WILL fail and retry. Make it safe to run multiple times:

```python
# BAD: Running twice = duplicate data
def load(df):
    df.write.mode("append").parquet("s3://bucket/data/")

# GOOD: Running twice = same result
def load(df, run_date: str):
    df.write \
        .mode("overwrite") \
        .parquet(f"s3://bucket/data/date={run_date}/")
```

### 2. Schema Evolution

```python
# Handle schema changes gracefully
from pyspark.sql.types import StructType

# Merge schemas when reading (handles added columns)
df = spark.read \
    .option("mergeSchema", "true") \
    .parquet("s3://bucket/data/")

# Or define explicit schema with nullable fields
schema = StructType([
    StructField("id", LongType(), nullable=False),
    StructField("name", StringType(), nullable=True),
    StructField("new_field", StringType(), nullable=True),  # New!
])
```

### 3. Dead Letter Queues

Route bad records to a separate location for inspection:

```python
def process_with_dlq(records):
    good_records = []
    bad_records = []
    
    for record in records:
        try:
            validated = validate_and_transform(record)
            good_records.append(validated)
        except Exception as e:
            bad_records.append({
                "original_record": record,
                "error": str(e),
                "timestamp": datetime.utcnow().isoformat(),
                "pipeline": "orders_ingestion"
            })
    
    if bad_records:
        write_to_dlq(bad_records, "s3://bucket/dlq/orders/")
        alert_on_call_if_needed(bad_records)
    
    return good_records
```

### 4. Backfilling

When you add a new pipeline, you often need historical data:

```python
# Airflow backfill command
# airflow dags backfill --start-date 2024-01-01 --end-date 2024-06-01 my_pipeline_dag

# In your DAG, always use execution_date (not datetime.now())
def extract(**context):
    execution_date = context['ds']  # '2024-01-15'
    data = fetch_data_for_date(execution_date)
    return data
```

---

## 🧰 Tool Selection Guide

| Scenario | Recommended Tool | Why |
|---|---|---|
| SaaS → Warehouse | Airbyte / Fivetran | 200+ connectors, no code |
| DB → DB (real-time) | Debezium + Kafka | CDC, low latency |
| Custom API → Warehouse | dlt | Minimal code, built-in state |
| Files → Hadoop | Apache Sqoop | Designed for HDFS |
| Log streaming | Fluentd / Logstash | Pluggable, reliable |
| Multi-source → Kafka | Apache NiFi | Visual, no-code |
| Event streaming | Apache Kafka | The standard |

---

*Related: [[03-Tools/Kafka]] | [[02-Core-Concepts/Stream-Processing]] | [[04-System-Design/ETL-Pipelines]] | Back to [[00-Index]]*
