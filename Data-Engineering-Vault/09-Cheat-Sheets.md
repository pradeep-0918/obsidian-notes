# 📋 Cheat Sheets — Quick Reference
#cheatsheet #sql #python #spark #bash

> Quick reference for the most commonly used commands, patterns, and syntax.

---

## 🗃️ SQL Cheat Sheet

### Aggregation Functions
```sql
COUNT(*)              -- Count all rows
COUNT(DISTINCT col)   -- Count unique values (ignores NULLs)
SUM(col)              -- Total (NULLs treated as 0)
AVG(col)              -- Mean (ignores NULLs)
MAX(col) / MIN(col)   -- Extremes
STDDEV(col)           -- Standard deviation
PERCENTILE_APPROX(col, 0.5)  -- Median (Spark/Hive)
PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY col)  -- Median (PostgreSQL)
STRING_AGG(col, ', ')         -- Concatenate strings (PostgreSQL)
ARRAY_AGG(col)                -- Collect into array (PostgreSQL)
```

### Window Function Reference
```sql
-- Syntax
function() OVER (
    PARTITION BY col1, col2    -- Group within
    ORDER BY col3 DESC         -- Sort within group
    ROWS BETWEEN n PRECEDING AND m FOLLOWING  -- Frame
)

-- Ranking
ROW_NUMBER()    -- 1,2,3,4 (always unique)
RANK()          -- 1,2,2,4 (skip ties)
DENSE_RANK()    -- 1,2,2,3 (no skip)
NTILE(4)        -- Split into 4 equal buckets
PERCENT_RANK()  -- 0.0 to 1.0 relative rank

-- Offset
LAG(col, n, default)   -- Value n rows before
LEAD(col, n, default)  -- Value n rows after
FIRST_VALUE(col)       -- First value in window
LAST_VALUE(col)        -- Last value in window

-- Aggregate over window
SUM(col) OVER (...)   -- Running/grouped sum
AVG(col) OVER (...)   -- Running/grouped avg

-- Frame options
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  -- Running total
ROWS BETWEEN 6 PRECEDING AND CURRENT ROW           -- Rolling 7
ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING  -- Group total
```

### Date Functions
```sql
-- PostgreSQL
NOW()                           -- Current timestamp with TZ
CURRENT_DATE                    -- Current date
CURRENT_TIMESTAMP               -- Same as NOW()
DATE_TRUNC('month', ts)        -- Truncate to month start
DATE_PART('year', ts)          -- Extract year
EXTRACT(DOW FROM date)         -- Day of week (0=Sunday)
AGE(end_date, start_date)      -- Interval between dates
DATEDIFF(end, start)           -- Days between (Spark/Hive)
date + INTERVAL '7 days'       -- Add 7 days
TO_DATE(string, 'YYYY-MM-DD')  -- Parse string to date
TO_CHAR(date, 'Mon YYYY')      -- Format date to string

-- Spark SQL
date_trunc('month', col)
date_add(col, 7)
datediff(end_col, start_col)
months_between(end_col, start_col)
from_unixtime(unix_ts)
unix_timestamp(ts_col)
```

### String Functions
```sql
LOWER(str) / UPPER(str)
TRIM(str) / LTRIM / RTRIM
LENGTH(str) / CHAR_LENGTH(str)
SUBSTRING(str, start, length)
REPLACE(str, old, new)
CONCAT(str1, str2) / CONCAT_WS(sep, str1, str2)
SPLIT_PART(str, delimiter, n)   -- PostgreSQL
REGEXP_REPLACE(str, pattern, replacement)
REGEXP_EXTRACT(str, pattern, group)  -- Spark
LIKE '%pattern%'
ILIKE '%pattern%'                -- Case-insensitive (PostgreSQL)
```

---

## 🐍 Python Cheat Sheet

### Pandas Quick Reference
```python
# Reading
pd.read_csv('file.csv', parse_dates=['date'], dtype={'id': int})
pd.read_parquet('file.parquet', columns=['id', 'name'])
pd.read_json('file.json', orient='records')
pd.read_sql(sql, conn)

# Exploring
df.head(10)
df.tail(5)
df.info()
df.describe()
df.shape  # (rows, cols)
df.dtypes
df.isnull().sum()
df['col'].value_counts()
df['col'].nunique()

# Selecting
df['col']                      # Series
df[['col1', 'col2']]           # DataFrame
df.loc[0:10, 'col1':'col3']    # Label-based
df.iloc[0:10, 0:3]             # Integer-based
df[df['col'] > 100]            # Filter
df.query("col > 100 and status == 'active'")

# Transforming
df['new'] = df['a'] * df['b']
df['cat'] = pd.cut(df['age'], bins=[0,18,65,999], labels=['youth','adult','senior'])
df['str_col'] = df['str_col'].str.strip().str.lower()
df = df.rename(columns={'old': 'new'})
df = df.drop(columns=['col1', 'col2'])
df = df.dropna(subset=['critical_col'])
df = df.fillna({'col1': 0, 'col2': 'unknown'})

# Aggregating
df.groupby('region')['revenue'].sum()
df.groupby(['region', 'month']).agg({
    'revenue': ['sum', 'mean'],
    'orders': 'count'
})

# Merging
pd.merge(df1, df2, on='id', how='left')
pd.concat([df1, df2], ignore_index=True)

# Writing
df.to_parquet('output.parquet', index=False, compression='snappy')
df.to_csv('output.csv', index=False)
df.to_sql('table', engine, if_exists='append', index=False)
```

