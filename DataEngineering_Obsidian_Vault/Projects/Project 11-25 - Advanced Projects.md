# Project 11 - Real-Time Clickstream Analytics (Advanced)

#data-engineering #advanced #kafka #spark-streaming #kinesis #bigquery #real-time

---

## 📌 Overview

An advanced, production-grade real-time clickstream pipeline that processes user behavior events from websites and mobile apps at scale using Kafka, Spark Streaming, and Kinesis. This architecture mirrors what FAANG companies run to power their recommendation engines, A/B testing platforms, and real-time dashboards.

---

## 🎯 Scope
- **Data Ingestion**: Extract and process real-time clickstream data from websites and mobile apps
- **Processing**: Clean, enrich, and transform user behavior data
- **Storage**: Store structured clickstream data in AWS Kinesis / Azure Event Hub / Google BigQuery
- **Visualization**: Generate insights on user engagement and interaction patterns

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | Web & Mobile App Logs (Google Analytics, Firebase, Apache Server Logs) | Event sources |
| Processing | Python, PySpark, Apache Kafka, Spark Streaming | Stream processing |
| Storage | AWS Kinesis / Azure Event Hub / Google BigQuery | Real-time + analytical storage |
| Visualization | Google Data Studio / Power BI / Tableau | Engagement dashboards |

---

## 🔄 Advanced Architecture

```
[Client Side]
    JavaScript SDK → fires events on user action
         │
         ▼
[Event Ingestion]
    Apache Kafka (high throughput)
    OR AWS Kinesis (managed, AWS-native)
    Partitioned by: user_id
    Schema: Apache Avro + Schema Registry
         │
         ▼
[Stream Processing]
    Apache Spark Streaming (PySpark)
    - Exactly-once semantics
    - Session stitching (30-min window)
    - Bot filtering (IP reputation, user-agent checks)
    - Real-time enrichment (user profile lookup)
    - Deduplication (event_id uniqueness check)
         │
         ├──▶ [Google BigQuery]
         │        Partitioned by event_date
         │        Clustered by user_id, event_type
         │
         └──▶ [Redis / Cassandra]
                  Real-time counters (DAU, active sessions)
                       │
                       ▼
              [BI / ML Layer]
              Recommendation Engine + Dashboards
```

---

## ⚠️ Key Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| High Data Volume & Velocity | Streaming frameworks (Kafka & Kinesis) |
| Data Duplication Issues | Deduplication & session tracking |
| Cross-Device Tracking | Standardize user identifiers across platforms |
| Schema Evolution | Apache Avro + Confluent Schema Registry |

---

## 🏢 Industry Insights

### Netflix's Clickstream Architecture
Netflix's **Keystone** pipeline processes 700GB+ of compressed event data per hour. It uses Kafka for event streaming, Flink for stateful processing, and stores data in both **Elasticsearch** (for real-time search) and **S3/Iceberg** (for batch analytics). The entire recommendation model is retrained daily on this clickstream data.

### Meta's Event System
Meta processes **trillion events per day** across Facebook, Instagram, and WhatsApp. Their internal system **Scribe** (essentially a distributed log) feeds into **Manhattan** (their distributed key-value store) for real-time features and **Hive/Presto** for batch analytics.

### Uber's Real-Time Analytics
Uber uses **Apache Pinot** (an OLAP system they open-sourced) for sub-second clickstream analytics. This powers driver surge pricing, ETA predictions, and fraud detection all in real-time.

---

## 🚀 Optimization Techniques

- **BigQuery Materialized Views**: Pre-compute expensive aggregations
- **Kafka Log Compaction**: For event deduplication and state preservation
- **Flink State Backends**: RocksDB for large stateful operations (session tracking)
- **Spark Structured Streaming**: Use `trigger(processingTime='30 seconds')` for micro-batch

---

## 🔗 Related Projects

- [[Project 05 - Clickstream Pipeline]] — Beginner version of this pipeline
- [[Project 16 - Real-Time Financial Fraud Detection]] — Similar streaming pattern for fraud
- [[Streaming vs Batch Processing]] — Architecture decision rationale
- [[Advanced & Underrated Tools]] — Apache Pinot for real-time OLAP

