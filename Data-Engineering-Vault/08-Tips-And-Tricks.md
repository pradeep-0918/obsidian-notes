# ⚡ Tips & Tricks — Industry Secrets
#tips #productivity #career #hacks

> Hard-won wisdom from experienced data engineers. What the courses don't teach you.

---

## 🚀 Performance Tips

### SQL
```sql
-- 1. Use EXPLAIN ANALYZE before optimizing anything
EXPLAIN (ANALYZE, BUFFERS, FORMAT JSON)
SELECT * FROM your_slow_query;

-- 2. Covering indexes eliminate table lookups entirely
CREATE INDEX idx_orders_covering
    ON orders (customer_id, order_date DESC)
    INCLUDE (amount, status);  -- All columns the query needs

-- 3. Partial indexes for filtered queries
CREATE INDEX idx_active_orders ON orders (created_at)
    WHERE status NOT IN ('cancelled', 'refunded');
-- Only indexes rows that match - much smaller!

-- 4. Avoid SELECT * in production
-- SELECT * reads ALL columns from disk even if you need 2
SELECT order_id, amount FROM orders;  -- Not SELECT *

-- 5. LIMIT before JOIN when possible
SELECT *
FROM (SELECT * FROM orders WHERE status = 'pending' LIMIT 1000) o
JOIN customers c ON o.customer_id = c.id;
```

### Python
```python
# 1. Use generators for large data - don't load everything into memory
def read_large_file(filepath):
    with open(filepath) as f:
        for line in f:  # Reads one line at a time
            yield json.loads(line)

# 2. Pandas: use appropriate dtypes (can reduce memory by 70%!)
df = pd.read_csv('data.csv', dtype={
    'id': 'int32',          # vs int64 (4 bytes vs 8 bytes)
    'status': 'category',   # vs object (fixed categories)
    'region': 'category',
})
print(df.memory_usage(deep=True))

# 3. Vectorize, don't loop over DataFrames
# BAD (slow):
for idx, row in df.iterrows():
    df.loc[idx, 'revenue'] = row['price'] * row['quantity']

# GOOD (fast):
df['revenue'] = df['price'] * df['quantity']

# 4. Use chunked reading for large files
for chunk in pd.read_csv('huge_file.csv', chunksize=100_000):
    process(chunk)

# 5. Profile before optimizing
import cProfile
cProfile.run('your_slow_function()', sort='cumulative')
```

### Spark
```python
# 1. Check if you actually need Spark
# For < 10GB, pandas is faster (no overhead)

# 2. Always check partition count
df.rdd.getNumPartitions()  # Should be ~2-4x your core count

# 3. Cache if reusing a DataFrame multiple times
df.cache()
df.count()  # Force caching

# 4. Use broadcast joins for tables < 10MB
from pyspark.sql.functions import broadcast
result = large_df.join(broadcast(small_lookup), "id")

# 5. Enable AQE (Spark 3.0+)
spark.conf.set("spark.sql.adaptive.enabled", "true")
# Automatically fixes skew, coalesces small partitions

# 6. Use appropriate file formats
# NEVER: CSV for Spark jobs
# ALWAYS: Parquet or Delta
```

---

## 🛡️ Reliability Tips

```python
# 1. Always set timeouts on external calls
import requests
response = requests.get(url, timeout=(5, 30))  # connect=5s, read=30s

# 2. Use exponential backoff for retries
import time
def retry_with_backoff(func, max_retries=5):
    for attempt in range(max_retries):
        try:
            return func()
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            wait_time = (2 ** attempt) + random.uniform(0, 1)
            print(f"Attempt {attempt+1} failed: {e}. Retrying in {wait_time:.1f}s")
            time.sleep(wait_time)

# 3. Always use transactions for multi-step operations
with conn.cursor() as cur:
    try:
        cur.execute("INSERT INTO orders ...")
        cur.execute("UPDATE inventory ...")
        cur.execute("INSERT INTO audit_log ...")
        conn.commit()
    except Exception as e:
        conn.rollback()
        raise

# 4. Validate data before writing
assert df['order_id'].notna().all(), "NULL order IDs found!"
assert (df['amount'] >= 0).all(), "Negative amounts found!"
assert df['order_id'].is_unique, "Duplicate order IDs!"

# 5. Write atomically (avoid half-written files)
# BAD: Write directly to destination
df.write.parquet("s3://bucket/production/orders.parquet")

# GOOD: Write to temp location first
df.write.parquet("s3://bucket/staging/orders.parquet")
# Then move atomically
s3.copy_object(...)
s3.delete_object(...)
```

