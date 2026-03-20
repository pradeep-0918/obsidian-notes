# Data Engineer Career Roadmap

#career #roadmap #data-engineering #skills #interview #resume

---

## 🎯 Overview

This roadmap takes you from zero to job-ready Data Engineer, covering the exact skills, projects, and mindset required to land roles at top companies. Based on analysis of 500+ data engineering job descriptions and real interview experiences at FAANG, unicorn startups, and enterprise companies.

---

## 📊 Skills Required: Beginner → Advanced

### 🟢 Tier 1 — Foundation (Must Master First)

#### SQL — Advanced Level
SQL is the #1 skill tested in every data engineering interview. Being "okay" at SQL is not enough.

**What you must know**:
```sql
-- 1. Window Functions (every interview has these)
SELECT
    employee_id,
    department,
    salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dept_rank,
    LAG(salary, 1) OVER (PARTITION BY department ORDER BY hire_date) AS prev_salary,
    SUM(salary) OVER (PARTITION BY department) AS dept_total,
    AVG(salary) OVER (ORDER BY hire_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS rolling_avg
FROM employees;

-- 2. CTEs (Common Table Expressions)
WITH high_value_customers AS (
    SELECT customer_id, SUM(amount) as total_spend
    FROM orders
    GROUP BY customer_id
    HAVING SUM(amount) > 10000
),
customer_details AS (
    SELECT c.*, h.total_spend
    FROM customers c
    JOIN high_value_customers h ON c.id = h.customer_id
)
SELECT * FROM customer_details ORDER BY total_spend DESC;

-- 3. Query Optimization
-- Bad: Full table scan
SELECT * FROM orders WHERE DATE(created_at) = '2024-12-01';

-- Good: Partition pruning (uses index on created_at)
SELECT * FROM orders
WHERE created_at >= '2024-12-01' AND created_at < '2024-12-02';

-- 4. Slowly Changing Dimensions (SCD Type 2)
-- Critical for data warehousing interviews
INSERT INTO dim_customer
SELECT c.customer_id, c.name, c.address,
       CURRENT_DATE as valid_from,
       '9999-12-31' as valid_to,
       TRUE as is_current
FROM staging_customer c
LEFT JOIN dim_customer d ON c.customer_id = d.customer_id AND d.is_current = TRUE
WHERE d.customer_id IS NULL  -- New customers
   OR c.address != d.address; -- Changed address
```

**Study Resources**: Mode Analytics SQL Tutorial, LeetCode SQL 50, Hackerrank SQL

---

#### Python — Data Engineering Focused
```python
# Must-know patterns:

# 1. Context Managers for resource management
from contextlib import contextmanager
import psycopg2

@contextmanager
def get_db_connection(connection_string):
    conn = psycopg2.connect(connection_string)
    try:
        yield conn
    finally:
        conn.close()

# 2. Generators for memory-efficient processing
def read_large_csv_in_chunks(filepath, chunksize=10000):
    """Process 10GB CSV without loading into memory."""
    import pandas as pd
    for chunk in pd.read_csv(filepath, chunksize=chunksize):
        processed = chunk.dropna().drop_duplicates()
        yield processed

# 3. Dataclasses for pipeline configuration
from dataclasses import dataclass
from typing import Optional

@dataclass
class PipelineConfig:
    source_bucket: str
    destination_bucket: str
    partition_column: str
    batch_size: int = 10000
    max_retries: int = 3
    timeout_seconds: int = 3600

# 4. Decorators for retry logic
import functools, time, logging

def retry(max_attempts=3, delay=1, exceptions=(Exception,)):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    if attempt == max_attempts - 1:
                        raise
                    logging.warning(f"Attempt {attempt+1} failed: {e}. Retrying in {delay}s...")
                    time.sleep(delay * (2 ** attempt))
        return wrapper
    return decorator
```

---

### 🔵 Tier 2 — Core Engineering Skills

#### Apache Spark (PySpark)
**Essential Spark Knowledge for Interviews**:

