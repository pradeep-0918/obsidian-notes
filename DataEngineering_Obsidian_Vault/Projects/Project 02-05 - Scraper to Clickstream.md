# Project 02 - E-Commerce Scraper Pipeline

#data-engineering #ETL #beginner #web-scraping #bigquery #e-commerce

---

## 📌 Overview

A data pipeline that scrapes product information from e-commerce websites, cleans and deduplicates the data, stores it in a cloud warehouse, and enables pricing trend analysis. This pattern is foundational for competitive intelligence, price monitoring, and consumer trend platforms.

---

## 🎯 Scope

- **Data Ingestion**: Scrape and collect e-commerce product data (prices, availability, ratings)
- **Processing**: Clean and standardize product details
- **Storage**: Store structured data in a cloud warehouse (BigQuery)
- **Visualization**: Analyze pricing trends with Looker/Superset

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | BeautifulSoup, Scrapy | Web scraping |
| Processing | Python (Pandas, PySpark) | Data cleaning |
| Storage | AWS S3 / BigQuery | Data lake + warehouse |
| Visualization | Looker / Superset | Trend dashboards |

---

## 🔄 Data Pipeline Architecture

```
[Target Websites]
    │
    ▼
[Scraping Layer]
    Scrapy Spider / BeautifulSoup
    - Rotating proxies & User-Agent headers
    - Rate limiting & backoff
    - sitemap.xml parsing
         │
         ▼
[Raw Storage]
    AWS S3 / GCS (JSON/CSV format)
         │
         ▼
[Processing Layer]
    PySpark / Pandas
    - Deduplication (fuzzy matching)
    - Price normalization (currency conversion)
    - Category standardization
         │
         ▼
[Warehouse]
    Google BigQuery
    - Partitioned by scrape_date
    - Clustered by product_category
         │
         ▼
[Analytics]
    Looker / Superset
    - Price comparison dashboards
    - Trend visualizations
```

---

## 📊 Use Cases

- **Competitive Pricing Analysis**: Compare your prices vs competitors across marketplaces
- **Stock Availability Monitoring**: Track inventory fluctuations in real time
- **Consumer Trends**: Identify trending products by scraping review volumes and ratings

---

## ⚠️ Challenges

1. **Web Scraping Limitations**: Websites block bot traffic using rate limits, CAPTCHAs, JavaScript rendering
2. **Data Duplication**: Same product listed multiple times with slightly different names
3. **Handling Large Data**: Millions of records slow down queries without proper indexing
4. **Legal & Ethical Issues**: Terms of Service violations, robots.txt compliance
5. **Dynamic JavaScript Pages**: Single-page apps (React/Angular) require browser simulation

---

## ✅ Solutions

- **Use rotating proxies & headers** to bypass scraping blocks (ScraperAPI, Bright Data)
- **Deduplicate records using fuzzy matching** (fuzzywuzzy / RapidFuzz libraries)
- **Use Spark SQL for efficient querying** with partitioning and clustering
- **Use Selenium or Playwright** for JavaScript-heavy pages
- **Always respect robots.txt** and implement polite crawl delays

---

## 🧠 Key Learnings

- Understanding **HTML parsing** with CSS selectors and XPath
- How **Scrapy's middleware system** works for proxy rotation and retry logic
- Why **fuzzy deduplication** is necessary (same product = "iPhone 15 Pro" vs "Apple iPhone 15 Pro 256GB")
- The difference between **structured scraping** (clear HTML tables) vs **unstructured scraping** (ad-hoc product pages)

---

## 🏢 Industry Insights

### How Rakuten & Price Comparison Sites Work
Rakuten and similar price comparison engines run scraping pipelines every 15–30 minutes across thousands of product pages. They use **distributed Scrapy clusters** with Scrapyd or Scrapy Cloud, managing thousands of concurrent spiders.

### How Amazon Monitors Competitors
Amazon uses a combination of scraping and **data partnerships** to track competitor pricing. Their internal pricing engine re-evaluates millions of product prices multiple times per day based on competitor data.

### How DataForSEO & SimilarWeb Scale Scraping
These companies use Kubernetes-managed scraper fleets with **auto-scaling based on queue depth**. Each scraper is a stateless container that pulls URLs from a Redis queue.

