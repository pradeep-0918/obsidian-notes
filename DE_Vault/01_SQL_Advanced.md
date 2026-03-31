# 01 — SQL Advanced
#sql #window-functions #cte #indexing #transactions #optimization

**Roadmap Days:** 1–2 | [[DE_Vault/00_Master_Index]] | Next → [[02_Python_ETL]]

---

## 🗺️ Topic Map

```
SQL Advanced
├── Window Functions
├── CTEs (Common Table Expressions)
├── Query Optimization & EXPLAIN
├── Indexing Strategies
├── Transactions & ACID
├── Stored Procedures / Views / Materialized Views
└── Advanced Patterns (SCD, Recursive, Gap Detection)
```

---

## 1. Window Functions

### Core Syntax
```sql
function_name() OVER (
  PARTITION BY col1, col2
  ORDER BY col3
  ROWS BETWEEN start AND end
)
```

### Ranking Functions
| Function | Gaps? | Resets per Partition? |
|----------|-------|-----------------------|
| `ROW_NUMBER()` | No gaps, always unique | Yes |
| `RANK()` | Gaps after ties | Yes |
| `DENSE_RANK()` | No gaps | Yes |
| `NTILE(n)` | Splits into n buckets | Yes |

```sql
-- Top-N per group pattern (interview staple)
SELECT * FROM (
  SELECT *, ROW_NUMBER() OVER (PARTITION BY category ORDER BY revenue DESC) AS rn
  FROM sales
) WHERE rn <= 3;
```

### Value Functions
```sql
LAG(col, offset, default)   -- previous row
LEAD(col, offset, default)  -- next row
FIRST_VALUE(col)            -- first in window
LAST_VALUE(col)             -- last in window (needs frame!)
NTH_VALUE(col, n)           -- nth row in window
```

### Aggregate Window Functions
```sql
SUM(amount) OVER (PARTITION BY user_id ORDER BY date 
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)  -- running total

AVG(score) OVER (ORDER BY date 
  ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)          -- 7-day rolling avg
```

### Frame Clauses
| Frame | Meaning |
|-------|---------|
| `ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW` | Running total |
| `ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING` | 3-row sliding window |
| `RANGE BETWEEN INTERVAL '7 days' PRECEDING AND CURRENT ROW` | Time-based range |
| `ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING` | Reverse running total |

> ⚠️ **Hidden Trap:** `LAST_VALUE` without `ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` returns current row value, not the actual last. Always specify frame for `LAST_VALUE`.

> 💡 **RANGE vs ROWS:** `RANGE` groups duplicate ORDER BY values together. `ROWS` treats each row individually. For running totals with ties, `RANGE` can give different results than expected.

---

## 2. CTEs (Common Table Expressions)

### Simple CTE
```sql
WITH monthly_revenue AS (
  SELECT user_id, DATE_TRUNC('month', created_at) AS month, SUM(amount) AS revenue
  FROM orders GROUP BY 1, 2
)
SELECT * FROM monthly_revenue WHERE revenue > 1000;
```

### Multiple CTEs (chained)
```sql
WITH
  raw AS (SELECT ...),
  cleaned AS (SELECT ... FROM raw WHERE ...),
  aggregated AS (SELECT ... FROM cleaned GROUP BY ...)
SELECT * FROM aggregated;
```

### Recursive CTE (org hierarchy, sequences)
```sql
WITH RECURSIVE org_tree AS (
  -- Base case: root nodes
  SELECT id, name, manager_id, 0 AS depth
  FROM employees WHERE manager_id IS NULL

  UNION ALL

  -- Recursive case
  SELECT e.id, e.name, e.manager_id, ot.depth + 1
  FROM employees e
  JOIN org_tree ot ON e.manager_id = ot.id
)
SELECT * FROM org_tree ORDER BY depth;
```

> 💡 **Recursive CTE Use Cases:** org charts, BOM (bill of materials), graph traversal, generating date series, Fibonacci numbers.