```python
# 1. Spark Optimization Techniques
from pyspark.sql import SparkSession
from pyspark.sql.functions import broadcast, col

spark = SparkSession.builder \
    .config("spark.sql.adaptive.enabled", "true")  # AQE
    .config("spark.sql.adaptive.coalescePartitions.enabled", "true")
    .getOrCreate()

# Broadcast join (use when one table is small < 10MB)
df_sales = spark.read.parquet("s3://bucket/sales/")
df_products = spark.read.parquet("s3://bucket/products/")  # Small dimension table

df_joined = df_sales.join(broadcast(df_products), "product_id")

# 2. Partition management
# Too few partitions = slow (under-parallelism)
# Too many partitions = overhead (scheduling cost)
df.repartition(200, col("date"))  # Repartition by date column

# 3. Caching strategy
df_frequently_used = df.filter(col("status") == "active").cache()
df_frequently_used.count()  # Trigger cache
# ... use df_frequently_used multiple times ...
df_frequently_used.unpersist()  # Free memory when done

# 4. Reading data efficiently
df = spark.read \
    .option("mergeSchema", "false")  # Faster, no schema inference
    .option("basePath", "s3://bucket/data/")  # For partition discovery
    .parquet("s3://bucket/data/year=2024/month=12/")
```

---

#### Apache Kafka
**Interview Topics**:
- Explain Kafka's partition strategy and why it matters
- What is a consumer group and how does rebalancing work?
- Explain at-least-once vs exactly-once delivery semantics
- How does Kafka achieve fault tolerance?

```python
# Kafka Producer with exactly-once semantics
from confluent_kafka import Producer

producer_config = {
    'bootstrap.servers': 'kafka:9092',
    'enable.idempotence': True,       # Idempotent producer (no duplicates)
    'acks': 'all',                     # Wait for all replicas
    'max.in.flight.requests.per.connection': 5,
    'retries': 2147483647,
}
```

---

#### Cloud — AWS Focus (Most In-Demand)
**Services every data engineer must know**:

| Service | What to Know |
|---------|-------------|
| **S3** | Bucket policies, lifecycle policies, versioning, requester-pays |
| **IAM** | Roles vs Users vs Policies, least privilege principle |
| **Glue** | Crawlers, Data Catalog, ETL jobs, connections |
| **Athena** | Query optimization, workgroups, query result reuse |
| **EMR** | Instance types, auto-scaling, spot instances |
| **Kinesis** | Shards, enhanced fan-out, partition keys |
| **Redshift** | Distribution styles, sort keys, VACUUM/ANALYZE |
| **Lambda** | Triggers, layers, concurrency, timeout limits |

---

#### System Design for Data Engineers
This is where senior vs junior engineers are differentiated.

**Common Interview Questions**:
1. "Design a data pipeline that processes 1 billion events per day"
2. "Design a data warehouse for an e-commerce company"
3. "Design a real-time fraud detection system"
4. "How would you ensure data quality in a production pipeline?"

**Framework for answering**:
```
1. Clarify requirements (scale, latency, cost constraints)
2. Define data sources and sinks
3. Choose processing pattern (batch/streaming/both)
4. Address storage layer (lake/warehouse/lakehouse)
5. Discuss fault tolerance and monitoring
6. Talk about scaling strategy
7. Mention cost optimization
```

---

### 🔴 Tier 3 — Differentiation Skills

- **dbt (Data Build Tool)**: Transform data in warehouses using version-controlled SQL
- **Apache Airflow (Advanced)**: Dynamic DAGs, plugins, custom operators
- **Data Quality**: Great Expectations, Soda Core, dbt tests
- **DataOps**: CI/CD for data pipelines (GitHub Actions + dbt + Airflow)
- **Apache Iceberg / Delta Lake**: Advanced data lake management
- **Cost Optimization**: AWS Cost Explorer, Spot instances, data lifecycle

---

## 🏢 What Companies Expect

### Resume Expectations

**❌ What most beginners write**:
```
• Worked on data pipeline projects using Python and Spark
• Used AWS for cloud storage
• Processed large datasets
```

**✅ What impresses hiring managers**:
```
• Built PySpark ETL pipeline processing 2TB daily from 15 MySQL sources into
  Redshift, reducing report generation time from 6 hours to 20 minutes (95% improvement)

• Designed Apache Kafka + Flink real-time fraud detection system processing
  50,000 transactions/second with <150ms latency, reducing fraud losses by $2M/year

• Architected S3 Data Lake with Apache Iceberg on 200TB dataset, enabling
  time-travel queries and reducing storage costs by 40% via lifecycle policies
```

**Formula**: Action verb + tool/technology + scale/metric + business impact

### Project Expectations

**Beginner role** (0–2 years): 2–3 solid projects showing basic ETL, SQL, Python, one cloud platform

**Mid-level role** (2–5 years): 3–5 projects showing real-time processing, orchestration, data modeling, performance optimization

**Senior role** (5+ years): Evidence of system design thinking, leading complex migrations, cost optimization at scale, data platform architecture

### Interview Expectations

**Round 1 — Coding**: SQL (window functions, CTEs, optimization), Python (data manipulation, algorithm)

