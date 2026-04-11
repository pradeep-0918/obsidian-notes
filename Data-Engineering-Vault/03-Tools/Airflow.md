# 🌬️ Apache Airflow — Complete Guide
#tool #airflow #orchestration #workflow

> The industry standard for scheduling and monitoring data pipelines. Think of it as cron on steroids with a visual dashboard.

---

## 🧠 Core Concepts

```
DAG (Directed Acyclic Graph):
    ┌──────────┐     ┌──────────┐     ┌──────────┐
    │ Extract  │────►│Transform │────►│  Load    │
    └──────────┘     └──────────┘     └──────────┘
         │                                  │
         └─────────────────────────────────►│
                                       ┌────┴──────┐
                                       │  Notify   │
                                       └───────────┘

Key concepts:
- DAG = The pipeline definition
- Task = A single step in the pipeline
- Operator = The type of task
- Schedule = When the DAG runs
- Run = A single execution of the DAG
- XCom = Data passed between tasks
- Connection = Stored credentials
- Variable = Configuration values
```

---

## 🚀 Installation & Setup

```bash
# Install Airflow
pip install apache-airflow==2.8.0 \
  apache-airflow-providers-amazon \
  apache-airflow-providers-postgres \
  apache-airflow-providers-databricks

# Initialize database
airflow db init

# Create admin user
airflow users create \
  --username admin \
  --firstname Admin \
  --lastname User \
  --role Admin \
  --email admin@example.com

# Start (development)
airflow webserver -p 8080 &
airflow scheduler &
```

```yaml
# docker-compose.yml (production-like)
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_USER: airflow
      POSTGRES_PASSWORD: airflow
      POSTGRES_DB: airflow

  redis:
    image: redis:7

  airflow-webserver:
    image: apache/airflow:2.8.0
    command: webserver
    environment:
      AIRFLOW__CORE__EXECUTOR: CeleryExecutor
      AIRFLOW__DATABASE__SQL_ALCHEMY_CONN: postgresql+psycopg2://airflow:airflow@postgres/airflow
      AIRFLOW__CELERY__BROKER_URL: redis://redis:6379/0
      AIRFLOW__CORE__FERNET_KEY: ''
      AIRFLOW__CORE__DAGS_ARE_PAUSED_AT_CREATION: 'true'
    ports:
      - "8080:8080"

  airflow-scheduler:
    image: apache/airflow:2.8.0
    command: scheduler

  airflow-worker:
    image: apache/airflow:2.8.0
    command: celery worker
```

---

## 📝 DAG Patterns

### Pattern 1: Basic ETL DAG