> ⚠️ **Termination:** Always ensure base case is finite. PostgreSQL has `max_recursion_depth = 100` by default. Add `WHERE depth < 10` as safety.

### Correlated Subqueries
```sql
-- Find employees earning more than their department average
SELECT name, salary, department_id
FROM employees e
WHERE salary > (
  SELECT AVG(salary) FROM employees 
  WHERE department_id = e.department_id  -- correlated: references outer query
);
```

> ⚠️ **Performance:** Correlated subqueries run once per outer row = O(n²). Prefer window functions or JOINs where possible.

---

## 3. Query Optimization & EXPLAIN

### EXPLAIN Anatomy (PostgreSQL)
```sql
EXPLAIN ANALYZE SELECT ...;
```

Key nodes to understand:
| Node | Meaning | Watch For |
|------|---------|-----------|
| `Seq Scan` | Full table scan | High rows, add index |
| `Index Scan` | Uses index | Good for selective filters |
| `Index Only Scan` | Covers all cols in index | Best case |
| `Hash Join` | Build hash table from smaller table | Memory usage |
| `Merge Join` | Both sides sorted | Good on sorted data |
| `Nested Loop` | Outer × inner iteration | Bad on large tables |
| `Sort` | Explicit sort | Can be expensive |
| `Gather` | Parallel query merge point | |

> 💡 **Cost Format:** `(cost=startup..total rows=X width=Y)`. Lower total cost = better. Actual vs estimated rows divergence = stale stats → run `ANALYZE`.

### Statistics & Planner
```sql
-- Update table statistics (run after bulk load)
ANALYZE table_name;

-- See planner stats
SELECT * FROM pg_stats WHERE tablename = 'orders';

-- Force/hint index (PostgreSQL doesn't support hints natively)
SET enable_seqscan = OFF;  -- dev/debug only
```

---

## 4. Indexing Strategies

### Index Types
| Type | Use Case |
|------|---------|
| B-Tree (default) | Equality, range, ORDER BY, LIKE 'prefix%' |
| Hash | Equality only, faster than B-Tree for `=` |
| GIN | Full-text search, JSONB, arrays |
| BRIN | Time-series, append-only large tables (tiny index) |
| Partial | Filtered index — `WHERE active = true` |
| Composite | Multi-column — order matters! |
| Covering | Include extra columns — `INCLUDE (col)` |
| Expression | Index on expression — `LOWER(email)` |

### Composite Index Rule
```sql
-- Index (a, b, c) is useful for:
-- WHERE a = ?            ✅
-- WHERE a = ? AND b = ?  ✅
-- WHERE a = ? AND b = ? AND c = ?  ✅
-- WHERE b = ?            ❌ (skips leading column)
-- WHERE a = ? AND c = ?  ✅ for a, ❌ for c (partial use)
```

> 💡 **Covering Index:** Add `INCLUDE (amount)` to avoid table heap lookup — creates an "index only scan".

> ⚠️ **Index Bloat:** Dead tuples cause index bloat. Run `VACUUM` regularly, monitor with `pg_stat_user_tables`.

---

## 5. Transactions & ACID

### ACID Properties
| Property | Meaning |
|----------|---------|
| **A**tomicity | All or nothing — no partial commits |
| **C**onsistency | Data moves from valid state to valid state |
| **I**solation | Concurrent transactions don't interfere |
| **D**urability | Committed data survives crashes (WAL) |

### Isolation Levels & Read Phenomena
| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|-------|-----------|---------------------|--------------|
| READ UNCOMMITTED | ✅ possible | ✅ | ✅ |
| READ COMMITTED (default PG) | ❌ | ✅ | ✅ |
| REPEATABLE READ | ❌ | ❌ | ✅ (in most DBs) |
| SERIALIZABLE | ❌ | ❌ | ❌ |

```sql
BEGIN;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
-- operations
COMMIT;
```