---

*Tags: #data-engineering #advanced #kafka #spark-streaming #kinesis #bigquery #clickstream #real-time*

---
---

# Project 12 - IoT Data Lake & Warehouse (Advanced)

#data-engineering #advanced #iot #kafka #flink #snowflake #data-lake

---

## 📌 Overview

A complete IoT data architecture combining real-time streaming (Kafka/Flink) with batch analytics (Hadoop/Spark), storing raw data in a data lake (S3/GCS/Azure) and aggregated data in cloud warehouses (Snowflake/BigQuery/Redshift). This is the Lambda Architecture pattern applied to IoT.

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | IoT Sensors (Temp, Humidity), MQTT, Kafka, Kinesis | Streaming sensor data |
| Batch Processing | Hadoop (HDFS, Hive), Spark (PySpark) | Historical data processing |
| Real-Time Processing | Apache Kafka, Apache Flink, Spark Streaming | Live stream processing |
| Raw Storage | Amazon S3 / Google Cloud Storage / Azure Data Lake | Data lake (raw + processed) |
| Structured Storage | Snowflake / Google BigQuery / Amazon Redshift | Data warehouse |
| Visualization | Tableau / Power BI / Looker / Grafana | Dashboards |

---

## 🔄 Lambda Architecture for IoT

```
[IoT Sensors]
         │
         ├──── [Batch Layer] ────────────────────▶
         │     Hadoop (HDFS) + Spark              │
         │     Daily batch jobs                   │
         │                                        ▼
         │                               [Serving Layer]
         │                               Snowflake / Redshift
         │                               Combined batch + speed views
         │                                        ▲
         └──── [Speed Layer] ──────────────────── │
               Kafka → Flink → Redis              │
               Real-time aggregations ────────────
```

---

## 📊 Use Cases
- **Predictive Maintenance**: Identify device failures before they happen
- **Real-time IoT Monitoring**: Track live sensor data, send alerts
- **Anomaly Detection**: Detect unusual temperature or device behavior patterns

---

## 🏢 Industry Insights

### How Siemens Handles Industrial IoT
Siemens' MindSphere platform processes data from millions of industrial devices globally. Their architecture uses **Apache Kafka** for ingestion, **Apache Flink** for real-time processing, and **Snowflake** for analytics — exactly this project's stack.

### How AWS IoT Works
AWS IoT Core acts as a managed MQTT broker that can route messages to Kinesis, Lambda, S3, or DynamoDB. This eliminates the need to manage your own Mosquitto broker cluster.

---

## 🚀 Key Optimizations

- **InfluxDB or TimescaleDB** instead of PostgreSQL for time-series IoT data
- **Kafka KSQL**: Stream processing without writing Java/Scala code
- **Apache Iceberg on S3**: Better than raw Parquet — supports time travel and schema evolution
- **Grafana Alerting**: Configure threshold-based alerts directly in Grafana

---

## 🔗 Related Projects

- [[Project 08 - IoT Sensor Data Pipeline]] — Beginner IoT version
- [[Project 21 - Azure IoT Data Pipeline]] — Azure-specific IoT architecture
- [[Data Lake vs Data Warehouse vs Lakehouse]] — Understanding storage layers
- [[Advanced & Underrated Tools]] — Apache Iceberg for the data lake layer

---

*Tags: #data-engineering #advanced #iot #kafka #flink #spark #snowflake #data-lake #lambda-architecture*

---
---

# Project 13 - Hadoop Batch ETL with Airflow (Advanced)

#data-engineering #advanced #hadoop #spark #hive #airflow #batch-etl

---

## 📌 Overview

A Hadoop-based batch ETL pipeline that processes customer transaction data using PySpark, stores raw and transformed data in HDFS, uses Hive for structured querying, and automates the entire workflow with Apache Airflow.

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | Customer transaction CSV (daily) | Transaction records |
| Processing | Apache Spark (PySpark) | Batch ETL & transformation |
| Raw Storage | HDFS (Hadoop Distributed File System) | Raw data lake |
| Structured Storage | Apache Hive | Structured analytical queries |
| Orchestration | Apache Airflow | Workflow scheduling & monitoring |