```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from airflow.providers.postgres.operators.postgres import PostgresOperator
from airflow.providers.amazon.aws.operators.s3 import S3CopyObjectOperator
from datetime import datetime, timedelta
import logging

logger = logging.getLogger(__name__)

# ── DAG CONFIGURATION ─────────────────────────────────────────────

default_args = {
    'owner': 'data-engineering',
    'depends_on_past': False,
    'start_date': datetime(2024, 1, 1),
    'email': ['data-team@company.com'],
    'email_on_failure': True,
    'email_on_retry': False,
    'retries': 3,
    'retry_delay': timedelta(minutes=5),
    'retry_exponential_backoff': True,
    'max_retry_delay': timedelta(minutes=30),
    'execution_timeout': timedelta(hours=2),
}

with DAG(
    dag_id='daily_sales_etl',
    default_args=default_args,
    description='Daily sales data ETL pipeline',
    schedule_interval='0 6 * * *',   # Daily at 6am UTC
    catchup=False,                    # Don't backfill missed runs
    max_active_runs=1,                # Only one concurrent run
    tags=['sales', 'daily', 'production'],
    doc_md="""
    ## Daily Sales ETL
    
    Extracts sales data from PostgreSQL, transforms in Spark,
    and loads to the data warehouse.
    
    **SLA**: Must complete by 8am UTC
    **Owner**: Data Engineering
    **Oncall**: #data-oncall Slack
    """,
) as dag:

    # ── TASKS ─────────────────────────────────────────────────────
    
    def check_source_data(**context):
        """Verify source data exists before processing."""
        execution_date = context['ds']  # '2024-01-15'
        count = get_source_record_count(execution_date)
        
        if count == 0:
            raise ValueError(f"No source data found for {execution_date}")
        if count < 1000:
            logger.warning(f"Low record count: {count} for {execution_date}")
        
        logger.info(f"Source check passed: {count} records")
        return count  # Stored as XCom
    
    def extract_data(**context):
        """Extract data from source system."""
        execution_date = context['ds']
        record_count = context['ti'].xcom_pull(task_ids='check_source')
        
        df = read_from_postgres(
            table='sales',
            where=f"date = '{execution_date}'"
        )
        
        output_path = f"s3://raw-bucket/sales/date={execution_date}/sales.parquet"
        df.to_parquet(output_path)
        
        return output_path
    
    def transform_data(**context):
        """Transform extracted data with Spark."""
        input_path = context['ti'].xcom_pull(task_ids='extract')
        execution_date = context['ds']
        
        output_path = f"s3://processed-bucket/sales/date={execution_date}/"
        
        spark_transform(input_path, output_path)
        return output_path
    
    def load_data(**context):
        """Load transformed data to warehouse."""
        input_path = context['ti'].xcom_pull(task_ids='transform')
        execution_date = context['ds']
        
        load_to_warehouse(input_path, partition_date=execution_date)
    
    def notify_success(**context):
        """Send success notification."""
        execution_date = context['ds']
        send_slack_message(f"✅ Daily sales ETL completed for {execution_date}")
    
    # ── TASK INSTANCES ───────────────────────────────────────────
    
    check_task = PythonOperator(
        task_id='check_source',
        python_callable=check_source_data,
    )
    
    extract_task = PythonOperator(
        task_id='extract',
        python_callable=extract_data,
    )
    
    transform_task = PythonOperator(
        task_id='transform',
        python_callable=transform_data,
        pool='spark_pool',  # Limit concurrent Spark jobs
    )
    
    load_task = PythonOperator(
        task_id='load',
        python_callable=load_data,
    )
    
    notify_task = PythonOperator(
        task_id='notify',
        python_callable=notify_success,
        trigger_rule='all_success',
    )
    
    # ── DEPENDENCIES ─────────────────────────────────────────────
    check_task >> extract_task >> transform_task >> load_task >> notify_task
```

---

### Pattern 2: Dynamic Task Generation

```python
from airflow.decorators import dag, task

REGIONS = ['US', 'EU', 'APAC', 'LATAM']

@dag(
    dag_id='regional_reports',
    schedule_interval='@daily',
    start_date=datetime(2024, 1, 1),
    catchup=False,
)
def regional_pipeline():
    
    @task
    def get_regions() -> list[str]:
        """Dynamically determine which regions to process."""
        return fetch_active_regions_from_db()
    
    @task
    def process_region(region: str, **context) -> dict:
        """Process a single region."""
        date = context['ds']
        records = extract_region_data(region, date)
        transform_and_load(records, region, date)
        return {"region": region, "records": len(records)}
    
    @task
    def aggregate_results(results: list[dict]) -> None:
        """Combine all regional results."""
        total = sum(r['records'] for r in results)
        logger.info(f"Total records processed: {total}")
        update_dashboard(results)
    
    regions = get_regions()
    
    # Dynamic task mapping (Airflow 2.3+)
    region_results = process_region.expand(region=regions)
    aggregate_results(region_results)

dag_instance = regional_pipeline()
```

---

### Pattern 3: Sensor Pattern