> 💡 **PostgreSQL's REPEATABLE READ** also prevents phantom reads (unlike SQL standard). SERIALIZABLE uses SSI (Serializable Snapshot Isolation) — no locking, uses conflict detection.

### Locking
```sql
SELECT ... FOR UPDATE;           -- row-level exclusive lock
SELECT ... FOR SHARE;            -- row-level shared lock
SELECT ... FOR UPDATE SKIP LOCKED; -- skip locked rows (queue processing!)
LOCK TABLE t IN EXCLUSIVE MODE;  -- table-level lock
```

> 💡 **`SKIP LOCKED` Pattern:** Ideal for job queues — multiple workers can pull different rows without blocking each other.

---

## 6. Stored Procedures, Views, Materialized Views

### Views
```sql
CREATE VIEW active_users AS
SELECT * FROM users WHERE active = true;
-- Updatable if single table, no aggregations
```

### Materialized Views
```sql
CREATE MATERIALIZED VIEW monthly_summary AS
SELECT date_trunc('month', created_at), COUNT(*), SUM(amount)
FROM orders GROUP BY 1;

REFRESH MATERIALIZED VIEW CONCURRENTLY monthly_summary;
-- CONCURRENTLY: no lock during refresh, needs unique index
```

> 💡 **When to use Materialized Views:** Heavy aggregation queries (dashboards), reused subqueries, reporting. Cost: refresh lag + storage.

### Stored Procedures vs Functions
| | Function | Procedure |
|--|---------|-----------|
| Returns value | Yes | Optional (OUT params) |
| Can COMMIT/ROLLBACK | No | Yes (PostgreSQL 11+) |
| Use in SELECT | Yes | No |
| Side effects | Technically no | Yes |

---

## 7. Advanced SQL Patterns

### SCD Type 2 MERGE
```sql
MERGE INTO dim_customer AS target
USING staging_customer AS source ON target.customer_id = source.customer_id
WHEN MATCHED AND target.is_current = TRUE 
  AND (target.email != source.email OR target.address != source.address)
THEN UPDATE SET
  target.valid_to = CURRENT_DATE - 1,
  target.is_current = FALSE
WHEN NOT MATCHED THEN INSERT 
  (customer_id, email, address, valid_from, valid_to, is_current)
  VALUES (source.customer_id, source.email, source.address, CURRENT_DATE, '9999-12-31', TRUE);
-- Then insert new version for changed rows
```

### Gap Detection (LAG/LEAD)
```sql
-- Find gaps in sequential IDs
SELECT id + 1 AS gap_start, 
       next_id - 1 AS gap_end
FROM (
  SELECT id, LEAD(id) OVER (ORDER BY id) AS next_id FROM orders
) t
WHERE next_id > id + 1;
```

### Running Total & Moving Avg
```sql
SELECT date,
  SUM(revenue) OVER (ORDER BY date) AS running_total,
  AVG(revenue) OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW) AS rolling_7d_avg
FROM daily_revenue;
```

---

## 🔗 Resources
- [LeetCode SQL Problems](https://leetcode.com/problemset/database/)
- [PostgreSQL Window Functions Docs](https://www.postgresql.org/docs/current/tutorial-window.html)
- [Use The Index, Luke](https://use-the-index-luke.com/)
- [Mode SQL Tutorial](https://mode.com/sql-tutorial/sql-window-functions/)

---

## 🃏 Interview Cheatsheet
- **Window vs GROUP BY:** Window functions don't reduce rows; GROUP BY does.
- **When does `RANK()` skip numbers?** When there are ties — two rows ranked 2 gives next rank 4.
- **What is a covering index?** An index that contains all columns needed by a query, avoiding table heap access.
- **What is a phantom read?** Two reads in same transaction return different sets due to another transaction inserting rows.
- **VACUUM vs ANALYZE:** VACUUM reclaims dead tuples; ANALYZE updates statistics for the query planner.