**Round 2 — Technical**: Spark/Kafka concepts, pipeline design, debugging scenarios

**Round 3 — System Design**: Design a data platform end-to-end

**Round 4 — Behavioral**: STAR method stories about technical challenges, cross-team collaboration, handling production incidents

---

## 🔍 Gap Analysis

### What Beginners Lack
1. **Scale thinking**: Don't understand what changes when data is 1TB vs 1GB
2. **Production mindset**: No error handling, no monitoring, no retries in their code
3. **Performance optimization**: Write Spark/SQL that works but is 10x slower than it should be
4. **Data modeling**: Don't understand star schema, slowly changing dimensions
5. **Cloud cost awareness**: Use expensive services when cheaper alternatives exist

### What Intermediate Engineers Lack
1. **System design skills**: Can build individual components but can't design an entire platform
2. **Data quality engineering**: No automated testing of pipeline outputs
3. **Advanced Kafka/Flink**: Only use Kafka as a queue, don't understand partitioning deeply
4. **DataOps practices**: No CI/CD for their pipelines, manual deployments
5. **Stakeholder communication**: Can't explain technical decisions to non-technical audiences

### How to Reach Top 1%
1. **Contribute to open source**: Kafka, Spark, Airflow, dbt — even documentation
2. **Understand internals**: Know why Spark uses lazy evaluation, how Kafka achieves durability
3. **Cost/performance trade-offs**: Always think in terms of both, not just "make it work"
4. **Write about what you learn**: Blog posts, LinkedIn articles — builds credibility
5. **Build in public**: GitHub with real, well-documented projects
6. **Be a T-shaped engineer**: Deep in data engineering, aware of ML, software engineering, DevOps

---

## 📅 Daily Practice Plan

### Morning Routine (30 minutes)
```
Mon/Wed/Fri: 1-2 SQL problems on LeetCode (focus: Hard difficulty window functions)
Tue/Thu/Sat: 1 Python coding problem (focus: data manipulation, file processing)
Sunday: System design reading (30 min)
```

### Weekly Project Work (2–3 hours)
```
Week 1: Build [[Project 01 - CSV Data Pipeline]] — implement with proper error handling
Week 2: Add Apache Airflow orchestration to it
Week 3: Build [[Project 03 - Retail POS ETL Pipeline]] — with monitoring
Week 4: Deploy to AWS (S3 + Athena + Glue)
(Continue through projects progressively)
```

### Monthly Goals
```
Month 1: Master SQL (LeetCode 50 SQL), Python data manipulation, 3 beginner projects
Month 2: Master Spark, build 3 more projects, learn Airflow
Month 3: Learn Kafka + streaming, 2 advanced projects, AWS certification prep
Month 4: System design practice, portfolio polish, start applying
```

### Debugging Skills (Essential)
```python
# Always add logging to pipelines
import logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s %(levelname)s %(message)s',
    handlers=[
        logging.FileHandler('pipeline.log'),
        logging.StreamHandler()
    ]
)
logger = logging.getLogger(__name__)

# Profile slow Spark jobs
df.explain(mode="cost")      # Show query plan with cost estimates
df.explain(mode="extended")  # Detailed physical + logical plan
spark.sparkContext.setLogLevel("WARN")  # Reduce log noise in production
```

---

## 🎯 Job Application Strategy

### Resume
- 1 page maximum (2 pages for 8+ years experience)
- Use numbers everywhere — rows processed, latency reduced, cost saved
- List projects with GitHub links
- Skills section: List tools by category (Processing, Storage, Cloud, Orchestration)

### Coding Interview Prep Resources
- **SQL**: LeetCode SQL 50, Mode Analytics Tutorial, StrataScratch
- **Python**: LeetCode (Easy/Medium Data Manipulation), HackerRank
- **System Design**: ByteByteGo, Designing Data-Intensive Applications (Kleppmann book)
- **Kafka/Spark**: Official documentation + Confluent's free courses

### Salary Negotiation
- Research market rates: Levels.fyi, Glassdoor, LinkedIn Salary
- Data Engineers with 3+ years: $120K–$180K USD (US market)
- Get competing offers — nothing negotiates better than a competing offer
- Always negotiate — companies expect it

---

## 🔗 Related

- [[Top 1% Data Engineer Strategy]]
- [[Data Engineering Blueprint Index]]
- [[Advanced & Underrated Tools]]
- [[Big Data Tools Overview]]

---

*Tags: #career #roadmap #data-engineering #skills #sql #python #spark #kafka #aws #interview #resume*
