# 🔄 ETL Pipelines — System Design Guide
#design #etl #elt #pipeline #architecture

> Designing pipelines that are reliable, scalable, and maintainable.

---

## 🧠 ETL vs ELT

```
ETL (Extract → Transform → Load):
Source → [Transform in pipeline] → Destination
Used when: Limited destination compute, legacy systems

ELT (Extract → Load → Transform):
Source → [Load raw] → Destination → [Transform in warehouse]
Used when: Powerful cloud warehouse (Snowflake, BigQuery, Redshift)
```

**The industry has shifted to ELT** because:
- Cloud warehouses (Snowflake, BigQuery) are extremely powerful
- Raw data preserved for re-transformation
- SQL (dbt) is more accessible than Spark for transformations
- Easier to debug (see intermediate data)

---

## 🏗️ Pipeline Architecture Patterns

### Pattern 1: Medallion Architecture (Bronze → Silver → Gold)

```
┌──────────────────────────────────────────────────────────────┐
│                    DATA LAKEHOUSE                             │
│                                                              │
│  BRONZE (Raw)        SILVER (Cleansed)    GOLD (Curated)    │
│  ┌─────────────┐     ┌─────────────┐     ┌─────────────┐   │
│  │ orders_raw  │────►│orders_clean │────►│fct_orders   │   │
│  │ (as-is from │     │(deduplicated│     │(business    │   │
│  │  source)    │     │ validated   │     │ metrics)    │   │
│  │             │     │ typed)      │     │             │   │
│  └─────────────┘     └─────────────┘     └─────────────┘   │
│                                                              │
│  - Never delete      - Clean types        - Star schema      │
│  - Append only       - Remove dupes       - Aggregations     │
│  - Source fidelity   - Standard nulls     - Business logic   │
│  - Partition by date - Schema applied     - Ready for BI     │
└──────────────────────────────────────────────────────────────┘
```

**Implementation:**
```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

class MedallionPipeline:
    """Implements Bronze → Silver → Gold architecture."""
    
    def __init__(self, spark: SparkSession, base_path: str):
        self.spark = spark
        self.base_path = base_path
    
    def bronze_ingestion(self, source_data, table_name: str, run_date: str):
        """Raw ingestion with minimal processing."""
        df = source_data \
            .withColumn("_ingested_at", F.current_timestamp()) \
            .withColumn("_source_system", F.lit("orders_api")) \
            .withColumn("_run_date", F.lit(run_date)) \
            .withColumn("_row_id", F.monotonically_increasing_id())
        
        # Append to bronze (never delete!)
        df.write \
            .mode("append") \
            .partitionBy("_run_date") \
            .format("delta") \
            .save(f"{self.base_path}/bronze/{table_name}")
        
        return df.count()
    
    def silver_cleansing(self, table_name: str, run_date: str):
        """Clean, validate, and deduplicate."""
        bronze_df = self.spark.read \
            .format("delta") \
            .load(f"{self.base_path}/bronze/{table_name}") \
            .filter(F.col("_run_date") == run_date)
        
        silver_df = bronze_df \
            .dropDuplicates(["order_id"]) \
            .filter(F.col("order_id").isNotNull()) \
            .filter(F.col("amount") >= 0) \
            .withColumn("amount", F.col("amount").cast("double")) \
            .withColumn("order_date", F.to_date("order_date", "yyyy-MM-dd")) \
            .withColumn("status", F.lower(F.trim(F.col("status")))) \
            .withColumn("_silver_processed_at", F.current_timestamp())
        
        # Validate
        invalid_count = silver_df.filter(
            F.col("status").isin(["created","processing","shipped","delivered","cancelled"])
            == False
        ).count()
        
        if invalid_count > silver_df.count() * 0.01:  # >1% invalid
            raise ValueError(f"Too many invalid records: {invalid_count}")
        
        # Write silver (overwrite partition)
        silver_df.write \
            .mode("overwrite") \
            .partitionBy("order_date") \
            .format("delta") \
            .option("replaceWhere", f"order_date = '{run_date}'") \
            .save(f"{self.base_path}/silver/{table_name}")
    
    def gold_aggregation(self, run_date: str):
        """Business-level aggregations for analytics."""
        orders = self.spark.read.format("delta") \
            .load(f"{self.base_path}/silver/orders")
        customers = self.spark.read.format("delta") \
            .load(f"{self.base_path}/silver/customers")
        
        # Daily metrics (incremental)
        daily_metrics = orders \
            .filter(F.col("order_date") == run_date) \
            .join(customers.select("customer_id", "segment", "region"), "customer_id", "left") \
            .groupBy("order_date", "region", "segment") \
            .agg(
                F.count("order_id").alias("order_count"),
                F.sum("amount").alias("total_revenue"),
                F.countDistinct("customer_id").alias("unique_customers"),
                F.avg("amount").alias("avg_order_value")
            )
        
        from delta.tables import DeltaTable
        
        if DeltaTable.isDeltaTable(self.spark, f"{self.base_path}/gold/daily_metrics"):
            delta_table = DeltaTable.forPath(
                self.spark, f"{self.base_path}/gold/daily_metrics"
            )
            delta_table.alias("target").merge(
                daily_metrics.alias("source"),
                "target.order_date = source.order_date AND target.region = source.region AND target.segment = source.segment"
            ).whenMatchedUpdateAll().whenNotMatchedInsertAll().execute()
        else:
            daily_metrics.write.format("delta").save(f"{self.base_path}/gold/daily_metrics")
```