---

## 📊 Use Cases
- **Customer Spending Analysis**: Identify top customers by spending
- **Fraud Detection**: Detect anomalies in transaction patterns
- **Sales Performance Insights**: Generate daily and monthly sales reports

---

## 🔍 Deep Dive — Hive Optimization

```sql
-- Partitioned Hive Table for fast queries
CREATE TABLE transactions_partitioned (
    transaction_id STRING,
    customer_id STRING,
    amount DOUBLE,
    merchant STRING
)
PARTITIONED BY (year INT, month INT, day INT)
STORED AS ORC
TBLPROPERTIES ('orc.compress'='SNAPPY');

-- Optimized query using partition pruning
SELECT customer_id, SUM(amount) as total_spend
FROM transactions_partitioned
WHERE year = 2024 AND month = 12
GROUP BY customer_id
ORDER BY total_spend DESC
LIMIT 100;
```

---

## 🏢 Industry Insights

### How Yahoo! Uses Hadoop at Scale
Yahoo! has one of the world's largest Hadoop clusters (40,000+ nodes). They run PySpark jobs for daily batch ETL of ad click data, search logs, and content recommendations. Airflow orchestrates thousands of daily DAGs.

### The Transition from Hadoop to Cloud
Many companies are **migrating from on-premise Hadoop to cloud** (S3 + EMR or GCS + Dataproc). The economics favor cloud: no hardware management, pay-per-use, auto-scaling. Hive is being replaced by **BigQuery** or **Snowflake** in most new architectures.

---

## 🔗 Related Projects

- [[Project 03 - Retail POS ETL Pipeline]] — Simpler ETL with Airflow
- [[Project 14 - E-Commerce Spark + Athena Pipeline]] — Cloud-native version of this
- [[Orchestration with Apache Airflow]] — Deep Airflow concepts

---

*Tags: #data-engineering #advanced #hadoop #spark #hive #airflow #hdfs #batch-etl #transaction-processing*

---
---

# Project 14 - E-Commerce Spark + Athena Pipeline (Advanced)

#data-engineering #advanced #spark #aws-athena #s3 #serverless #e-commerce

---

## 📌 Overview

A serverless analytics pipeline that processes e-commerce sales data using PySpark, stores it in Amazon S3 as Parquet, and enables cost-effective SQL analytics using AWS Athena — no database servers required. This is the modern, cloud-native replacement for traditional data warehouses for analytical workloads.

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | E-commerce sales transactions (CSV/JSON) | Order data |
| Processing | Apache Spark (PySpark) | Batch ETL & transformation |
| Raw Storage | Amazon S3 (Data Lake) | Raw data preservation |
| Processed Storage | Amazon S3 (Partitioned Parquet for Athena) | Analytics-optimized storage |
| Querying | AWS Athena (Serverless SQL on S3) | Ad-hoc business queries |

---

## 🔍 Deep Dive — Athena Optimization

```sql
-- Athena query with partition pruning (FAST - scans only Dec data)
SELECT product_id,
       SUM(quantity) as total_sold,
       SUM(revenue) as total_revenue
FROM ecommerce.sales
WHERE year = '2024' AND month = '12'  -- Partition filter!
GROUP BY product_id
ORDER BY total_revenue DESC;

-- Without partition filter (SLOW - full table scan, expensive!)
SELECT * FROM ecommerce.sales WHERE product_id = 'P001';
```

```python
# PySpark: Write Athena-optimized Parquet
df_clean.write \
    .partitionBy("year", "month", "day") \
    .option("compression", "snappy") \
    .mode("overwrite") \
    .parquet("s3://my-datalake/processed/sales/")

# Register in Glue Data Catalog for Athena
import boto3
glue_client = boto3.client('glue')
# Run Glue Crawler to auto-discover Parquet schema
```

---

## 📊 Use Cases
- **Sales Performance Analysis**: Identify top-selling products by region and time
- **Customer Behavior Insights**: Understand purchase patterns, cart abandonment
- **Revenue Trend Analysis**: Analyze daily, monthly, and yearly revenue trends

