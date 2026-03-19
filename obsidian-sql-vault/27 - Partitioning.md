# 27 — Partitioning

#advanced #performance #dataengineering

---

## 🧠 Introduction

Partitioning splits a large table into **smaller physical segments** based on a partition key. MySQL treats it as one logical table, but internally distributes data for better performance and manageability.

> When a table has 100M+ rows, indexes alone may not be enough. Partitioning is the next level of scale.

---

## 🗺️ Types of Partitioning

| Type | Split By | Best For |
|------|----------|---------|
| `RANGE` | Value ranges | Date-based data, logs |
| `LIST` | Specific values | Category/region-based |
| `HASH` | Hash of column | Evenly distributing rows |
| `KEY` | MySQL-managed hash | Similar to HASH |

---

## 🔷 RANGE Partitioning (Most Common)

```sql
-- Partition orders by year
CREATE TABLE orders (
    order_id   INT NOT NULL AUTO_INCREMENT,
    user_id    INT NOT NULL,
    amount     DECIMAL(10,2),
    order_date DATE NOT NULL,
    status     VARCHAR(20),
    PRIMARY KEY (order_id, order_date)  -- partition key must be in PK!
)
PARTITION BY RANGE (YEAR(order_date)) (
    PARTITION p2021 VALUES LESS THAN (2022),
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION p_future VALUES LESS THAN MAXVALUE  -- catch-all
);

-- Partition by month (more granular)
CREATE TABLE logs (
    log_id    BIGINT NOT NULL,
    log_date  DATE NOT NULL,
    message   TEXT,
    PRIMARY KEY (log_id, log_date)
)
PARTITION BY RANGE (TO_DAYS(log_date)) (
    PARTITION p_jan2024 VALUES LESS THAN (TO_DAYS('2024-02-01')),
    PARTITION p_feb2024 VALUES LESS THAN (TO_DAYS('2024-03-01')),
    PARTITION p_mar2024 VALUES LESS THAN (TO_DAYS('2024-04-01')),
    PARTITION p_future  VALUES LESS THAN MAXVALUE
);
```

---

## 🔶 LIST Partitioning

```sql
-- Partition by region
CREATE TABLE sales (
    sale_id INT NOT NULL,
    region  VARCHAR(20) NOT NULL,
    amount  DECIMAL(10,2),
    PRIMARY KEY (sale_id, region)
)
PARTITION BY LIST COLUMNS (region) (
    PARTITION p_south VALUES IN ('Chennai', 'Bangalore', 'Hyderabad', 'Kochi'),
    PARTITION p_north VALUES IN ('Delhi', 'Chandigarh', 'Jaipur', 'Lucknow'),
    PARTITION p_west  VALUES IN ('Mumbai', 'Pune', 'Ahmedabad', 'Surat'),
    PARTITION p_east  VALUES IN ('Kolkata', 'Bhubaneswar', 'Patna')
);
```

---

## 🔵 HASH Partitioning

```sql
-- Distribute evenly across 8 partitions by user_id
CREATE TABLE user_events (
    event_id BIGINT NOT NULL,
    user_id  INT NOT NULL,
    event_type VARCHAR(50),
    PRIMARY KEY (event_id, user_id)
)
PARTITION BY HASH (user_id)
PARTITIONS 8;
```

---

## ⚡ Partition Pruning (The Key Benefit)

MySQL skips partitions that don't match the query condition:

```sql
-- Query only reads p2024 partition (not all 5 partitions!)
SELECT * FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';

-- Verify with EXPLAIN PARTITIONS
EXPLAIN PARTITIONS
SELECT * FROM orders WHERE order_date >= '2024-01-01';
-- partitions: p2024,p_future → only 2 partitions scanned!
```

---

## 🔧 Managing Partitions

```sql
-- Add new partition
ALTER TABLE orders ADD PARTITION (
    PARTITION p2025 VALUES LESS THAN (2026)
);

-- Drop old partition (deletes data!) — instant, no log
ALTER TABLE orders DROP PARTITION p2021;

-- Truncate partition (clear data, keep structure)
ALTER TABLE orders TRUNCATE PARTITION p2021;

-- Reorganize partitions
ALTER TABLE orders REORGANIZE PARTITION p_future INTO (
    PARTITION p2025 VALUES LESS THAN (2026),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);

-- Show partition info
SELECT PARTITION_NAME, TABLE_ROWS, DATA_LENGTH
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME = 'orders';

-- Check which partition a row is in
EXPLAIN PARTITIONS SELECT * FROM orders WHERE order_id = 12345;
```

---

## 🌍 Real-World Use Case — Log Management Pipeline

```sql
-- Web server logs: 500M rows/year, need last 3 months fast
CREATE TABLE web_logs (
    log_id     BIGINT NOT NULL AUTO_INCREMENT,
    log_date   DATE NOT NULL,
    server_id  INT,
    endpoint   VARCHAR(255),
    status_code SMALLINT,
    response_ms INT,
    PRIMARY KEY (log_id, log_date)
)
PARTITION BY RANGE (TO_DAYS(log_date)) (
    PARTITION p202401 VALUES LESS THAN (TO_DAYS('2024-02-01')),
    PARTITION p202402 VALUES LESS THAN (TO_DAYS('2024-03-01')),
    -- ... monthly partitions
    PARTITION p_current VALUES LESS THAN MAXVALUE
);

-- Monthly maintenance: drop old partitions (instant!)
ALTER TABLE web_logs DROP PARTITION p202401;  -- deletes Jan 2024 data instantly!

-- vs DELETE (which would take hours and create huge logs):
-- DELETE FROM web_logs WHERE log_date < '2024-02-01'; -- DON'T DO THIS
```

---

## 💼 Interview Questions

1. **What is partitioning and why use it?**
   > Partitioning divides a large table into smaller physical chunks. Benefits: faster queries via partition pruning, faster deletes (drop partition), better parallel processing.

2. **What is partition pruning?**
   > MySQL automatically skips irrelevant partitions based on query conditions — only scanning partitions that could contain matching data.

3. **What are the partition key constraints?**
   > The partition key must be part of every unique key (including PK) on the table. Functions on columns are allowed for RANGE/LIST.

4. **When would you use partitioning vs indexing?**
   > Use indexing for selective queries on any-sized table. Use partitioning for 100M+ row tables with time-based or category-based access patterns, especially for time-series data lifecycle management (drop old partitions).

5. **Scenario:** *"You have a 2TB log table. Analysts query last 30 days. Admins delete logs older than 1 year. How do you design this?"*
   > Monthly RANGE partition by log date. Analysts get partition pruning for recent queries. Admins drop partitions (instant) instead of slow DELETE.

---

## ⚠️ Common Mistakes

- Forgetting partition key must be in PRIMARY KEY — causes error
- Over-partitioning (too many partitions) — MySQL has per-partition overhead
- Partitioning on low-cardinality column with RANGE — use LIST instead
- Not using partition pruning correctly — verify with EXPLAIN PARTITIONS

---

## 🔗 Related Topics

- [[25 - Indexes]] — Combine with partitioning for best performance
- [[26 - EXPLAIN ANALYZE]] — Verify partition pruning with EXPLAIN PARTITIONS
- [[05 - DDL DML TCL DCL]] — DROP PARTITION is DDL
- [[34 - Data Engineering Projects]] — Real-world partitioning in pipelines