### Production Best Practices
- Store raw HTML in S3 before parsing — allows re-parsing without re-scraping
- Use **content hashing** to detect if a page actually changed before re-processing
- Implement **incremental scraping** (only scrape changed pages)
- Monitor scraping success rates — a sudden drop signals the site changed its structure

---

## 🚀 Optimization Techniques

- **Scrapy-Redis**: Distributed scraping with shared Redis queue across multiple Scrapy instances
- **Bloom Filters**: Use probabilistic data structures to avoid re-visiting already-scraped URLs
- **BigQuery Partitioning**: Partition by `scrape_date` to limit query scan costs
- **BigQuery Clustering**: Cluster by `product_category` + `brand` for faster analytical queries
- **Async scraping**: Use `asyncio` + `aiohttp` for non-Scrapy scenarios (5–10x faster than sync)

---

## 🔍 Deep Dive

### Step-by-Step Architecture Explanation

**Step 1 — Spider Design**
```python
import scrapy
class ProductSpider(scrapy.Spider):
    name = "products"
    custom_settings = {
        'DOWNLOAD_DELAY': 1.5,
        'ROTATING_PROXY_LIST': ['proxy1:port', 'proxy2:port'],
        'AUTOTHROTTLE_ENABLED': True,
    }
    def parse(self, response):
        for product in response.css('div.product-card'):
            yield {
                'name': product.css('h2.product-name::text').get(),
                'price': product.css('span.price::text').get(),
                'url': response.url,
                'scraped_at': datetime.utcnow().isoformat(),
            }
```

**Step 2 — Fuzzy Deduplication**
```python
from rapidfuzz import fuzz, process
def deduplicate_products(df):
    seen = []
    duplicates = []
    for idx, name in enumerate(df['product_name']):
        match = process.extractOne(name, seen, scorer=fuzz.token_sort_ratio)
        if match and match[1] > 85:
            duplicates.append(idx)
        else:
            seen.append(name)
    return df.drop(index=duplicates)
```

**Step 3 — BigQuery Load**
```python
from google.cloud import bigquery
client = bigquery.Client()
job_config = bigquery.LoadJobConfig(
    write_disposition="WRITE_APPEND",
    time_partitioning=bigquery.TimePartitioning(field="scrape_date"),
    clustering_fields=["product_category", "brand"],
)
client.load_table_from_dataframe(df_clean, "project.dataset.products", job_config=job_config)
```

---

## 💡 Tips & Tricks

- **Performance**: Run Scrapy with `CONCURRENT_REQUESTS=32` for speed, but respect rate limits
- **Debugging**: Use Scrapy shell (`scrapy shell URL`) to test CSS selectors interactively
- **Cost**: BigQuery charges per query scan — always use partition filters in WHERE clauses
- **Scaling**: When >100 sites, switch from single Scrapy process to distributed Scrapy-Redis cluster

---

## 🔗 Related Projects

- [[Project 01 - CSV Data Pipeline]] — Simpler flat-file version of this pattern
- [[Project 06 - Public API Data Pipeline]] — Cleaner data ingestion via APIs vs scraping
- [[ETL vs ELT]] — This project uses ELT (raw data to BigQuery, transform in SQL)
- [[Big Data Tools Overview]] — Spark and BigQuery deep dive

---

*Tags: #data-engineering #ETL #beginner #web-scraping #bigquery #scrapy #pandas #e-commerce*

---
---

# Project 03 - Retail POS ETL Pipeline

#data-engineering #ETL #beginner #spark #hive #airflow #retail

---

## 📌 Overview

A daily batch ETL pipeline that ingests transactional data from a retail store's Point-of-Sale (POS) system, processes it with Apache Spark and Hive, and automates the workflow with Apache Airflow. This is one of the most common real-world data engineering patterns in retail, CPG, and supply chain industries.

---

## 🎯 Scope

- **Data Ingestion**: Ingest transactional data from Retail POS (MySQL/PostgreSQL/CSV Exports)
- **Processing**: Batch ETL — clean, transform, store data for reporting
- **Storage**: Apache Hive (ORC Format) for historical analysis
- **Orchestration**: Automate daily processing with Apache Airflow / AWS Step Functions
- **Query & Reporting**: Presto/Trino/AWS Athena for SQL-based revenue insights

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | MySQL / PostgreSQL / CSV Exports | POS transaction data |
| Processing | Apache Spark (Batch), Pandas | Data cleaning & enrichment |
| Storage | Apache Hive (ORC) | Historical analytics store |
| Orchestration | Apache Airflow / AWS Step Functions | Workflow automation |
| Querying | Presto / Trino / AWS Athena | Business SQL queries |