---

## 🏢 Industry Insights

### Serverless Analytics at Scale — The Athena Pattern
Athena is used by companies like **Airbnb, Lyft, and Pinterest** for ad-hoc analytics on S3 data lakes. The cost model ($5 per TB scanned) makes it extremely economical compared to keeping a Redshift cluster running 24/7. Companies **save 60–80%** on analytics infrastructure costs by switching to S3 + Athena.

### The S3 Data Lake Pattern (Databricks/Delta Lake Alternative)
While Athena is powerful, it lacks ACID transactions. Companies that need updates/deletes use **Apache Iceberg** or **Delta Lake** on top of S3, which Athena also supports via the Iceberg table format.

---

## 🚀 Optimization Techniques

- **Parquet + Snappy**: Best balance of compression ratio and query speed for Athena
- **Partition Projection**: Use Athena partition projection to avoid Glue crawler costs
- **CTAS (Create Table As Select)**: Use Athena CTAS to create optimized tables from raw data
- **Workgroup Budgets**: Set Athena workgroup query scan limits to control costs

---

## 🔗 Related Projects

- [[Project 13 - Hadoop Batch ETL with Airflow]] — On-premise Hadoop equivalent
- [[Project 19 - Sales Analytics with Redshift + QuickSight]] — Full data warehouse version
- [[Data Lake vs Data Warehouse vs Lakehouse]] — When to use Athena vs Redshift
- [[Advanced & Underrated Tools]] — Apache Iceberg for ACID on S3

---

*Tags: #data-engineering #advanced #spark #pyspark #aws-athena #s3 #parquet #serverless #e-commerce*

---
---

# Project 15 - Kafka + Spark Streaming + Cassandra (Advanced)

#data-engineering #advanced #kafka #spark-streaming #cassandra #hive #lambda-architecture

---

## 📌 Overview

A Lambda Architecture implementation that streams real-time user activity data via Apache Kafka, processes it with Spark Streaming, stores fast-access data in Apache Cassandra (NoSQL), and historical analytics data in Apache Hive.

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | User activity logs (clicks, page views) via Kafka | Event streaming |
| Real-Time Processing | Apache Spark Streaming (PySpark) | ETL & transformations |
| Fast NoSQL Storage | Apache Cassandra | Real-time lookups |
| Analytical Storage | Apache Hive | Historical trend analysis |

---

## 🔍 Deep Dive — Cassandra Schema Design

```cql
-- Cassandra table optimized for user lookup (partition by user_id)
CREATE TABLE user_activity (
    user_id UUID,
    event_timestamp TIMESTAMP,
    event_type TEXT,
    page_url TEXT,
    session_id UUID,
    PRIMARY KEY (user_id, event_timestamp)
) WITH CLUSTERING ORDER BY (event_timestamp DESC)
  AND compaction = {'class': 'TimeWindowCompactionStrategy',
                    'compaction_window_unit': 'HOURS',
                    'compaction_window_size': 1};

-- Query: Get last 10 events for a user (extremely fast)
SELECT * FROM user_activity
WHERE user_id = ? LIMIT 10;
```

---

## 📊 Use Cases
- **Real-Time User Behavior Analysis**: Track actions on a website or app
- **Fraud Detection**: Detect unusual login attempts or suspicious transactions
- **Personalized Recommendations**: Suggest content based on recent user interactions

---

## ⚠️ Key Challenges

| Challenge | Solution |
|-----------|----------|
| High-Velocity Streaming Data | Kafka for scalable ingestion |
| Efficient NoSQL Storage | Proper Cassandra schema design |
| Combining Real-Time & Batch | Lambda Architecture (Cassandra + Hive) |

---

## 🏢 Industry Insights

### How Instagram Uses Cassandra
Instagram stores hundreds of billions of user activity records in Apache Cassandra. They process ~500 million events daily and use Cassandra for its **linear horizontal scalability** — adding more nodes increases throughput proportionally.

### The Shift from Lambda to Kappa Architecture
Many companies have simplified from Lambda (batch + streaming) to **Kappa Architecture** (streaming only) using Apache Flink or Spark Structured Streaming with Delta Lake. The simplified architecture reduces operational overhead significantly.