---

## 💼 Career Tips

### Resume Tips for Data Engineers

```
DO:
✅ Quantify everything: "Reduced pipeline runtime by 60% (6h → 2.5h)"
✅ Mention scale: "Processed 10TB daily", "500M events/day"
✅ List specific tools + versions: "Apache Spark 3.5, PySpark"
✅ Show impact: "Enabled real-time fraud detection, saving $2M/year"
✅ Include GitHub link with actual projects

DON'T:
❌ "Responsible for data pipelines" (vague)
❌ List every tool you've ever touched
❌ Include tools you couldn't answer interview questions about
❌ Forget to mention cloud platforms (AWS/GCP/Azure)
```

### Resume Keywords (ATS)
```
Data Engineering: ETL, ELT, data pipeline, data warehouse, data lake, lakehouse
Processing: Apache Spark, PySpark, Apache Flink, MapReduce
Ingestion: Apache Kafka, Debezium, CDC, Airbyte, Fivetran
Orchestration: Apache Airflow, Prefect, Dagster, dbt
Databases: PostgreSQL, MySQL, Cassandra, MongoDB, Redis
Cloud: AWS S3, Glue, Redshift, EMR, Lambda, Kinesis
Analytics: Snowflake, BigQuery, Redshift, dbt, Presto/Trino
Formats: Parquet, Avro, ORC, Delta Lake, Apache Iceberg
```

### Negotiation Tips
```
1. Always negotiate (80% of offers have room)
2. State a number first if asked: anchor high
3. Total comp = salary + equity + bonus + benefits
4. DE market rates (2024, US):
   Junior:       $90K-$130K
   Mid:          $130K-$180K
   Senior:       $180K-$250K
   Staff/Lead:   $250K-$400K+
5. Remote adds ~10-20% purchasing power
```

---

## 🔧 Debugging Tips

```python
# 1. Add structured logging everywhere
import logging
import json

class JSONFormatter(logging.Formatter):
    def format(self, record):
        return json.dumps({
            'timestamp': self.formatTime(record),
            'level': record.levelname,
            'message': record.getMessage(),
            'pipeline': record.pipeline if hasattr(record, 'pipeline') else None,
        })

# 2. Log data statistics at each step
def log_df_stats(df, step_name):
    logger.info(f"{step_name}: rows={df.count()}, "
                f"nulls={df.filter(F.col('key').isNull()).count()}")

# 3. Use small samples for development
IS_DEV = os.environ.get("ENV") != "production"
if IS_DEV:
    df = df.sample(0.01)  # 1% sample

# 4. Save intermediate results for debugging
df_bronze.write.mode("overwrite").parquet("/tmp/debug_bronze/")
# Can reload and inspect without re-running from scratch

# 5. Diff two DataFrames
def diff_dfs(df1, df2, keys):
    """Find rows in df1 not in df2."""
    return df1.join(df2, on=keys, how="left_anti")
```

---

## 🏁 Hidden Industry Tips

1. **The real cost of bad data is 10x the cost of preventing it.** Invest in data quality early.

2. **Every query that runs in production will be run in production forever.** Write them well.

3. **Small files are the silent killer.** 10,000 files × 1MB = much slower than 10 files × 1GB.

4. **Your pipeline's SLA is your stakeholders' trust.** Late data with no communication = career damage.

5. **Learn to say no.** "We can't add real-time to this batch pipeline without a full redesign" is a valid answer.

6. **Documentation is part of the pipeline.** Undocumented pipelines are unmaintainable.

7. **The best pipeline is the one that doesn't need debugging.** Build in observability from day 1.

8. **Your local tests will not catch production failures.** Production data is always messier.

9. **Understand the business, not just the technology.** "Why do we need this metric?" prevents wasted work.

10. **Collaboration > Technical brilliance.** The best data engineers communicate clearly with analysts, scientists, and product managers.

---

*Related: [[09-Cheat-Sheets]] | Back to [[00-Index]]*
