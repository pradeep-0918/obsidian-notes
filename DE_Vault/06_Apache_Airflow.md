# 06 — Apache Airflow
#airflow #orchestration #dag #taskflow #xcom #sensors

**Roadmap Day:** 11 | [[DE_Vault/00_Master_Index]] | Prev → [[05_Structured_Streaming]] | Next → [[07_Data_Modeling]]

---

## 🗺️ Topic Map

```
Apache Airflow
├── DAG Anatomy (schedule, catchup, max_active_runs)
├── Operators (Python, Bash, Sensor, TaskFlow)
├── Task Dependencies & Branching
├── XCom (inter-task data)
├── Variables & Connections
├── Retries & Callbacks
├── Executors (Local, Celery, Kubernetes)
├── Idempotency Patterns
└── Best Practices
```

---

## 1. DAG Anatomy

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.utils.dates import days_ago
from datetime import datetime, timedelta

default_args = {
    "owner": "data-engineering",
    "retries": 3,
    "retry_delay": timedelta(minutes=5),
    "retry_exponential_backoff": True,
    "on_failure_callback": alert_slack,
    "email_on_failure": False,
}

with DAG(
    dag_id="weather_etl",
    description="Daily weather data ingestion",
    schedule_interval="0 6 * * *",     # 6 AM UTC daily
    start_date=datetime(2024, 1, 1),
    catchup=False,                      # don't backfill past runs
    max_active_runs=1,                  # only 1 run at a time
    max_active_tasks=5,
    tags=["etl", "weather"],
    default_args=default_args,
    params={"env": "prod"},
) as dag:
    ...
```

> 💡 **`catchup=False`:** For most production DAGs, set this. Otherwise, if you deploy today with `start_date=2024-01-01`, Airflow schedules hundreds of historical runs.

> 💡 **`schedule_interval` options:** `"@daily"`, `"@hourly"`, `"@weekly"`, cron string, `timedelta(hours=6)`, or `None` (manual trigger only).

---

## 2. Operators

```python
from airflow.operators.python import PythonOperator, BranchPythonOperator
from airflow.operators.bash import BashOperator
from airflow.sensors.filesystem import FileSensor
from airflow.sensors.external_task import ExternalTaskSensor

# Python Operator
extract_task = PythonOperator(
    task_id="extract_weather",
    python_callable=extract_weather,
    op_kwargs={"date": "{{ ds }}",    # Jinja template: execution date
               "source": "{{ params.source }}"},
)

# Bash Operator
run_spark = BashOperator(
    task_id="run_spark_job",
    bash_command="spark-submit /jobs/transform.py --date {{ ds }}",
)

# File Sensor (waits for file to exist)
wait_for_file = FileSensor(
    task_id="wait_for_file",
    filepath="/data/raw/{{ ds }}/data.parquet",
    poke_interval=60,           # check every 60 seconds
    timeout=3600,               # fail after 1 hour
    mode="reschedule",          # release slot while waiting (vs "poke")
)

# External Task Sensor (wait for another DAG to complete)
wait_for_upstream = ExternalTaskSensor(
    task_id="wait_upstream",
    external_dag_id="upstream_dag",
    external_task_id="final_task",
    execution_delta=timedelta(hours=1),
)
```

> 💡 **Sensor mode "reschedule" vs "poke":** `poke` holds a worker slot the entire time. `reschedule` releases the slot between checks — much more efficient for long waits.

---

## 3. TaskFlow API (modern Airflow 2.x)

```python
from airflow.decorators import dag, task
from datetime import datetime

@dag(schedule_interval="@daily", start_date=datetime(2024, 1, 1), catchup=False)
def weather_etl():

    @task
    def extract(date: str) -> list[dict]:
        return fetch_weather_api(date)

    @task
    def transform(raw_records: list[dict]) -> list[dict]:
        return [clean_record(r) for r in raw_records]

    @task
    def load(clean_records: list[dict]) -> None:
        insert_to_postgres(clean_records)

    # Wire up the pipeline
    raw = extract(date="{{ ds }}")
    clean = transform(raw)
    load(clean)

dag_instance = weather_etl()
```

> 💡 **TaskFlow vs classic:** TaskFlow automatically handles XCom serialization — return values are auto-pushed/pulled. Cleaner code, but XCom size limits still apply.

---

## 4. XCom — Inter-task Communication

```python
# Classic operators: push manually
def push_data(**context):
    result = {"row_count": 1000, "date": "2024-01-01"}
    context["ti"].xcom_push(key="etl_result", value=result)

