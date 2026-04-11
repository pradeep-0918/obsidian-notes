# ❄️ Snowflake — Complete Guide
#tool #snowflake #warehouse #cloud #sql

> The cloud data warehouse that changed the industry. Separation of storage and compute is the key insight.

---

## 🧠 Why Snowflake is Different

Traditional databases: Storage and compute are coupled.
```
Redshift: Add storage → Add compute (or waste money)
On-prem:  Buy bigger server for both storage and compute
```

Snowflake: Separated.
```
Storage:  S3/Blob/GCS (elastic, cheap, pay per byte)
Compute:  Virtual Warehouses (pause when not in use, instant resize)

Result: Pay for storage always, pay for compute ONLY when running queries.
```

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────────────┐
│                   SNOWFLAKE ARCHITECTURE                  │
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │              CLOUD SERVICES LAYER                 │   │
│  │  Authentication | Query parsing | Optimization   │   │
│  │  Metadata management | Transaction management    │   │
│  └──────────────────────────────────────────────────┘   │
│                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │  Virtual     │  │  Virtual     │  │  Virtual     │     │
│  │  Warehouse  │  │  Warehouse  │  │  Warehouse  │     │
│  │  (Analytics)│  │  (Loading)  │  │  (Reporting)│     │
│  │  XL: 16 nodes│  │  M: 4 nodes │  │  S: 2 nodes │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │              STORAGE LAYER (S3/Azure/GCS)         │   │
│  │   Columnar, compressed, auto-clustered           │   │
│  └──────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────┘
```

---

## 🔑 Key Concepts

### Virtual Warehouses
```sql
-- Create warehouses for different workloads
CREATE WAREHOUSE analytics_wh
    WAREHOUSE_SIZE = 'X-LARGE'        -- 16 nodes
    AUTO_SUSPEND = 300                 -- Pause after 5 min idle
    AUTO_RESUME = TRUE
    INITIALLY_SUSPENDED = TRUE
    COMMENT = 'For heavy analytical queries';

CREATE WAREHOUSE etl_wh
    WAREHOUSE_SIZE = 'LARGE'
    AUTO_SUSPEND = 60
    AUTO_RESUME = TRUE
    MAX_CLUSTER_COUNT = 4             -- Multi-cluster for concurrency
    MIN_CLUSTER_COUNT = 1
    SCALING_POLICY = 'ECONOMY';

-- Use a specific warehouse
USE WAREHOUSE etl_wh;

-- Resize on the fly (takes ~5 seconds)
ALTER WAREHOUSE analytics_wh SET WAREHOUSE_SIZE = '2X-LARGE';
```

### Tables and Clustering
```sql
-- Standard table with cluster key
CREATE TABLE orders (
    order_id    NUMBER AUTOINCREMENT PRIMARY KEY,
    customer_id NUMBER,
    order_date  DATE,
    region      VARCHAR(50),
    amount      NUMBER(12, 2),
    status      VARCHAR(20)
)
CLUSTER BY (order_date, region);  -- Pre-sort data for faster queries

-- Transient table (no time travel, cheaper)
CREATE TRANSIENT TABLE staging_orders LIKE orders;

-- Temporary table (session-scoped)
CREATE TEMPORARY TABLE temp_analysis AS
SELECT * FROM orders WHERE status = 'pending';

-- External table (data in S3)
CREATE EXTERNAL TABLE ext_orders (
    order_id    NUMBER AS (VALUE:order_id::NUMBER),
    customer_id NUMBER AS (VALUE:customer_id::NUMBER),
    amount      FLOAT  AS (VALUE:amount::FLOAT)
)
WITH LOCATION = @my_s3_stage/orders/
FILE_FORMAT = (TYPE = 'PARQUET');
```

---

## 🔧 Advanced Snowflake Features

### Time Travel
```sql
-- Query data as it was 3 hours ago
SELECT * FROM orders AT (OFFSET => -10800);  -- seconds

-- Query as of a specific timestamp
SELECT * FROM orders AT (TIMESTAMP => '2024-01-15 12:00:00'::TIMESTAMP);

-- Query as of a specific statement
SELECT * FROM orders BEFORE (STATEMENT => '8e5d0ca9-005e-44e6-b858-a8f5b37c5726');

-- Restore a table to a previous state
CREATE TABLE orders_restored CLONE orders AT (OFFSET => -3600);

-- Undrop a table (within retention period)
UNDROP TABLE accidentally_deleted_table;