### Useful Patterns
```python
# Safe division
df['rate'] = df['numerator'] / df['denominator'].replace(0, pd.NA)

# Flatten nested JSON
df = pd.json_normalize(records, sep='_')

# Apply function with progress bar
from tqdm import tqdm
tqdm.pandas()
df['result'] = df['col'].progress_apply(my_func)

# Efficient bulk insert to PostgreSQL
from psycopg2.extras import execute_values
execute_values(cursor, "INSERT INTO table (a, b) VALUES %s", data_tuples)
```

---

## ⚡ PySpark Quick Reference

```python
# Imports
from pyspark.sql import SparkSession, functions as F, types as T, Window

# Session
spark = SparkSession.builder.appName("Job").getOrCreate()

# Read
df = spark.read.parquet("s3://bucket/data/")
df = spark.read.csv("path/", header=True, inferSchema=True)
df = spark.read.format("delta").load("s3://bucket/delta/")

# Common transforms
df.select("a", "b", F.col("c").alias("d"))
df.filter(F.col("status") == "active")
df.withColumn("new", F.col("a") * F.col("b"))
df.withColumn("date", F.to_date("str_col", "yyyy-MM-dd"))
df.dropDuplicates(["id"])
df.dropna(subset=["id", "amount"])
df.fillna({"amount": 0, "region": "unknown"})
df.groupBy("region").agg(F.sum("amount").alias("total"))
df.orderBy("date", ascending=False)
df.limit(1000)

# Joins
df1.join(df2, on="id", how="left")
df1.join(F.broadcast(df2), on="id")  # Broadcast small table
df1.join(df2, on="id", how="left_anti")  # NOT IN equivalent
df1.join(df2, on="id", how="left_semi")  # WHERE EXISTS equivalent

# Useful functions
F.when(condition, val).when(cond2, val2).otherwise(default)
F.coalesce(F.col("a"), F.col("b"), F.lit(0))  # First non-null
F.concat_ws("|", F.col("a"), F.col("b"))
F.explode(F.col("array_col"))
F.from_json(F.col("json_str"), schema)
F.to_json(F.col("struct_col"))
F.date_trunc("month", F.col("ts"))
F.unix_timestamp(F.col("ts"))

# Write
df.write.mode("overwrite").parquet("s3://bucket/output/")
df.write.mode("append").partitionBy("year","month").parquet("s3://bucket/")
df.write.format("delta").mode("overwrite").save("s3://bucket/delta/")
```

---

## 🌬️ Airflow Cheat Sheet

```python
# Cron expressions
'0 6 * * *'       # Daily at 6am
'0 * * * *'       # Hourly
'0 6 * * 1'       # Weekly Monday 6am
'0 6 1 * *'       # Monthly (1st day) 6am
'@daily'          # Daily at midnight
'@hourly'         # Every hour

# Common operators
PythonOperator(task_id='t', python_callable=fn)
BashOperator(task_id='t', bash_command='echo hello')
SQLExecuteQueryOperator(task_id='t', conn_id='pg', sql='SELECT 1')
S3KeySensor(task_id='t', bucket_name='b', bucket_key='{{ ds }}/file')
SparkSubmitOperator(task_id='t', application='job.py')

# XCom (pass data between tasks)
# Push: return value OR
ti.xcom_push(key='my_key', value=my_data)
# Pull:
value = ti.xcom_pull(task_ids='upstream_task', key='my_key')

# Template variables
{{ ds }}           # Execution date: '2024-01-15'
{{ ds_nodash }}    # '20240115'
{{ execution_date }}  # Datetime object
{{ next_ds }}      # Next execution date
{{ prev_ds }}      # Previous execution date
{{ ts }}           # Timestamp
{{ dag.dag_id }}   # DAG ID
{{ task.task_id }} # Task ID
```

---

## 🖥️ Command Line Cheat Sheet

```bash
# Kafka
kafka-topics.sh --list --bootstrap-server localhost:9092
kafka-topics.sh --describe --topic my-topic --bootstrap-server localhost:9092
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic my-topic --from-beginning
kafka-consumer-groups.sh --describe --group my-group --bootstrap-server localhost:9092

# HDFS
hdfs dfs -ls /path/
hdfs dfs -du -h /path/
hdfs dfs -put local_file.parquet /hdfs/path/
hdfs dfs -get /hdfs/path/file.parquet ./

# AWS CLI
aws s3 ls s3://bucket/prefix/ --recursive
aws s3 cp local.parquet s3://bucket/path/
aws s3 sync ./local_dir s3://bucket/path/
aws s3 cp --recursive s3://bucket/src/ s3://bucket/dst/

# Docker
docker-compose up -d           # Start all services
docker-compose logs -f kafka   # Follow logs
docker exec -it kafka bash     # Shell into container
docker stats                   # Resource usage

# Git (for data pipelines)
git log --oneline --graph     # Visualize history
git diff HEAD~1 HEAD          # What changed since last commit
git stash                     # Save changes temporarily
git bisect                    # Find which commit broke something
```

---

## 📊 Data Profiling Quick Checks

```python
def profile_dataframe(df: pd.DataFrame) -> dict:
    """Quick data quality profile."""
    return {
        "rows": len(df),
        "columns": len(df.columns),
        "null_pcts": (df.isnull().sum() / len(df) * 100).to_dict(),
        "dtypes": df.dtypes.astype(str).to_dict(),
        "duplicates": df.duplicated().sum(),
        "numeric_stats": df.describe().to_dict(),
        "cardinality": {col: df[col].nunique() for col in df.columns}
    }
```

---

*Related: [[08-Tips-And-Tricks]] | [[06-Interview-Prep/SQL-Questions]] | Back to [[00-Index]]*