---

## 🛡️ Pipeline Reliability Patterns

### Checkpointing and Idempotency

```python
import json
import hashlib
from datetime import datetime
from pathlib import Path

class PipelineCheckpointer:
    """Track pipeline state for safe restarts."""
    
    def __init__(self, checkpoint_path: str):
        self.checkpoint_path = Path(checkpoint_path)
        self.checkpoint_path.mkdir(parents=True, exist_ok=True)
    
    def _checkpoint_file(self, pipeline_id: str, run_date: str) -> Path:
        return self.checkpoint_path / f"{pipeline_id}_{run_date}.json"
    
    def is_completed(self, pipeline_id: str, run_date: str) -> bool:
        checkpoint_file = self._checkpoint_file(pipeline_id, run_date)
        if checkpoint_file.exists():
            with open(checkpoint_file) as f:
                state = json.load(f)
                return state.get("status") == "completed"
        return False
    
    def mark_step_complete(self, pipeline_id: str, run_date: str, step: str, metadata: dict = None):
        checkpoint_file = self._checkpoint_file(pipeline_id, run_date)
        state = {}
        if checkpoint_file.exists():
            with open(checkpoint_file) as f:
                state = json.load(f)
        
        state.setdefault("steps", {})[step] = {
            "status": "completed",
            "completed_at": datetime.utcnow().isoformat(),
            "metadata": metadata or {}
        }
        
        with open(checkpoint_file, 'w') as f:
            json.dump(state, f, indent=2)
    
    def mark_completed(self, pipeline_id: str, run_date: str):
        checkpoint_file = self._checkpoint_file(pipeline_id, run_date)
        state = {}
        if checkpoint_file.exists():
            with open(checkpoint_file) as f:
                state = json.load(f)
        
        state["status"] = "completed"
        state["completed_at"] = datetime.utcnow().isoformat()
        
        with open(checkpoint_file, 'w') as f:
            json.dump(state, f, indent=2)
```

### Data Quality Gates

```python
from dataclasses import dataclass
from typing import Callable

@dataclass
class QualityCheck:
    name: str
    check_fn: Callable
    threshold: float  # Minimum pass rate (0-1)
    severity: str  # 'warning' or 'error'

class DataQualityGate:
    """Enforces data quality before pipeline proceeds."""
    
    def __init__(self, checks: list[QualityCheck]):
        self.checks = checks
    
    def validate(self, df) -> dict:
        results = {}
        total_rows = df.count()
        
        for check in self.checks:
            passing_rows = check.check_fn(df).count()
            pass_rate = passing_rows / total_rows if total_rows > 0 else 0
            
            results[check.name] = {
                "pass_rate": pass_rate,
                "passing_rows": passing_rows,
                "total_rows": total_rows,
                "passed": pass_rate >= check.threshold,
                "severity": check.severity
            }
        
        # Raise error for failed critical checks
        failures = [
            name for name, result in results.items()
            if not result["passed"] and result["severity"] == "error"
        ]
        
        if failures:
            raise ValueError(f"Data quality gate failed: {failures}\n{results}")
        
        return results

# Usage
gate = DataQualityGate([
    QualityCheck(
        name="no_null_order_ids",
        check_fn=lambda df: df.filter(F.col("order_id").isNotNull()),
        threshold=1.0,
        severity="error"
    ),
    QualityCheck(
        name="valid_amounts",
        check_fn=lambda df: df.filter(F.col("amount") > 0),
        threshold=0.95,
        severity="error"
    ),
    QualityCheck(
        name="known_statuses",
        check_fn=lambda df: df.filter(
            F.col("status").isin(["created","processing","shipped","delivered","cancelled"])
        ),
        threshold=0.99,
        severity="warning"
    ),
])

results = gate.validate(df)
```

---

## 📊 Pipeline Monitoring

```python
import time
from contextlib import contextmanager

class PipelineMonitor:
    """Track pipeline metrics for observability."""
    
    def __init__(self, pipeline_name: str):
        self.pipeline_name = pipeline_name
        self.metrics = {}
    
    @contextmanager
    def track_step(self, step_name: str):
        start_time = time.time()
        try:
            yield
            duration = time.time() - start_time
            self.metrics[step_name] = {
                "status": "success",
                "duration_seconds": duration
            }
        except Exception as e:
            duration = time.time() - start_time
            self.metrics[step_name] = {
                "status": "failed",
                "duration_seconds": duration,
                "error": str(e)
            }
            raise
    
    def record(self, key: str, value):
        self.metrics[key] = value
    
    def publish(self):
        # Send to your monitoring system
        print(f"Pipeline metrics: {self.metrics}")

# Usage
monitor = PipelineMonitor("daily_orders_etl")

with monitor.track_step("extract"):
    df = extract_from_source()
    monitor.record("rows_extracted", df.count())

with monitor.track_step("transform"):
    clean_df = transform(df)
    monitor.record("rows_after_transform", clean_df.count())

monitor.publish()
```

---

*Related: [[03-Tools/Airflow]] | [[03-Tools/Spark]] | [[04-System-Design/Data-Lake]] | Back to [[00-Index]]*