---

## 🔄 Data Pipeline Architecture

```
[POS System]
MySQL / PostgreSQL / CSV Export
    │
    ▼ (daily at 2 AM)
[Airflow DAG triggers]
    │
    ├──▶ Task 1: Extract → Pull data from MySQL → S3 Raw
    ├──▶ Task 2: Validate → Schema checks, null validation
    ├──▶ Task 3: Transform → Spark job (clean, enrich, deduplicate)
    ├──▶ Task 4: Load → Write to Hive (ORC, partitioned by date)
    └──▶ Task 5: Notify → Send success/failure email/Slack alert
         │
         ▼
[Hive Data Warehouse]
    Partitioned by: year/month/day
    Format: ORC (Optimized Row Columnar)
         │
         ▼
[Presto / AWS Athena]
    Business analyst SQL queries
         │
         ▼
[BI Dashboard]
    Revenue, inventory, customer insights
```

---

## 📊 Use Cases

- **Daily revenue tracking** for retail managers and C-suite
- **Inventory monitoring** to optimize stock levels and prevent stockouts
- **Customer purchase pattern analysis** for targeted marketing

---

## ⚠️ Challenges

1. **Handling inconsistent POS exports**: Different store locations export in slightly different formats
2. **Optimizing query performance**: Large Hive tables without partitioning are extremely slow
3. **Ensuring automated execution**: Jobs that fail silently are a major operational risk
4. **Late-arriving data**: Some transactions arrive hours after business close
5. **Schema evolution**: POS software upgrades add/remove columns without warning

---

## ✅ Solutions

- **Implemented automated data validation checks** at ingestion (Great Expectations / custom validators)
- **Used ORC format and partitioning in Hive** for 10x query speedup
- **Deployed Airflow DAGs with monitoring alerts** (email, Slack, PagerDuty)
- **Used date-based upsert logic** to handle late-arriving records
- **Partitioned data on date columns** for faster query performance on sales data
- **Scheduled incremental data loads** to reduce processing time and storage costs
- **Implemented S3 lifecycle policies** to archive old data

---

## 🧠 Key Learnings

- How **Apache Airflow DAGs** work — tasks, dependencies, retries, SLAs
- Why **ORC format** outperforms CSV/JSON by 5–10x (compression + predicate pushdown)
- The difference between **full load** vs **incremental load** strategies
- How **partitioning on date columns** reduces Hive query time from hours to minutes
- Why **idempotent DAGs** are critical — if Airflow re-runs a failed day, data should not duplicate

---

## 🏢 Industry Insights