-- Set retention period (1-90 days for Enterprise)
ALTER TABLE critical_table SET DATA_RETENTION_TIME_IN_DAYS = 90;
```

### Zero-Copy Cloning
```sql
-- Instant clone (doesn't copy data, only metadata)
-- Great for testing/dev environments
CREATE DATABASE prod_clone CLONE production_db;
CREATE SCHEMA dev_schema CLONE production_schema;
CREATE TABLE test_orders CLONE prod.public.orders;

-- Test destructive operations safely
DELETE FROM test_orders WHERE status = 'cancelled';
-- Original orders table: untouched
```

### Streams and Tasks (CDC)
```sql
-- Create a stream to track changes
CREATE STREAM orders_stream ON TABLE orders
    APPEND_ONLY = FALSE;  -- Capture inserts, updates, AND deletes

-- Query the stream
SELECT
    *,
    METADATA$ACTION,          -- INSERT or DELETE
    METADATA$ISUPDATE,        -- Was this part of an update?
    METADATA$ROW_ID           -- Unique row identifier
FROM orders_stream;

-- Automated task to process stream changes
CREATE TASK process_order_changes
    WAREHOUSE = etl_wh
    SCHEDULE = '5 MINUTE'           -- Every 5 minutes
    WHEN SYSTEM$STREAM_HAS_DATA('orders_stream')
AS
    MERGE INTO order_summary s
    USING (
        SELECT customer_id, SUM(amount) as total
        FROM orders_stream
        WHERE METADATA$ACTION = 'INSERT'
        GROUP BY customer_id
    ) src
    ON s.customer_id = src.customer_id
    WHEN MATCHED THEN
        UPDATE SET s.total_revenue = s.total_revenue + src.total
    WHEN NOT MATCHED THEN
        INSERT (customer_id, total_revenue) VALUES (src.customer_id, src.total);

-- Activate the task
ALTER TASK process_order_changes RESUME;
```

### Dynamic Data Masking
```sql
-- Create masking policy
CREATE MASKING POLICY pii_email_mask AS (val STRING) RETURNS STRING ->
    CASE
        WHEN CURRENT_ROLE() IN ('DATA_SCIENTIST', 'ANALYST') THEN '****@****.***'
        WHEN CURRENT_ROLE() IN ('DATA_ENGINEER', 'ADMIN') THEN val
        ELSE '****REDACTED****'
    END;

-- Apply to column
ALTER TABLE customers
    MODIFY COLUMN email SET MASKING POLICY pii_email_mask;
```

---

## 🐍 Python + Snowflake

```python
import snowflake.connector
import pandas as pd
from snowflake.connector.pandas_tools import write_pandas

# Connect
conn = snowflake.connector.connect(
    account='myaccount.us-east-1',
    user='myuser',
    password='mypassword',
    database='ANALYTICS',
    schema='PUBLIC',
    warehouse='ANALYTICS_WH',
    role='DATA_ENGINEER'
)

cursor = conn.cursor()

# Execute query
cursor.execute("SELECT * FROM orders WHERE order_date >= CURRENT_DATE - 7")
df = cursor.fetch_pandas_all()

# Write DataFrame to Snowflake (efficient bulk load)
success, nchunks, nrows, _ = write_pandas(
    conn=conn,
    df=df,
    table_name='ORDERS_STAGING',
    auto_create_table=True,
    overwrite=True
)

# Using SQLAlchemy
from sqlalchemy import create_engine
from snowflake.sqlalchemy import URL

engine = create_engine(URL(
    account="myaccount",
    user="myuser",
    password="mypassword",
    database="ANALYTICS",
    schema="PUBLIC",
    warehouse="ANALYTICS_WH",
))

with engine.connect() as con:
    df = pd.read_sql("SELECT * FROM orders LIMIT 1000", con)
```

---

## 💰 Cost Optimization

```sql
-- See credit usage by warehouse
SELECT warehouse_name, SUM(credits_used) as total_credits
FROM snowflake.account_usage.warehouse_metering_history
WHERE start_time >= CURRENT_DATE - 30
GROUP BY 1
ORDER BY 2 DESC;

-- Find expensive queries
SELECT query_text, total_elapsed_time / 1000 as seconds,
       credits_used_cloud_services
FROM snowflake.account_usage.query_history
WHERE start_time >= CURRENT_DATE - 7
ORDER BY total_elapsed_time DESC
LIMIT 20;

-- Check table clustering efficiency
SELECT SYSTEM$CLUSTERING_INFORMATION('orders', '(order_date, region)');
```

---

*Related: [[04-System-Design/Data-Warehouse]] | [[04-System-Design/ETL-Pipelines]] | Back to [[00-Index]]*