---

## 🔗 Related Projects

- [[Project 11 - Real-Time Clickstream Analytics]] — Similar real-time pattern
- [[Project 16 - Real-Time Financial Fraud Detection]] — Fraud detection using streaming
- [[Streaming vs Batch Processing]] — Lambda vs Kappa architecture comparison

---

*Tags: #data-engineering #advanced #kafka #spark-streaming #cassandra #hive #lambda-architecture #nosql*

---
---

# Project 16 - Real-Time Financial Fraud Detection (Advanced)

#data-engineering #advanced #kafka #flink #bigquery #fraud-detection #fintech

---

## 📌 Overview

A high-throughput, low-latency real-time fraud detection pipeline that streams financial transactions via Kafka, processes them with Apache Flink for anomaly detection, and stores results in Google BigQuery. This is among the most critical data engineering use cases in the financial industry.

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | Financial transactions (credit/debit, bank transfers) via Kafka | Transaction stream |
| Real-Time Processing | Apache Flink | Stream processing, anomaly detection |
| Analytical Storage | Google BigQuery | Structured querying and BI reporting |

---

## 🔄 Fraud Detection Architecture

```
[Payment Systems]
Card swipes, online transactions, bank transfers
         │
         ▼
[Apache Kafka]
    Topic: financial-transactions
    Partitioned by: account_id
    Retention: 24 hours (high-frequency data)
         │
         ▼
[Apache Flink - Fraud Detection Engine]
    Rule-based checks (fast, deterministic):
    - Amount > $10,000 (flag for review)
    - 5+ transactions in 60 seconds from same card
    - Geographic impossibility (NY at 1pm, LA at 1:05pm)
    
    ML-based checks (model scoring):
    - Call fraud scoring model via REST API
    - Ensemble of anomaly detection models
         │
         ├──▶ [Alert System]
         │        Kafka Topic: fraud-alerts
         │        → Send to fraud analyst dashboard
         │        → Block transaction in real-time
         │
         └──▶ [Google BigQuery]
                  All transactions + fraud scores stored
                  Advanced analytics + model retraining
```

---

## 📊 Use Cases
- **Fraud Detection in Real-Time**: Identify suspicious transactions instantly (< 100ms)
- **Customer Spending Pattern Analysis**: Track and analyze purchase behavior
- **Real-Time Transaction Monitoring**: Detect anomalies and generate instant alerts

---

## ⚠️ Key Challenges

| Challenge | Solution |
|-----------|----------|
| High-Throughput Streaming Data | Optimized Flink pipeline |
| Low-Latency Processing | Minimize Flink checkpoint intervals |
| Efficient BigQuery Storage | Optimize partitioning and clustering |

---

## 🏢 Industry Insights

### How PayPal Detects Fraud
PayPal processes **13 million transactions per day** and uses a combination of rule-based systems and deep learning models to detect fraud. Their fraud detection pipeline achieves **< 300ms latency** from transaction to decision.

### How Stripe Uses Real-Time ML
Stripe's **Radar** fraud detection system uses ML models that score each transaction in **< 100ms**. They use Apache Kafka for event ingestion, a proprietary stream processor for ML scoring, and PostgreSQL + BigQuery for storage. Their false positive rate is kept below 0.1% to avoid blocking legitimate transactions.

### How Mastercard's Decision Intelligence Works
Mastercard analyzes **165 million transactions per hour** globally. Their AI system uses pattern recognition across 80 data points per transaction including merchant category, geography, time, and spending history.

---

## 🚀 Optimization Techniques

- **Flink CEP (Complex Event Processing)**: Detect fraud patterns across multiple related events
- **RocksDB State Backend**: Store account state for stateful fraud rules in Flink
- **BigQuery Clustering**: Cluster by `account_id` + `transaction_date` for fast lookups
- **Flink Savepoints**: Enable rolling upgrades of Flink jobs without data loss

