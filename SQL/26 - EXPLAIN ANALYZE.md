# 26 — EXPLAIN / EXPLAIN ANALYZE

#advanced #performance #interview

---

## 🧠 Introduction

`EXPLAIN` is MySQL's **query execution plan tool** — it shows how MySQL intends to execute your query. `EXPLAIN ANALYZE` (MySQL 8.0.18+) actually runs the query and shows real execution stats.

> EXPLAIN is your X-ray machine for slow queries. Every Data Engineer must know how to read it.

---

## 📐 Syntax

```sql
-- Shows execution plan (doesn't run the query)
EXPLAIN SELECT * FROM orders WHERE user_id = 5;

-- Shows plan with JSON format (more detail)
EXPLAIN FORMAT=JSON SELECT * FROM orders WHERE user_id = 5;

-- Actually runs query and shows real stats (MySQL 8.0.18+)
EXPLAIN ANALYZE SELECT * FROM orders WHERE user_id = 5;

-- Works with UPDATE/DELETE too
EXPLAIN UPDATE employees SET salary = salary * 1.1 WHERE dept = 'Engineering';
```

---

## 📊 Reading EXPLAIN Output

```
+----+-------+--------+------+---------------+-------------+-------+-------+------+-------+
| id | type  | table  | key  | key_len       | ref         | rows  | Extra |
+----+-------+--------+------+---------------+-------------+-------+-------+------+-------+
|  1 | ref   | orders | idx_user_id | 4      | const       |   42  | NULL  |
+----+-------+--------+------+---------------+-------------+-------+-------+------+-------+
```

---

## 🔑 Key Columns Explained

### `type` — Access Method (MOST IMPORTANT)

Best → Worst:

| type | Meaning | Performance |
|------|---------|------------|
| `system` | Single row system table | ⚡ Best |
| `const` | Single row via PK or UNIQUE | ⚡ Excellent |
| `eq_ref` | One row per row from prev table (PK JOIN) | ⚡ Excellent |
| `ref` | Multiple rows via non-unique index | ✅ Good |
| `range` | Index range scan (BETWEEN, IN, >) | ✅ Good |
| `index` | Full index scan | ⚠️ OK |
| `ALL` | Full table scan | 🔴 BAD! |

> **Red flag:** If you see `type: ALL` on a large table — add an index!

### `key` — Index Used

```
key: NULL          → no index used (potential problem!)
key: idx_user_id   → using this index (good)
key: PRIMARY       → using primary key
```

### `rows` — Estimated Rows Scanned

Lower is better. If this equals total table rows → full scan.

### `Extra` — Additional Info

| Extra | Meaning |
|-------|---------|
| `Using index` | Covering index — no table lookup needed ⚡ |
| `Using where` | Post-index filtering in MySQL |
| `Using filesort` | Extra sort operation (add index on ORDER BY column) |
| `Using temporary` | Temp table created (GROUP BY without index) |
| `Using join buffer` | JOIN without index on join column |

---

## 🔍 EXPLAIN Examples — Reading the Output

```sql
-- Example 1: Full table scan (BAD)
EXPLAIN SELECT * FROM orders WHERE YEAR(order_date) = 2024;
-- type: ALL, key: NULL, rows: 1000000 → BAD! Function prevents index use

-- Fix: remove function
EXPLAIN SELECT * FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
-- type: range, key: idx_order_date → GOOD

-- Example 2: Good index usage
EXPLAIN SELECT * FROM orders WHERE user_id = 5;
-- type: ref, key: idx_user_id, rows: 42 → GOOD

-- Example 3: Covering index
EXPLAIN SELECT user_id, status FROM orders WHERE user_id = 5;
-- Extra: Using index → all needed columns in index, no table lookup

-- Example 4: Using filesort (needs attention)
EXPLAIN SELECT * FROM employees ORDER BY salary;
-- Extra: Using filesort → no index on salary, MySQL sorts in memory/disk
-- Fix: CREATE INDEX idx_salary ON employees (salary);
```

