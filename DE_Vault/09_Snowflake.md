# 09 — Snowflake
#snowflake #data-warehouse #time-travel #zero-copy #streams #tasks

**Roadmap Day:** 17 | [[00_Master_Index]] | Prev → [[08_AWS_Data_Services]] | Next → [[10_dbt]]

---

## 🗺️ Topic Map

```
Snowflake
├── Architecture (3-layer: Storage / Compute / Cloud Services)
├── Virtual Warehouses
├── Micro-partitioning
├── Time Travel
├── Zero-Copy Cloning
├── Streams (CDC)
├── Tasks (scheduling)
├── Snowpipe (continuous ingest)
└── Clustering Keys
```

---

## 1. Architecture — 3-Layer Separation

```
┌──────────────────────────────────────────┐
│         Cloud Services Layer              │
│  (query parsing, optimization, metadata,  │
│   auth, transactions — always on)         │
└──────────────────┬───────────────────────┘
                   │
┌──────────────────▼───────────────────────┐
│         Compute Layer                     │
│  ┌───────────┐  ┌───────────┐            │
│  │  Virtual  │  │  Virtual  │  (XS to    │
│  │ Warehouse │  │ Warehouse │   6XL)     │
│  └───────────┘  └───────────┘            │
└──────────────────┬───────────────────────┘
                   │  (shared storage)
┌──────────────────▼───────────────────────┐
│         Storage Layer                     │
│  Columnar, compressed, micro-partitioned  │
│  (S3/Azure Blob/GCS — you don't see it)  │
└──────────────────────────────────────────┘
```

> 💡 **Key insight:** Compute and storage are fully decoupled. Multiple warehouses can query the same data simultaneously without contention. Scale compute independently of storage.

---

## 2. Virtual Warehouses

```sql
-- Create and manage warehouses
CREATE WAREHOUSE etl_wh
  WAREHOUSE_SIZE = 'LARGE'
  AUTO_SUSPEND = 60          -- suspend after 60s idle (save credits)
  AUTO_RESUME = TRUE
  MAX_CLUSTER_COUNT = 3      -- multi-cluster for concurrency scaling
  MIN_CLUSTER_COUNT = 1;

-- Switch warehouse mid-session
USE WAREHOUSE reporting_wh;

-- Monitor credit usage
SELECT * FROM SNOWFLAKE.ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY
WHERE WAREHOUSE_NAME = 'ETL_WH'
ORDER BY START_TIME DESC LIMIT 100;
```

> 💡 **Warehouse sizing:** Start small, monitor query times, scale up if needed. XS = 1 credit/hr, S = 2, M = 4, L = 8, XL = 16, 2XL = 32... doubling each size.

---

## 3. Micro-partitioning

- Snowflake automatically divides tables into **micro-partitions** (50-500MB compressed)
- Each partition stores metadata: min/max values, row count, distinct count per column
- **Partition pruning:** WHERE clauses eliminate entire partitions without scanning them
- No manual partitioning needed — Snowflake handles it

```sql
-- Clustering key — reorder micro-partitions to improve pruning
ALTER TABLE events CLUSTER BY (event_date, user_region);

-- Check clustering quality
SELECT SYSTEM$CLUSTERING_INFORMATION('events', '(event_date, user_region)');

-- Monitor (expensive!) reclustering
ALTER TABLE events RECLUSTER;
```

---

## 4. Time Travel

```sql
-- Query historical data (default 1 day, Enterprise = 90 days)
SELECT * FROM orders AT (OFFSET => -3600);            -- 1 hour ago
SELECT * FROM orders AT (TIMESTAMP => '2024-01-15 10:00:00'::TIMESTAMP);
SELECT * FROM orders BEFORE (STATEMENT => '<query_id>');

-- Restore dropped table
UNDROP TABLE orders;

-- Clone table at historical point
CREATE TABLE orders_backup CLONE orders
  AT (TIMESTAMP => DATEADD(days, -1, CURRENT_TIMESTAMP));
```

> 💡 **Time Travel storage cost:** Stores historical versions. 90-day Travel = ~90x storage cost at worst case. Use Fail-safe (7 additional days beyond Time Travel) for disaster recovery.

---

## 5. Zero-Copy Cloning

```sql
-- Instant clone — no data copied, only metadata!
CREATE TABLE orders_dev CLONE orders;                 -- table
CREATE SCHEMA analytics_dev CLONE analytics;          -- entire schema
CREATE DATABASE prod_copy CLONE production;            -- entire database

-- Clone at a point in time
CREATE TABLE orders_jan CLONE orders
  AT (TIMESTAMP => '2024-01-31 23:59:59'::TIMESTAMP);
```

> 💡 **How it works:** Clone shares underlying micro-partitions with original. Only new/modified data creates new partitions. Ideal for: dev/test environments, pre-migration snapshots, data science experiments.

---

## 6. Streams (CDC)

```sql
-- Create stream on a table (captures inserts, updates, deletes)
CREATE STREAM orders_stream ON TABLE orders;

-- Query stream — shows what changed since last consumed
SELECT * FROM orders_stream;
-- Columns: original cols + METADATA$ACTION (INSERT/DELETE) + METADATA$ISUPDATE + METADATA$ROW_ID

-- Consume stream in a transaction
BEGIN;
INSERT INTO orders_summary
  SELECT user_id, SUM(amount) FROM orders_stream
  WHERE METADATA$ACTION = 'INSERT'
  GROUP BY user_id;
COMMIT;  -- stream advances offset on COMMIT
```

> ⚠️ **Stream stale:** If stream isn't consumed before the Time Travel period expires for changed data, it becomes "stale" and must be recreated.

---

## 7. Tasks — Scheduled SQL

```sql
-- Create task (runs SQL on schedule)
CREATE TASK refresh_summary
  WAREHOUSE = etl_wh
  SCHEDULE = 'USING CRON 0 6 * * * UTC'
AS
  INSERT OVERWRITE INTO daily_summary
  SELECT date, COUNT(*) FROM orders GROUP BY date;

-- Task tree (chaining)
CREATE TASK child_task
  AFTER parent_task     -- triggers when parent succeeds
AS
  CALL my_stored_procedure();

-- Enable (tasks start in SUSPENDED state)
ALTER TASK refresh_summary RESUME;
ALTER TASK refresh_summary SUSPEND;
```

---

## 8. Snowpipe — Continuous Ingestion

```sql
-- Create pipe (reads from S3 using SQS notification)
CREATE PIPE events_pipe
  AUTO_INGEST = TRUE
  AS
  COPY INTO events
  FROM @my_s3_stage/events/
  FILE_FORMAT = (TYPE = PARQUET);

-- Check pipe status
SELECT SYSTEM$PIPE_STATUS('events_pipe');
```

> 💡 **Snowpipe vs COPY INTO:** Snowpipe is event-driven (S3 SQS notification → micro-batch load, ~1 min latency). COPY INTO is manual/scheduled bulk load.

---

## 🃏 Interview Cheatsheet
- **Snowflake 3 layers?** Cloud Services (metadata, auth, optimization), Compute (Virtual Warehouses), Storage (micro-partitioned columnar).
- **Zero-copy clone?** Instant — shares micro-partitions. No data copied. New writes create new partitions.
- **Time Travel vs Fail-safe?** Time Travel: queryable history (1-90 days). Fail-safe: 7 days beyond TT, only Snowflake support can recover.
- **Stream offset advance?** On COMMIT of a DML statement that reads the stream.
- **Micro-partition pruning?** Snowflake stores min/max per column per partition → eliminates partitions on WHERE clauses.