### How Walmart Uses Batch ETL
Walmart processes **2.5 petabytes of data daily** from 10,000+ stores. Their Hadoop cluster (one of the world's largest) runs Spark batch jobs every hour. They use Apache Hive as the metadata layer and Presto for ad-hoc queries by analysts.

### How Target's Data Pipeline Works
Target uses Apache Airflow to orchestrate 1,000+ DAGs. Each DAG corresponds to a specific data domain (inventory, loyalty, pricing). They've built a custom Airflow plugin for data quality gate enforcement between pipeline steps.

### How McDonald's (Digital Order Data) Handles ETL
McDonald's mobile order data flows through batch ETL every 15 minutes — not truly real-time. They use AWS Step Functions for orchestration because it natively integrates with other AWS services (Lambda, Glue, EMR).

### Production Best Practices
- Use **Airflow's `ExternalTaskSensor`** to wait for upstream DAGs before running downstream
- Implement **Data SLAs** in Airflow — alert if data isn't ready by 6 AM for morning reports
- Use **backfill capability** to reprocess historical data when logic changes
- Store Airflow DAG code in **Git** with CI/CD pipeline for deployment

---

## 🚀 Optimization Techniques

- **Hive Bucketing**: Combine partitioning with bucketing on `store_id` for even faster joins
- **Spark Dynamic Allocation**: Automatically adjust Spark executors based on workload
- **ORC Bloom Filters**: Add bloom filters to frequently filtered columns for predicate pushdown
- **Incremental Processing**: Only process new records (using `WHERE ingestion_date = yesterday`)
- **Spark AQE**: Enable Adaptive Query Execution for automatic join strategy optimization

---

## 🔍 Deep Dive

### Airflow DAG Example
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.apache.spark.operators.spark_submit import SparkSubmitOperator
from datetime import datetime, timedelta

default_args = {
    'owner': 'data-team',
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
    'email_on_failure': True,
    'email': ['data-alerts@company.com'],
    'sla': timedelta(hours=4),
}

with DAG('retail_pos_etl',
         default_args=default_args,
         schedule_interval='0 2 * * *',  # Daily at 2 AM
         start_date=datetime(2024, 1, 1),
         catchup=False) as dag:

    extract = PythonOperator(task_id='extract_pos_data', python_callable=extract_from_mysql)
    validate = PythonOperator(task_id='validate_schema', python_callable=run_great_expectations)
    transform = SparkSubmitOperator(task_id='spark_transform', application='jobs/retail_etl.py')
    load = PythonOperator(task_id='load_to_hive', python_callable=load_hive_partition)
    notify = PythonOperator(task_id='send_success_alert', python_callable=send_slack_notification)

    extract >> validate >> transform >> load >> notify
```

---

## 💡 Tips & Tricks

- Always set `catchup=False` in Airflow unless you specifically need historical backfills
- Use Airflow Variables/Connections for credentials — never hardcode in DAG files
- For debugging failed Spark jobs, check the **Spark History Server** for stage-level metrics
- Use `--conf spark.sql.adaptive.enabled=true` for all Spark 3.x jobs

---

## 🔗 Related Projects

- [[Project 01 - CSV Data Pipeline]] — Simpler version without orchestration
- [[Project 13 - Hadoop Batch ETL with Airflow]] — Advanced Hadoop/Hive version
- [[Project 24 - AWS Glue ETL + Airflow Orchestration]] — Managed ETL with AWS Glue
- [[Orchestration with Apache Airflow]] — Deep dive on Airflow concepts
- [[Streaming vs Batch Processing]] — Why this project uses batch

---

*Tags: #data-engineering #ETL #beginner #spark #hive #airflow #retail #batch-processing #orchestration*

---
---

# Project 04 - Web Server Log Analytics

#data-engineering #ETL #beginner #log-analytics #pyspark #kibana #security

---

## 📌 Overview

A log ingestion and analysis pipeline that collects Apache/Nginx web server logs, parses unstructured log data using regex, stores it efficiently, and generates traffic and security dashboards. Log analytics is one of the most critical skills in data engineering — nearly every company has servers generating logs.

---

## 🎯 Scope

- **Data Ingestion**: Collect log files from web servers (Apache, Nginx)
- **Processing**: Parse and clean unstructured log data using Regex
- **Storage**: Store logs efficiently for querying and analysis (HDFS/S3)
- **Visualization**: Generate reports on website traffic trends using Kibana/Grafana

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | Apache Web Server Logs, Nginx Logs | Raw log files |
| Processing | Python (Regex), PySpark | Log parsing & cleaning |
| Storage | AWS S3 / Google Cloud Storage / HDFS | Scalable log storage |
| Visualization | Kibana / Grafana | Traffic & security dashboards |

---

## 🔄 Data Pipeline Architecture

```
[Web Servers]
Apache / Nginx generating logs continuously
    │
    ▼ (batch: every 15 min OR stream: Filebeat/Fluentd)
[Log Collection]
    Filebeat → ships logs to Logstash / Kafka
         │
         ▼
[Parsing Layer]
    PySpark with Regex
    - Extract: IP, timestamp, method, URL, status_code, bytes, user_agent
    - Enrich: GeoIP lookup for visitor location
    - Classify: Error vs Success vs Redirect
         │
         ▼
[Storage]
    AWS S3 / HDFS (Parquet format)
    Partitioned by: date + log_type
         │
         ├──▶ Kibana/Elasticsearch (hot data — last 30 days)
         └──▶ S3/HDFS (cold data — archival)
              │
              ▼
[Dashboards]
    Grafana / Kibana
    - Traffic volume, error rates, top URLs, geographic distribution
```

---

## 📊 Use Cases

- **Website Traffic Analysis**: Page views, unique visitors, bounce rate by time
- **Error Log Monitoring**: Track 4xx/5xx errors and identify broken endpoints
- **Security Audit & Intrusion Detection**: Detect brute force, SQL injection patterns, DDoS

---

## ⚠️ Challenges

1. **Unstructured Log Data**: Logs are free-text strings requiring complex Regex parsing
2. **Large Log File Size**: High-traffic servers generate gigabytes of logs per hour
3. **Real-time Log Monitoring**: Batch processing introduces delay for security use cases
4. **Log Rotation**: Server log rotation can cause files to be overwritten before ingestion
5. **Multi-Format Logs**: Different applications write logs in different formats (custom, JSON, CLF)

---

## ✅ Solutions

- **Use Regex for parsing logs** into structured format (Apache Combined Log Format is standardized)
- **Process efficiently using PySpark** for large volumes
- **Use streaming frameworks** (Kafka + Flink/Spark Streaming) for real-time log analysis
- **Automate log file ingestion pipelines** to handle large volumes efficiently
- **Use schema-on-read approaches** in Apache Spark for flexibility
- **Integrate real-time alerting** for detecting security issues from logs

---

## 🧠 Key Learnings

- Mastering **Regex patterns** for Apache Combined Log Format
- Understanding the **ELK Stack** (Elasticsearch, Logstash, Kibana) architecture
- Why **log sampling** is used at high scale (not every log line needs analysis)
- The difference between **log aggregation** (count errors) vs **log indexing** (search any field)

---

## 🏢 Industry Insights

### How Netflix Handles Log Analytics
Netflix processes **billions of log events daily** using Apache Kafka as the central nervous system. Logs flow from services → Kafka → Flink (real-time processing) → Elasticsearch (search) + S3 (archival). Their observability platform is called **Edgar**.

### How Cloudflare Analyzes Logs
Cloudflare processes **55 million HTTP requests per second**. They use ClickHouse (a columnar OLAP database) for real-time log analytics at extreme scale. This is far faster than traditional Elasticsearch for time-series log data.

### How LinkedIn Uses Log Pipelines
LinkedIn built **Kafka** specifically to handle their log aggregation problem. Before Kafka, they were losing log data under high load. Kafka's durability guarantees solved this — it's now the industry standard for log streaming.

### Production Best Practices
- Use **Fluentd or Filebeat** (not manual scripts) for reliable log shipping
- Always preserve the **raw log** in cold storage before parsing
- Implement **log sampling** (e.g., log only 10% of successful requests, 100% of errors)
- Add **structured logging** to your applications (JSON logs are far easier to parse than plain text)

---

## 🚀 Optimization Techniques

- **Apache Regex UDF in Spark**: Pre-compile Regex patterns for performance
- **Log Partitioning**: Partition by `date` + `status_code_family` (2xx, 4xx, 5xx)
- **Elasticsearch ILM Policies**: Automatically move logs from hot → warm → cold → delete
- **GeoIP Enrichment**: Use MaxMind GeoLite2 database for IP-to-location mapping
- **Log Compression**: Use Gzip/LZ4 compression for cold storage (80% space savings)

---

## 🔍 Deep Dive

### Regex Log Parsing in PySpark
```python
from pyspark.sql.functions import regexp_extract, col

LOG_PATTERN = r'^(\S+) \S+ \S+ \[([\w:/]+\s[+\-]\d{4})\] "(\S+)\s?(\S+)?\s?(\S+)?" (\d{3}) (\d+)'

df = spark.read.text("s3://logs-bucket/apache/*.log")
df_parsed = df.select(
    regexp_extract('value', LOG_PATTERN, 1).alias('ip'),
    regexp_extract('value', LOG_PATTERN, 2).alias('timestamp'),
    regexp_extract('value', LOG_PATTERN, 3).alias('method'),
    regexp_extract('value', LOG_PATTERN, 4).alias('url'),
    regexp_extract('value', LOG_PATTERN, 6).cast('int').alias('status_code'),
    regexp_extract('value', LOG_PATTERN, 7).cast('long').alias('bytes'),
)
df_parsed = df_parsed.filter(col('ip') != '')  # Filter malformed lines
```

---

## 💡 Tips & Tricks

- Test your Regex in Python's `re` module before deploying to Spark
- Use **Kibana's Grok Debugger** to visually build Regex patterns for log parsing
- For security use cases, use **SIEM tools** (Splunk, Elastic SIEM) built on top of log pipelines
- Add a **`log_source`** column during ingestion to track which server generated the log

---

## 🔗 Related Projects

- [[Project 05 - Clickstream Pipeline]] — Similar event-based pipeline but for user behavior
- [[Project 11 - Real-Time Clickstream Analytics]] — Real-time version of log analytics
- [[Streaming vs Batch Processing]] — When to go real-time for log monitoring
- [[Big Data Tools Overview]] — Kafka, Spark Streaming details

---

*Tags: #data-engineering #ETL #beginner #log-analytics #pyspark #kibana #grafana #security #regex*

---
---

# Project 05 - Clickstream Pipeline

#data-engineering #ETL #beginner #kafka #flink #delta-lake #user-analytics

---

## 📌 Overview

A high-volume clickstream data pipeline that collects user interaction events from websites, processes them with Apache Flink/Spark Streaming, stores them in a Delta Lake architecture, and enables real-time user behavior and engagement analytics. Clickstream pipelines are the foundation of recommendation systems, A/B testing, and personalization at every major tech company.

---

## 🎯 Scope

- **Data Ingestion**: Collect clickstream data from websites (page views, clicks, scroll events)
- **Processing**: Parse, clean, and transform event logs using stream processing
- **Storage**: Store structured data in Delta Lake / Snowflake for analytics
- **Visualization**: Track user behavior and engagement in Looker / Redash

---

## ⚙️ Tech Stack

| Layer | Tools | Purpose |
|-------|-------|---------|
| Data Source | Web Server Logs, Google Analytics Events, Firebase | User event streams |
| Processing | Apache Flink / Spark Streaming | Real-time event processing |
| Storage | AWS S3 / Delta Lake / Snowflake | Layered data storage |
| Visualization | Looker / Redash | Engagement dashboards |

---

## 🔄 Data Pipeline Architecture

```
[Browser / Mobile App]
JavaScript event tracking (clicks, scrolls, page views)
    │
    ▼ (HTTP POST to event collector)
[Event Collector]
    Custom endpoint / Segment / Snowplow
         │
         ▼
[Apache Kafka]
    Topic: user-events
    Partitioned by: user_id hash
    Retention: 7 days
         │
         ▼
[Stream Processor]
    Apache Flink / Spark Streaming
    - Session stitching (30-min idle = new session)
    - Bot/crawler filtering
    - Event deduplication (exactly-once semantics)
    - Schema validation
         │
         ├──▶ [Delta Lake (S3)]
         │        Bronze → Silver → Gold layers
         │
         └──▶ [Snowflake / BigQuery]
                  Aggregated metrics tables
                       │
                       ▼
              [Looker / Redash]
              - Funnel analysis
              - User journey visualization
              - Conversion dashboards
```

---

## 📊 Use Cases

- **User Journey Analysis**: Understand the path users take from landing to conversion
- **Ad Click & Conversion Tracking**: Measure ROI of marketing campaigns
- **Real-time Recommendation Systems**: Feed user behavior data to ML models for personalization

---

## ⚠️ Challenges

1. **High Volume of Events**: Even medium-scale sites generate millions of events/hour
2. **Schema Evolution**: New event types are added by product teams without warning
3. **Data Latency**: Business teams expect near-real-time metrics
4. **Bot Traffic**: 30–40% of web traffic can be non-human (bots, scrapers)
5. **Session Attribution**: Correctly linking events across devices and sessions

---

## ✅ Solutions

- **Use Kafka + Flink for real-time ingestion** — handles millions of events/second
- **Implement Delta Lake for schema versioning** — handles evolving schemas gracefully
- **Optimize partitioning & caching strategies** to reduce latency
- **Use columnar storage formats (Parquet)** to optimize query performance
- **Leverage sessionization techniques** for better user journey insights
- **Implement data deduplication** to avoid duplicate events inflating metrics

---

## 🧠 Key Learnings

- Understanding **exactly-once semantics** in Kafka (idempotent producers + transactional consumers)
- How **Delta Lake's ACID transactions** solve the classic streaming/batch consistency problem
- The **Bronze/Silver/Gold medallion architecture** for progressive data refinement
- Why **sessionization** is complex (users close browser, come back later — same session or new?)

---

## 🏢 Industry Insights

### How Netflix Handles Clickstream
Netflix processes **500 billion events per day** from their streaming platform. They use Apache Kafka for ingestion and Apache Flink for real-time processing. The data feeds their recommendation engine, which drives 80% of content watched on the platform.

### How Uber's Event Platform Works
Uber built **uSignal**, a massive event processing platform that handles driver location updates, ride requests, and app interactions. They use Kafka + Flink with exactly-once guarantees, and store data in Apache Hudi (similar to Delta Lake) on HDFS.

### How Airbnb Processes Clickstream
Airbnb uses **Snowflake** as their primary analytics warehouse for clickstream data. They built a sessionization library that correctly handles multi-device users — crucial for understanding the host/guest journey.

### How LinkedIn Measures A/B Tests
LinkedIn's experimentation platform (XLNT) relies on clickstream data processed through Kafka → Spark Streaming → Pinot (real-time OLAP). This allows them to measure A/B test results within minutes of an experiment starting.

### Production Best Practices
- Use **Kafka Schema Registry** (Confluent) to enforce Avro/Protobuf schemas on event streams
- Implement **event versioning** (`event_version: 2`) so old and new formats can coexist
- Add **server-side timestamp** alongside client timestamp — clocks drift on user devices
- Use **watermarking** in Flink/Spark Streaming to handle late-arriving events

---

## 🚀 Optimization Techniques

- **Kafka Partitioning by User ID**: Ensures all events for a user go to same partition (ordered)
- **Flink Checkpointing**: Enable checkpointing every 60s for fault tolerance
- **Delta Lake Z-Ordering**: Use Z-ORDER BY user_id, event_date for co-located query performance
- **Micro-batch Optimization**: Use Spark Structured Streaming trigger intervals wisely (1-min micro-batch vs continuous)
- **Columnar Pushdown**: Use Parquet column pruning to only read needed columns

---

## 🔍 Deep Dive

### Delta Lake Medallion Architecture
```
Bronze Layer (Raw)     → Exact copy of incoming events, immutable
Silver Layer (Cleaned) → Deduplicated, validated, enriched events
Gold Layer (Aggregated)→ Business-ready metrics (DAU, conversion rates)
```

```python
# Flink → Delta Lake Write (Spark)
from delta import DeltaTable
from pyspark.sql.functions import col, window, count

# Silver Layer: Deduplicate events
df_silver = df_bronze \
    .dropDuplicates(['event_id']) \
    .filter(col('user_id').isNotNull()) \
    .filter(~col('user_agent').rlike('bot|crawler|spider'))

# Gold Layer: Hourly session metrics
df_gold = df_silver.groupBy(
    window(col('event_timestamp'), '1 hour'),
    col('page_url')
).agg(count('event_id').alias('page_views'))

df_gold.write.format('delta').mode('append').save('s3://datalake/gold/page_metrics/')
```

---

## 💡 Tips & Tricks

- Use **`OPTIMIZE` and `VACUUM`** commands on Delta tables regularly to maintain performance
- Add **data quality metrics** to each pipeline layer (null %, duplicate %) and alert on anomalies
- For very high volume, consider **Apache Pinot** or **Apache Druid** for sub-second OLAP queries
- Cache your **Kafka consumer offsets** in external storage (ZooKeeper/Kafka itself) for resilient processing

---

## 🔗 Related Projects

- [[Project 04 - Web Server Log Analytics]] — Similar but for server logs
- [[Project 11 - Real-Time Clickstream Analytics]] — Advanced version with Kinesis
- [[Project 15 - Kafka + Spark Streaming + Cassandra]] — Real-time NoSQL storage pattern
- [[Streaming vs Batch Processing]] — Why streaming is used here
- [[Advanced & Underrated Tools]] — Apache Iceberg as Delta Lake alternative

---

*Tags: #data-engineering #ETL #beginner #kafka #flink #spark-streaming #delta-lake #clickstream #user-analytics #real-time*