```python
# Apache Flink CEP - Detect rapid consecutive transactions
from pyflink.datastream import StreamExecutionEnvironment
from pyflink.datastream.connectors.kafka import KafkaSource

env = StreamExecutionEnvironment.get_execution_environment()
env.set_parallelism(8)  # Scale to 8 parallel workers

kafka_source = KafkaSource.builder() \
    .set_bootstrap_servers("kafka:9092") \
    .set_topics("financial-transactions") \
    .set_group_id("fraud-detector") \
    .build()

ds = env.from_source(kafka_source, WatermarkStrategy.no_watermarks(), "Kafka Source")
# Apply fraud detection logic...
env.execute("Fraud Detection Pipeline")
```

---

## 🔗 Related Projects

- [[Project 15 - Kafka + Spark Streaming + Cassandra]] — Similar Lambda Architecture
- [[Project 17 - Stock Market Analytics with GCP]] — Another financial data pipeline
- [[Streaming vs Batch Processing]] — Why fraud detection MUST be real-time
- [[Big Data Tools Overview]] — Apache Flink deep dive

---

*Tags: #data-engineering #advanced #kafka #flink #bigquery #fraud-detection #fintech #real-time #ml*

---
---

# Project 17–25 Summary Notes

> These projects follow the same deep patterns. Below are structured summaries with key differentiators.

---

# Project 17 - Stock Market Analytics with GCP

#data-engineering #advanced #gcp #spark-sql #pubsub #stock-market

**Core Stack**: Alpha Vantage/Yahoo Finance API → PySpark (Batch + SQL) → GCS/BigQuery → Google Pub/Sub

**Key Differentiator**: Uses **Google Pub/Sub** for event-driven architecture — when new stock data arrives, Pub/Sub triggers downstream processing automatically.

**Industry Insight**: Bloomberg and Reuters use similar API → Pub/Sub → BigQuery patterns. Google Pub/Sub guarantees at-least-once delivery with global distribution.

**Key Technique**:
```python
# Google Pub/Sub publisher
from google.cloud import pubsub_v1
publisher = pubsub_v1.PublisherClient()
topic_path = publisher.topic_path('my-project', 'stock-updates')
data = json.dumps({'symbol': 'AAPL', 'price': 189.50}).encode('utf-8')
publisher.publish(topic_path, data)
```

**Related**: [[Project 06 - Public API Data Pipeline]], [[Cloud Platforms - AWS vs GCP vs Azure]]

---

# Project 18 - E-Commerce Kinesis + Lambda Pipeline

#data-engineering #advanced #aws-kinesis #aws-lambda #serverless #e-commerce

**Core Stack**: E-commerce order events (JSON) → AWS Kinesis → AWS Lambda → Spark Batch → Amazon S3

**Key Differentiator**: **Serverless event-driven architecture** — Lambda functions trigger automatically on Kinesis stream events, enabling fully managed, auto-scaling ingestion with no servers to maintain.

**Industry Insight**: Amazon.com itself uses Kinesis + Lambda for real-time order processing. The serverless model reduces operational overhead by ~70% compared to managing Kafka clusters.

**Lambda Pattern**:
```python
# AWS Lambda function triggered by Kinesis
def lambda_handler(event, context):
    for record in event['Records']:
        payload = base64.b64decode(record['kinesis']['data'])
        order_data = json.loads(payload)
        # Process and store order
        store_to_s3(order_data)
    return {'statusCode': 200}
```

**Related**: [[Project 14 - E-Commerce Spark + Athena Pipeline]], [[Cloud Platforms - AWS vs GCP vs Azure]]

---

# Project 19 - Sales Analytics with Redshift + QuickSight

#data-engineering #advanced #redshift #quicksight #aws #data-warehouse

**Core Stack**: Multi-channel sales data → PySpark ETL → Amazon Redshift → AWS QuickSight

**Key Differentiator**: Full **AWS data warehouse stack** — Redshift is a massively parallel processing (MPP) columnar database, and QuickSight is a serverless BI tool natively integrated with it.

**Industry Insight**: Amazon Redshift powers analytics for thousands of enterprises including Nasdaq, McDonald's, and Pinterest. Its **AQUA (Advanced Query Accelerator)** hardware offloads query processing to near-storage compute.