```python
from airflow.sensors.s3_key_sensor import S3KeySensor
from airflow.sensors.external_task import ExternalTaskSensor
from airflow.sensors.sql import SqlSensor

with DAG('dependent_pipeline', ...) as dag:
    
    # Wait for upstream file to arrive
    wait_for_file = S3KeySensor(
        task_id='wait_for_upstream_file',
        bucket_name='my-data-bucket',
        bucket_key='upstream/{{ ds }}/data_complete.flag',
        aws_conn_id='aws_default',
        poke_interval=300,       # Check every 5 minutes
        timeout=7200,            # Give up after 2 hours
        mode='reschedule',       # Release worker slot while waiting
    )
    
    # Wait for another DAG to complete
    wait_for_upstream_dag = ExternalTaskSensor(
        task_id='wait_for_upstream_dag',
        external_dag_id='daily_sales_etl',
        external_task_id='load',
        allowed_states=['success'],
        execution_delta=timedelta(hours=0),
        poke_interval=60,
        timeout=3600,
        mode='reschedule',
    )
    
    # Wait for data to meet a SQL condition
    wait_for_data = SqlSensor(
        task_id='wait_for_source_data',
        conn_id='postgres_default',
        sql="SELECT COUNT(*) FROM orders WHERE date = '{{ ds }}' HAVING COUNT(*) > 100",
        poke_interval=120,
        timeout=3600,
        mode='reschedule',
    )
    
    [wait_for_file, wait_for_upstream_dag, wait_for_data] >> process_task
```

---

### Pattern 4: Task Groups

```python
from airflow.utils.task_group import TaskGroup

with DAG('complex_pipeline', ...) as dag:
    
    start = DummyOperator(task_id='start')
    
    with TaskGroup("extraction", tooltip="Data extraction tasks") as extraction:
        extract_orders = PythonOperator(task_id='extract_orders', ...)
        extract_customers = PythonOperator(task_id='extract_customers', ...)
        extract_products = PythonOperator(task_id='extract_products', ...)
    
    with TaskGroup("transformation") as transformation:
        transform_orders = PythonOperator(task_id='transform_orders', ...)
        build_customer_features = PythonOperator(task_id='customer_features', ...)
    
    with TaskGroup("loading") as loading:
        load_facts = PythonOperator(task_id='load_facts', ...)
        load_dims = PythonOperator(task_id='load_dims', ...)
        update_aggregates = PythonOperator(task_id='update_aggregates', ...)
    
    end = DummyOperator(task_id='end')
    
    start >> extraction >> transformation >> loading >> end
```

---

## 🔧 Custom Operators

```python
from airflow.models import BaseOperator
from airflow.utils.decorators import apply_defaults

class SparkSubmitWithMonitoringOperator(BaseOperator):
    """Submit Spark job with custom monitoring."""
    
    template_fields = ['application', 'arguments']  # Jinja templating
    
    @apply_defaults
    def __init__(
        self,
        application: str,
        arguments: list = None,
        executor_instances: int = 10,
        **kwargs
    ):
        super().__init__(**kwargs)
        self.application = application
        self.arguments = arguments or []
        self.executor_instances = executor_instances
    
    def execute(self, context):
        self.log.info(f"Submitting Spark job: {self.application}")
        
        # Start monitoring
        job_id = submit_spark_job(
            app=self.application,
            args=self.arguments + [f"--date={context['ds']}"],
            executors=self.executor_instances
        )
        
        # Wait and monitor
        status = wait_for_spark_job(job_id, timeout=7200)
        
        if status != "SUCCEEDED":
            raise AirflowException(f"Spark job {job_id} failed with status: {status}")
        
        # Push metrics as XCom
        metrics = get_job_metrics(job_id)
        self.xcom_push(context, key='spark_metrics', value=metrics)
        
        return job_id
```

---

## 📊 Airflow Best Practices

| Practice | Why | How |
|---|---|---|
| Use `catchup=False` | Avoid running hundreds of backfill jobs | Set in DAG or airflow.cfg |
| Set timeouts | Prevent stuck tasks from blocking resources | `execution_timeout` on tasks |
| Use connection pool | Don't overwhelm databases | `pool` parameter on tasks |
| Idempotent tasks | Safe to retry | Upserts, overwrite mode |
| Avoid top-level code | Slow DAG parsing | Move logic inside functions |
| Use `mode='reschedule'` for sensors | Don't block worker slots | Sensor `mode` param |
| Template fields | Dynamic dates/configs | Use `{{ ds }}` macros |
| `max_active_runs=1` | Prevent concurrent runs | DAG parameter |

---

*Related: [[04-System-Design/ETL-Pipelines]] | [[03-Tools/Spark]] | Back to [[00-Index]]*