def use_data(**context):
    result = context["ti"].xcom_pull(task_ids="push_task", key="etl_result")
    print(f"Row count: {result['row_count']}")
```

> ⚠️ **XCom size limit:** XCom stores data in Airflow's metadata DB. Keep values small (< 48KB). For large data, pass a path/S3 key, not the data itself.

---

## 5. Branching

```python
from airflow.operators.python import BranchPythonOperator

def choose_branch(**context):
    date = context["ds"]
    # Business logic
    if is_weekday(date):
        return "run_full_pipeline"
    else:
        return "run_lite_pipeline"

branch = BranchPythonOperator(
    task_id="branch_decision",
    python_callable=choose_branch,
)

# Return task_id string or list of task_id strings
branch >> [full_pipeline, lite_pipeline]
```

---

## 6. Connections & Variables

```python
from airflow.models import Variable
from airflow.hooks.base import BaseHook

# Variables — dynamic config
env = Variable.get("environment", default_var="dev")
config = Variable.get("etl_config", deserialize_json=True)

# Connections — stored credentials
conn = BaseHook.get_connection("postgres_prod")
print(conn.host, conn.login, conn.password)

# PostgresHook uses connections directly
from airflow.providers.postgres.hooks.postgres import PostgresHook
hook = PostgresHook(postgres_conn_id="postgres_prod")
df = hook.get_pandas_df("SELECT * FROM users LIMIT 100")
hook.run("INSERT INTO ...", parameters=[...])
```

---

## 7. Executors

| Executor | Use Case | Requires |
|----------|---------|---------|
| **SequentialExecutor** | Dev/testing, no parallelism | Nothing extra |
| **LocalExecutor** | Single machine, small-medium workloads | Postgres metadata DB |
| **CeleryExecutor** | Multi-machine, horizontal scale | Redis/RabbitMQ + workers |
| **KubernetesExecutor** | Cloud-native, task isolation | Kubernetes cluster |
| **CeleryKubernetesExecutor** | Hybrid | Both above |

---

## 8. Idempotency Patterns

```python
# Pattern 1: INSERT OR REPLACE (upsert)
def load_idempotent(date: str):
    df = transform(date)
    df.to_sql("weather", conn, if_exists="append", method="multi")
    # With UPSERT via ON CONFLICT (PostgreSQL)
    
# Pattern 2: Delete-then-insert
def load_delete_insert(date: str):
    cursor.execute("DELETE FROM weather WHERE date = %s", (date,))
    df.to_sql("weather", conn, if_exists="append")

# Pattern 3: Partition overwrite (Spark/Delta)
df.write.mode("overwrite") \
        .partitionBy("date") \
        .parquet("s3://bucket/weather/")  # only overwrites the date partition

# Test: Run DAG for same date twice → same result
```

---

## 9. Failure Callbacks

```python
def alert_slack(context):
    task_instance = context["task_instance"]
    dag_id = context["dag"].dag_id
    task_id = task_instance.task_id
    exec_date = context["execution_date"]
    log_url = task_instance.log_url

    message = f"❌ Task failed: {dag_id}.{task_id} | Date: {exec_date} | Logs: {log_url}"
    # Send to Slack webhook
    requests.post(SLACK_WEBHOOK, json={"text": message})

default_args = {
    "on_failure_callback": alert_slack,
    "on_retry_callback": alert_slack,
    "on_success_callback": None,
    "sla_miss_callback": alert_slack,
}
```

---

## 🃏 Interview Cheatsheet
- **`catchup=False`?** Prevents Airflow from scheduling historical runs when deploying old `start_date`.
- **XCom limits?** Stored in metadata DB; keep < 48KB. For larger data, pass S3 path.
- **Sensor mode "reschedule"?** Releases worker slot between poke checks; saves resources vs "poke" mode.
- **How to make DAG idempotent?** Use upsert/merge, delete-then-insert for same date, or partition overwrite.
- **TaskFlow vs classic operators?** TaskFlow auto-handles XCom; cleaner Python. Classic more flexible with operator features.
- **CeleryExecutor requires?** Redis/RabbitMQ message broker + separate worker machines running `airflow celery worker`.