**Redshift Optimization**:
```sql
-- Optimal Redshift table design
CREATE TABLE sales (
    sale_id BIGINT ENCODE ZSTD,
    customer_id INTEGER ENCODE ZSTD,
    product_id INTEGER ENCODE ZSTD,
    sale_date DATE ENCODE ZSTD,
    revenue DECIMAL(10,2) ENCODE ZSTD
)
DISTSTYLE KEY DISTKEY(customer_id)  -- Co-locate customer data
SORTKEY(sale_date);                  -- Fast date range queries

-- Load from S3 using COPY (10-100x faster than INSERT)
COPY sales FROM 's3://bucket/sales/'
IAM_ROLE 'arn:aws:iam::123:role/RedshiftS3Role'
FORMAT AS PARQUET;
```

**Related**: [[Project 14 - E-Commerce Spark + Athena Pipeline]], [[Data Lake vs Data Warehouse vs Lakehouse]]

---

# Project 20 - Mobile App Analytics on GCP

#data-engineering #advanced #gcp #dataproc #bigquery #cloud-spanner #mobile

**Core Stack**: Mobile app logs (JSON) → Google Cloud Dataproc (Spark) → Cloud Spanner (OLTP) + BigQuery (OLAP)

**Key Differentiator**: **HTAP Pattern** — separates transactional writes (Cloud Spanner) from analytical reads (BigQuery). Spanner provides globally distributed ACID transactions; BigQuery provides serverless analytics.

**Industry Insight**: Spotify and Snapchat use similar GCP architectures. **Spotify's Scio** (their Scala Spark library) runs on Dataproc for daily music listening analytics feeding their recommendation algorithms.

**Related**: [[Cloud Platforms - AWS vs GCP vs Azure]], [[Data Lake vs Data Warehouse vs Lakehouse]]

---

# Project 21 - Azure IoT Data Pipeline

#data-engineering #advanced #azure #azure-data-factory #cosmos-db #iot

**Core Stack**: IoT Hub → Azure Data Factory → Azure Stream Analytics → Azure Cosmos DB → Power BI

**Key Differentiator**: **Full Microsoft Azure stack** — Azure Data Factory is the managed ETL orchestration service (equivalent to AWS Glue + Airflow). Azure Stream Analytics is a fully managed stream processing service using SQL-like syntax.

**Industry Insight**: BMW, Volkswagen, and most European manufacturing companies use Azure IoT Hub for connected vehicle and factory floor telemetry. Azure's GDPR compliance features make it dominant in European enterprise.

**Azure Stream Analytics Query**:
```sql
-- Real-time anomaly detection in Azure Stream Analytics
SELECT 
    device_id,
    AVG(temperature) AS avg_temp,
    MAX(temperature) AS max_temp,
    System.Timestamp() AS window_end
INTO [anomaly-output]
FROM [iot-hub-input] TIMESTAMP BY event_time
GROUP BY device_id, TumblingWindow(minute, 5)
HAVING AVG(temperature) > 80
```

**Related**: [[Project 08 - IoT Sensor Data Pipeline]], [[Project 12 - IoT Data Lake & Warehouse]]

---

# Project 22 - GCS to AWS Redshift Migration

#data-engineering #advanced #cloud-migration #aws-emr #redshift #datasync

**Core Stack**: Google Cloud Storage → AWS DataSync → Amazon S3 → AWS EMR (Spark) → Amazon Redshift

**Key Differentiator**: **Cross-cloud migration** — a very common enterprise scenario when companies switch cloud providers or adopt a multi-cloud strategy. AWS DataSync handles the secure, efficient transfer of petabytes from GCS to S3.

**Industry Insight**: Companies like Twitter/X have undergone massive cloud migrations. The typical pattern is **strangler fig migration** — build the new pipeline in parallel, validate data parity, then cut over.

**Migration Best Practice**:
```
Phase 1: Historical data transfer (DataSync, one-time bulk)
Phase 2: Delta sync (DataSync incremental)
Phase 3: Parallel run (both systems active, compare outputs)
Phase 4: Cutover (switch traffic to AWS)
Phase 5: GCS decommission
```

