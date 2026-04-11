# 🌱 Beginner Data Engineering Roadmap
#beginner #roadmap #career

> **Goal:** Go from zero to writing your first real data pipeline in 3 months.
> **Time commitment:** ~10 hours/week

---

## 🤔 What is Data Engineering? (Feynman Explanation)

Imagine a city's water supply system:
- **Water sources** = Raw data (databases, APIs, logs, sensors)
- **Pipes & pumps** = Data pipelines
- **Water treatment plant** = Data transformation & cleaning
- **Reservoirs** = Data warehouses / data lakes
- **Taps in your home** = Business dashboards, ML models, analysts

A **Data Engineer** builds and maintains that entire water infrastructure so that when an analyst or data scientist turns on the tap, clean, reliable data flows out.

**Without data engineers:**
- Analysts spend 80% of time cleaning data
- ML models train on garbage → produce garbage predictions
- Business decisions are made on incomplete/wrong data

---

## 📅 Month 1: Foundations

### Week 1–2: SQL Mastery

SQL is the most important skill for any data engineer. You will use it every single day.

**Topics to cover:**
- SELECT, WHERE, GROUP BY, ORDER BY, HAVING
- JOINs (INNER, LEFT, RIGHT, FULL OUTER)
- Window Functions (ROW_NUMBER, RANK, LAG, LEAD)
- CTEs (WITH clauses)
- Subqueries
- Aggregations (COUNT, SUM, AVG, MIN, MAX)
- String & Date functions
- Indexes (basic understanding)

**Practice resources:**
- LeetCode SQL problems (start Easy → Medium)
- SQLZoo
- Mode Analytics SQL Tutorial

**Sample query to understand:**
```sql
-- Find the top 3 products by revenue per region, for this month
WITH monthly_sales AS (
    SELECT
        region,
        product_id,
        product_name,
        SUM(quantity * price) AS revenue,
        ROW_NUMBER() OVER (PARTITION BY region ORDER BY SUM(quantity * price) DESC) AS rank
    FROM orders
    WHERE DATE_TRUNC('month', order_date) = DATE_TRUNC('month', CURRENT_DATE)
    GROUP BY region, product_id, product_name
)
SELECT region, product_name, revenue
FROM monthly_sales
WHERE rank <= 3
ORDER BY region, revenue DESC;
```

---

### Week 3–4: Python for Data Engineering

Python is the lingua franca of data engineering. Focus on:

**Core Python:**
```python
# Data structures you must know cold
lists = [1, 2, 3]
dicts = {"key": "value"}
sets = {1, 2, 3}

# List comprehensions (you'll use these constantly)
clean_data = [x.strip().lower() for x in raw_data if x is not None]

# File I/O
with open('data.csv', 'r') as f:
    lines = f.readlines()

# Error handling (critical for pipelines)
try:
    result = risky_operation()
except ValueError as e:
    logger.error(f"Bad data: {e}")
    raise
```

**Pandas basics:**
```python
import pandas as pd

# Read data
df = pd.read_csv('sales.csv', parse_dates=['date'])

# Explore
df.head()
df.info()
df.describe()
df.isnull().sum()

# Transform
df['revenue'] = df['quantity'] * df['price']
df_clean = df.dropna(subset=['customer_id'])
df_grouped = df.groupby('region')['revenue'].sum().reset_index()

# Write
df_grouped.to_parquet('output/regional_revenue.parquet', index=False)
```

**Libraries to install and learn:**
```bash
pip install pandas numpy requests boto3 psycopg2-binary sqlalchemy
```

---

## 📅 Month 2: Core Tools

### Week 5–6: Databases Deep Dive

**Relational Databases (PostgreSQL)**

PostgreSQL is the most powerful open-source relational database. Learn:
- How to install and connect
- Creating tables, inserting data
- Indexes and query planning (`EXPLAIN ANALYZE`)
- Transactions (ACID)

```sql
-- Creating a well-designed table
CREATE TABLE orders (
    order_id      BIGSERIAL PRIMARY KEY,
    customer_id   BIGINT NOT NULL REFERENCES customers(customer_id),
    order_date    TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    status        VARCHAR(20) CHECK (status IN ('pending','shipped','delivered','cancelled')),
    total_amount  NUMERIC(12,2) NOT NULL CHECK (total_amount >= 0),
    created_at    TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    updated_at    TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

-- Index for common query patterns
CREATE INDEX idx_orders_customer_date ON orders(customer_id, order_date DESC);
CREATE INDEX idx_orders_status ON orders(status) WHERE status != 'delivered';
```

**When to use which database:**
| Database Type | Use When | Examples |
|---------------|----------|---------|
| Relational | Structured data, ACID needed | PostgreSQL, MySQL |
| Document | Flexible schema, JSON data | MongoDB, CouchDB |
| Key-Value | Caching, session storage | Redis, DynamoDB |
| Column | Analytics, time-series | Cassandra, ClickHouse |
| Graph | Relationships matter | Neo4j, ArangoDB |

See [[02-Core-Concepts/Databases]] for deep dives on each type.

---

### Week 7–8: Your First Pipeline

Build a complete ETL pipeline from scratch:

```
[Public API] → [Python Script] → [PostgreSQL] → [CSV Report]
```

**Step-by-step project:**

```python
import requests
import pandas as pd
import psycopg2
from datetime import datetime
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

def extract(api_url: str) -> list[dict]:
    """Extract data from API."""
    logger.info(f"Extracting from {api_url}")
    response = requests.get(api_url, timeout=30)
    response.raise_for_status()
    return response.json()

def transform(raw_data: list[dict]) -> pd.DataFrame:
    """Clean and transform raw data."""
    df = pd.DataFrame(raw_data)
    
    # Rename columns to snake_case
    df.columns = [c.lower().replace(' ', '_') for c in df.columns]
    
    # Drop rows with missing critical fields
    df = df.dropna(subset=['id', 'name'])
    
    # Add metadata
    df['ingested_at'] = datetime.utcnow()
    df['source'] = 'public_api'
    
    logger.info(f"Transformed {len(df)} records")
    return df

def load(df: pd.DataFrame, conn_string: str, table: str):
    """Load data into PostgreSQL."""
    engine = create_engine(conn_string)
    df.to_sql(table, engine, if_exists='append', index=False, method='multi')
    logger.info(f"Loaded {len(df)} rows into {table}")

if __name__ == "__main__":
    raw = extract("https://jsonplaceholder.typicode.com/posts")
    clean = transform(raw)
    load(clean, "postgresql://user:pass@localhost/mydb", "posts")
```

---

## 📅 Month 3: Pipeline Orchestration + Cloud Basics

### Week 9–10: Apache Airflow

Airflow lets you schedule and monitor your pipelines. Think of it as a very smart cron job with a visual dashboard.

**Key concepts:**
- **DAG** (Directed Acyclic Graph) = your pipeline definition
- **Task** = a single step in the pipeline
- **Operator** = the type of task (PythonOperator, BashOperator, etc.)
- **Schedule** = when it runs (cron expression)

**Your first DAG:**
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime, timedelta

default_args = {
    'owner': 'data-team',
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
    'email_on_failure': True,
}

with DAG(
    dag_id='my_first_pipeline',
    default_args=default_args,
    start_date=datetime(2024, 1, 1),
    schedule_interval='0 6 * * *',  # Daily at 6am
    catchup=False,
    tags=['beginner', 'etl'],
) as dag:

    extract_task = PythonOperator(
        task_id='extract',
        python_callable=extract,
    )

    transform_task = PythonOperator(
        task_id='transform',
        python_callable=transform,
    )

    load_task = PythonOperator(
        task_id='load',
        python_callable=load,
    )

    # Define dependencies
    extract_task >> transform_task >> load_task
```

See [[03-Tools/Airflow]] for the complete Airflow guide.

---

### Week 11–12: Cloud Basics (AWS)

Every modern data pipeline runs in the cloud. Start with AWS:

**Core AWS services for DE:**
| Service | What it does | Analogy |
|---------|-------------|---------|
| S3 | Object storage | Hard drive in the sky |
| RDS | Managed relational DB | PostgreSQL without maintenance |
| Redshift | Data warehouse | Fast analytics SQL engine |
| Glue | ETL service | Managed Spark/pandas |
| EMR | Managed Hadoop/Spark | Cluster in the cloud |
| Kinesis | Real-time streaming | Managed Kafka |
| Lambda | Serverless compute | Functions that auto-scale |

**Your first S3 interaction:**
```python
import boto3

s3 = boto3.client('s3')

# Upload a file
s3.upload_file('local_data.csv', 'my-bucket', 'data/2024/sales.csv')

# Download a file
s3.download_file('my-bucket', 'data/2024/sales.csv', 'downloaded.csv')

# List files with prefix
response = s3.list_objects_v2(Bucket='my-bucket', Prefix='data/2024/')
for obj in response.get('Contents', []):
    print(obj['Key'])

# Read directly into pandas (smart_open trick)
import pandas as pd
df = pd.read_csv('s3://my-bucket/data/2024/sales.csv')
```

---

## ✅ Beginner Checklist

Before moving to [[01-Roadmap/Intermediate]], make sure you can:

- [ ] Write complex SQL queries with window functions and CTEs
- [ ] Build a Python ETL script from scratch
- [ ] Connect Python to PostgreSQL and read/write data
- [ ] Understand what Parquet is and why it's better than CSV
- [ ] Create a basic Airflow DAG
- [ ] Upload/download files to/from S3
- [ ] Explain what a data warehouse is to a non-technical person
- [ ] Have at least 1 completed project on GitHub

---

## 🎯 Beginner Projects to Build

1. **Weather Data Pipeline** — Fetch weather API → store in PostgreSQL → generate daily report
2. **Reddit Sentiment Tracker** — Pull Reddit posts → sentiment analysis → store trends
3. **COVID Dashboard** — Fetch public data → transform → visualize in Metabase

See [[05-Projects/Beginner-Projects]] for full project guides.

---

## 📚 Resources for Beginners

- Books: [[07-Resources/Books]]
- Communities: [[07-Resources/Communities]]
- Cheat Sheets: [[09-Cheat-Sheets]]

---

*Next: [[01-Roadmap/Intermediate]] | Back to [[00-Index]]*
