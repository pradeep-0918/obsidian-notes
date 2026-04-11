# 🐘 Apache Hadoop — Complete Guide
#tool #hadoop #hdfs #mapreduce #distributed

> The foundation that started the big data revolution. Still relevant for HDFS and YARN in modern clusters.

---

## 🧠 What is Hadoop?

Hadoop is an **ecosystem**, not a single tool:
```
┌──────────────────────────────────────────┐
│              HADOOP ECOSYSTEM            │
│                                          │
│  ┌──────────────────────────────────┐   │
│  │  Applications & SQL              │   │
│  │  Hive  │  Pig  │  HBase  │ ...  │   │
│  ├──────────────────────────────────┤   │
│  │  Execution                       │   │
│  │  MapReduce  │  Spark  │  Tez    │   │
│  ├──────────────────────────────────┤   │
│  │  Resource Management             │   │
│  │         YARN                     │   │
│  ├──────────────────────────────────┤   │
│  │  Storage                         │   │
│  │         HDFS                     │   │
│  └──────────────────────────────────┘   │
└──────────────────────────────────────────┘
```

## Core Components

**HDFS** — Distributed file system (see [[02-Core-Concepts/File-Systems]])

**YARN** — Resource negotiator (cluster manager):
```bash
# YARN commands
yarn application -list                    # List running applications
yarn application -kill application_id     # Kill an application
yarn logs -applicationId application_id  # Get logs
```

**MapReduce** — Original batch processing (mostly replaced by Spark):
```python
# Modern replacement: use Spark instead
# But understanding MR helps understand Spark's internals
```

## Hive — SQL on Hadoop

```sql
-- Create external table pointing to S3/HDFS
CREATE EXTERNAL TABLE orders (
    order_id    BIGINT,
    customer_id BIGINT,
    amount      DOUBLE,
    status      STRING,
    order_date  DATE
)
PARTITIONED BY (year INT, month INT)
STORED AS PARQUET
LOCATION 's3://my-bucket/data/orders/';

-- Add partitions
ALTER TABLE orders ADD PARTITION (year=2024, month=1)
LOCATION 's3://my-bucket/data/orders/year=2024/month=1/';

-- Query (Hive will only read relevant partitions)
SELECT region, SUM(amount) as revenue
FROM orders
WHERE year = 2024 AND month BETWEEN 1 AND 3
GROUP BY region;
```

---

*Related: [[03-Tools/Spark]] | [[02-Core-Concepts/File-Systems]] | Back to [[00-Index]]*