**Related**: [[Project 23 - Google Cloud Database Migration]], [[Cloud Platforms - AWS vs GCP vs Azure]]

---

# Project 23 - Google Cloud Database Migration

#data-engineering #advanced #gcp #google-dms #cloud-sql #bigtable #migration

**Core Stack**: On-premises MySQL/PostgreSQL → Google DMS → Cloud SQL + Bigtable → Dataproc → Looker Studio

**Key Differentiator**: **Database Migration Service (DMS)** enables near-zero-downtime migration using Change Data Capture (CDC) — reads the database transaction log to replicate changes in real-time while the original system stays live.

**Industry Insight**: Google DMS competes with AWS DMS and Azure Database Migration Service. All use CDC under the hood. **Debezium** (open-source) is the widely adopted CDC tool for custom migrations.

**CDC Concept**:
```
Before DMS: Application writes → Old MySQL
During Migration: Application writes → Old MySQL → DMS (CDC reads binlog) → Cloud SQL
After Cutover: Application writes → Cloud SQL
```

**Related**: [[Project 22 - GCS to AWS Redshift Migration]], [[ETL vs ELT]]

---

# Project 24 - AWS Glue ETL + Airflow Orchestration

#data-engineering #advanced #aws-glue #airflow #rds #s3 #managed-etl

**Core Stack**: JSON/CSV data → Amazon S3 → AWS Glue (serverless Spark ETL) → Amazon RDS → Apache Airflow

**Key Differentiator**: **Managed ETL with AWS Glue** — Glue runs PySpark without managing clusters. The Glue Data Catalog auto-discovers schemas. Airflow orchestrates the Glue jobs with dependency management.

**Industry Insight**: AWS Glue is used by Expedia, Intuit, and thousands of AWS customers for serverless ETL. The main trade-off vs self-managed EMR: less control over Spark configuration, but zero cluster management overhead.

**Airflow + Glue Integration**:
```python
from airflow.providers.amazon.aws.operators.glue import GlueJobOperator

glue_etl = GlueJobOperator(
    task_id='run_glue_etl',
    job_name='my-etl-job',
    script_location='s3://bucket/scripts/etl.py',
    s3_bucket='my-bucket',
    iam_role_name='GlueETLRole',
    create_job_kwargs={'GlueVersion': '4.0', 'NumberOfWorkers': 10, 'WorkerType': 'G.1X'},
    aws_conn_id='aws_default',
)
```

**Related**: [[Project 03 - Retail POS ETL Pipeline]], [[Orchestration with Apache Airflow]]

---

# Project 25 - Azure Event Hub + Synapse Pipeline

#data-engineering #advanced #azure #event-hubs #synapse #azure-batch #lakehouse

**Core Stack**: IoT/App logs → Azure Event Hubs → Azure Batch → ADLS (Parquet/ORC) → Azure Synapse Serverless SQL → Power BI

**Key Differentiator**: **Azure Synapse Serverless SQL** queries data directly on Azure Data Lake Storage (ADLS) without loading into a database — similar to AWS Athena. Azure Batch handles large-scale parallel processing jobs economically.

**Industry Insight**: Microsoft uses this exact stack internally for Xbox Live telemetry, Teams usage analytics, and Azure service monitoring. Azure Synapse Serverless SQL costs **$5 per TB scanned** — same as AWS Athena, enabling cost-effective ad-hoc analytics.

**Synapse Serverless SQL**:
```sql
-- Query ADLS Parquet directly (no data loading needed!)
SELECT 
    event_type,
    COUNT(*) as event_count,
    AVG(duration_ms) as avg_duration
FROM OPENROWSET(
    BULK 'https://mydatalake.dfs.core.windows.net/events/**',
    FORMAT='PARQUET'
) AS events
WHERE YEAR(event_timestamp) = 2024
GROUP BY event_type
ORDER BY event_count DESC;
```

**Related**: [[Project 21 - Azure IoT Data Pipeline]], [[Project 14 - E-Commerce Spark + Athena Pipeline]], [[Cloud Platforms - AWS vs GCP vs Azure]]

---

*Tags: #data-engineering #advanced #aws #gcp #azure #cloud-migration #serverless #event-driven*