---

## 🔬 EXPLAIN ANALYZE — Real Execution Data

```sql
EXPLAIN ANALYZE
SELECT u.name, COUNT(o.order_id) AS order_count
FROM users u
JOIN orders o ON u.user_id = o.user_id
GROUP BY u.user_id, u.name;
```

Output includes:
```
-> Table scan on <temporary>  (cost=...) (actual time=0.001..0.234 rows=15000)
    -> Aggregate using temporary table  (actual time=120.5..120.5)
        -> Nested loop inner join  (actual rows=50000)
            -> Index lookup on o (key=idx_user_id)  (actual time=0.01..0.4)
            -> Single-row index lookup on u using PRIMARY KEY
```

> Key: `actual time=X..Y` where X=first row, Y=last row. High Y values indicate bottlenecks.

---

## 🛠️ Query Optimization Workflow

```sql
-- Step 1: Identify slow query
-- Check slow query log or monitor with:
SHOW PROCESSLIST;
SHOW STATUS LIKE 'Slow_queries';

-- Step 2: Run EXPLAIN
EXPLAIN SELECT ...;

-- Step 3: Check for:
-- - type: ALL → add index
-- - key: NULL → query not using any index
-- - Extra: Using filesort → index on ORDER BY column
-- - Extra: Using temporary → index on GROUP BY column

-- Step 4: Add index
CREATE INDEX idx_xxx ON table (column);

-- Step 5: Re-run EXPLAIN and compare
EXPLAIN SELECT ...;
-- Verify type improved, rows decreased, key is set

-- Step 6: Validate with EXPLAIN ANALYZE
EXPLAIN ANALYZE SELECT ...;
-- Compare actual time before and after
```

---

## 🌍 Real-World Use Case — Slow Query Investigation

```sql
-- Scenario: This query takes 15 seconds on 2M rows
SELECT u.name, SUM(o.amount) AS revenue
FROM users u
JOIN orders o ON u.user_id = o.user_id
WHERE o.order_date >= '2024-01-01'
  AND o.status = 'delivered'
GROUP BY u.user_id, u.name
ORDER BY revenue DESC
LIMIT 10;

-- EXPLAIN shows: type=ALL on orders, Using filesort

-- Fix 1: Index on status + order_date
CREATE INDEX idx_status_date ON orders (status, order_date);

-- Fix 2: Index on user_id for JOIN
CREATE INDEX idx_user_id ON orders (user_id);

-- After indexes: query drops from 15s to 0.05s
```

---

## 💼 Interview Questions

1. **What does `type: ALL` mean in EXPLAIN output?**
   > Full table scan — MySQL reads every row. A red flag for large tables. Fix by adding an appropriate index.

2. **What is `Using filesort` in EXPLAIN Extra?**
   > MySQL performs an extra sorting operation not covered by an index. Add an index on the ORDER BY column(s) to eliminate it.

3. **What is a covering index and how can you spot it in EXPLAIN?**
   > An index containing all columns needed by the query. Identified by `Using index` in EXPLAIN Extra — no table row lookup needed.

4. **What is the difference between EXPLAIN and EXPLAIN ANALYZE?**
   > EXPLAIN shows the estimated plan without running the query. EXPLAIN ANALYZE actually executes the query and shows real timing and row counts.

5. **Scenario:** *"A query is slow on a table with 10M rows. Walk through your optimization steps."*
   > 1. Run EXPLAIN → check type, key, rows. 2. Identify bottleneck (full scan, missing index, filesort). 3. Add appropriate index. 4. Re-EXPLAIN. 5. Use EXPLAIN ANALYZE to compare actual timings.

---

## 🔗 Related Topics

- [[25 - Indexes]] — Creating indexes based on EXPLAIN findings
- [[27 - Partitioning]] — Partitioning for very large tables
- [[22 - Joins]] — Optimizing JOIN performance
- [[09 - WHERE Clause]] — Write WHERE clauses that leverage indexes
