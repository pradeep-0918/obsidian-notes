# 🌬️ Airflow

> **Level:** Intermediate-Advanced | **Stage:** 3 of 5
> Apache Airflow is the most popular pipeline orchestration tool — it schedules, monitors, and manages your data workflows as code.

---

## 🤔 What is Apache Airflow?

Airflow is a **workflow orchestration platform** that lets you define, schedule, and monitor pipelines using Python code.

- Pipelines are defined as **DAGs** (Directed Acyclic Graphs)
- Each DAG has **tasks** connected by dependencies
- Airflow runs tasks in the right order, at the right time, on failure → retry

> **Real-world analogy:** Airflow is the **project manager** of your data team — it knows which jobs to run, in what order, how often, and sends you alerts when something breaks.

---

## 🧠 Key Concepts

### 1. DAG (Directed Acyclic Graph)
A DAG is your pipeline definition — tasks with dependencies.

```
           [extract_data]
                 ↓
          [validate_data]
          /              \
[transform_sales]   [transform_events]
          \              /
           [load_to_dw]
                 ↓
          [send_alert]
```

- **Directed:** Tasks flow in one direction
- **Acyclic:** No circular dependencies (no loops)

### 2. Basic DAG Structure
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime, timedelta

# Default settings for all tasks
default_args = {
    "owner": "data-team",
    "retries": 3,
    "retry_delay": timedelta(minutes=5),
    "email_on_failure": True,
    "email": ["data-team@company.com"]
}

# Define the DAG
with DAG(
    dag_id="daily_sales_pipeline",
    schedule_interval="0 6 * * *",    # Run at 6:00 AM daily
    start_date=datetime(2024, 1, 1),
    catchup=False,                     # Don't backfill missed runs
    default_args=default_args,
    tags=["sales", "daily"]
) as dag:
    pass  # Tasks defined below
```

### 3. Common Operators

```python
from airflow.operators.python import PythonOperator
from airflow.operators.bash import BashOperator
from airflow.providers.postgres.operators.postgres import PostgresOperator

# Python task
def extract_data():
    import requests
    data = requests.get("https://api.example.com/orders").json()
    return data

extract = PythonOperator(
    task_id="extract_orders",
    python_callable=extract_data
)

# Bash task
run_dbt = BashOperator(
    task_id="run_dbt_models",
    bash_command="dbt run --select staging+"
)

# SQL task
load_to_warehouse = PostgresOperator(
    task_id="load_data",
    postgres_conn_id="my_postgres",
    sql="INSERT INTO clean_orders SELECT * FROM staging_orders;"
)

# Set dependencies
extract >> load_to_warehouse >> run_dbt
```

### 4. Cron Schedule Syntax
```
"0 6 * * *"     → 6:00 AM every day
"0 */4 * * *"   → Every 4 hours
"0 0 * * 1"     → Every Monday at midnight
"*/15 * * * *"  → Every 15 minutes
"@daily"        → Once per day (midnight)
"@hourly"       → Once per hour
```

### 5. XCom (Cross-Communication Between Tasks)
```python
def extract(**context):
    data = {"rows": 1500, "status": "ok"}
    context["ti"].xcom_push(key="row_count", value=1500)

def validate(**context):
    count = context["ti"].xcom_pull(key="row_count", task_ids="extract")
    assert count > 0, "No data extracted!"

extract = PythonOperator(task_id="extract", python_callable=extract, provide_context=True)
validate = PythonOperator(task_id="validate", python_callable=validate, provide_context=True)
```

### 6. Connections & Variables
- **Connections:** Store DB credentials, API keys securely in Airflow UI
- **Variables:** Store config values (bucket names, thresholds)

```python
from airflow.models import Variable

S3_BUCKET = Variable.get("s3_data_bucket")
```

---

## 🏭 Real-World Airflow Setup

```
Airflow Metadata DB (Postgres) — stores DAG state, run history
Airflow Scheduler — decides when to trigger tasks
Airflow Webserver — UI for monitoring (localhost:8080)
Airflow Workers — execute the actual tasks

DAG Files: stored in /dags/ folder, synced from GitHub
```

**Production stack:**
- Self-hosted on Kubernetes (Helm chart)
- OR: **AWS MWAA**, **Google Cloud Composer**, **Astronomer** (managed Airflow)

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Apache Airflow** | Open-source orchestration |
| **Astronomer** | Managed Airflow (popular enterprise option) |
| **AWS MWAA** | Managed Airflow on AWS |
| **Google Cloud Composer** | Managed Airflow on GCP |
| **Prefect** | Modern alternative to Airflow (Python-first) |
| **Dagster** | Alternative with strong asset-based model |

---

## ✅ Mini Practice Tasks

- [ ] Install Airflow locally: `pip install apache-airflow` and run `airflow standalone`
- [ ] Create a DAG with 3 tasks: extract → transform → load
- [ ] Add retry logic and email alerts to a DAG
- [ ] Use XCom to pass row count between two tasks
- [ ] Schedule a DAG to run every day at 7 AM and trigger it manually

---

## 🔗 Related Notes

- [[Data Pipelines]] — Airflow orchestrates all types of pipelines
- [[ETL vs ELT]] — Airflow schedules both ETL and ELT workflows
- [[Data-Engineering-Vault/DataEngineering_Obsidian_Vault/Apache Spark]] — Airflow has SparkSubmitOperator to trigger Spark jobs
- [[Apache Kafka]] — Airflow can poll Kafka or trigger jobs from events
- [[Cloud for Data Engineering]] — Managed Airflow services on each cloud

---

## ➡️ Next Step

[[Cloud for Data Engineering]] — Learn the cloud platforms where all these tools run in production.
