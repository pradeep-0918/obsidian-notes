# 38 — Query Optimization Patterns

#advanced #performance #interview

---

## 🧠 Introduction

Query optimization is what separates good Data Engineers from great ones. A query that takes 60 seconds vs 0.06 seconds means the difference between a usable product and an unusable one.

> Rule 1: Measure first. Never optimize without `EXPLAIN`. Never guess.

---

## 🔍 Optimization Workflow

```
1. Identify slow query (slow log / SHOW PROCESSLIST)
         ↓
2. Run EXPLAIN (check type, key, rows)
         ↓
3. Identify bottleneck (full scan, no index, filesort)
         ↓
4. Apply fix (add index, rewrite query, add partition)
         ↓
5. Re-run EXPLAIN → verify improvement
         ↓
6. Benchmark with EXPLAIN ANALYZE (actual timing)
```

---

## ❌ Anti-Patterns & Fixes

### Anti-Pattern 1: Function on Indexed Column

```sql
-- ❌ SLOW: wrapping indexed column breaks index usage
SELECT * FROM orders WHERE YEAR(order_date) = 2024;
SELECT * FROM users  WHERE UPPER(email) = 'TEST@EMAIL.COM';
SELECT * FROM users  WHERE MONTH(created_at) = 7;

-- ✅ FAST: use range instead
SELECT * FROM orders WHERE order_date >= '2024-01-01' AND order_date < '2025-01-01';
SELECT * FROM users  WHERE email = LOWER('TEST@EMAIL.COM');  -- function on constant, not column
SELECT * FROM users  WHERE created_at >= '2024-07-01' AND created_at < '2024-08-01';
```

---

### Anti-Pattern 2: SELECT * in Production

```sql
-- ❌ SLOW: fetches all columns (more I/O, prevents covering indexes)
SELECT * FROM orders WHERE user_id = 5;

-- ✅ FAST: fetch only needed columns
SELECT order_id, amount, status FROM orders WHERE user_id = 5;

-- ✅ EVEN FASTER: create covering index
CREATE INDEX idx_user_orders ON orders (user_id, order_id, amount, status);
-- EXPLAIN shows "Using index" → no table row lookup!
```

---

### Anti-Pattern 3: Correlated Subquery Instead of JOIN

```sql
-- ❌ SLOW: correlated subquery runs once per outer row
SELECT
    name,
    (SELECT dept_name FROM departments WHERE dept_id = e.dept_id) AS dept  -- runs N times!
FROM employees e;

-- ✅ FAST: JOIN runs once
SELECT e.name, d.dept_name
FROM employees e
JOIN departments d ON e.dept_id = d.dept_id;
```

---

### Anti-Pattern 4: NOT IN with Subquery (Especially with NULLs)

```sql
-- ❌ SLOW + DANGEROUS: NOT IN materializes full subquery; NULL causes empty result
SELECT * FROM orders
WHERE user_id NOT IN (SELECT user_id FROM blacklisted_users);

-- ✅ FAST + SAFE: LEFT JOIN anti-pattern
SELECT o.*
FROM orders o
LEFT JOIN blacklisted_users b ON o.user_id = b.user_id
WHERE b.user_id IS NULL;

-- ✅ ALSO GOOD: NOT EXISTS (short-circuits)
SELECT * FROM orders o
WHERE NOT EXISTS (SELECT 1 FROM blacklisted_users WHERE user_id = o.user_id);
```

---

### Anti-Pattern 5: OR on Different Columns

```sql
-- ❌ May skip indexes: OR across different columns
SELECT * FROM users WHERE city = 'Chennai' OR email LIKE '%@gmail.com';

-- ✅ UNION splits into two indexed queries
SELECT * FROM users WHERE city = 'Chennai'
UNION
SELECT * FROM users WHERE email LIKE '%@gmail.com';
```

---

### Anti-Pattern 6: Implicit Type Conversion

```sql
-- ❌ Column is INT, comparing to string → full scan
SELECT * FROM orders WHERE order_id = '12345';  -- order_id is INT

-- ✅ Match types exactly
SELECT * FROM orders WHERE order_id = 12345;

-- ❌ Date stored as VARCHAR, comparing with DATE function
SELECT * FROM orders WHERE STR_TO_DATE(order_date_str, '%d/%m/%Y') = '2024-07-15';

-- ✅ Store dates as DATE type; query with proper type
SELECT * FROM orders WHERE order_date = '2024-07-15';
```

---

### Anti-Pattern 7: Missing Index on JOIN Column

```sql
-- ❌ SLOW: joining on non-indexed column
SELECT u.name, o.amount
FROM users u JOIN orders o ON u.email = o.billing_email;  -- email not indexed!

-- ✅ Add index to both sides of the join
CREATE INDEX idx_user_email      ON users (email);
CREATE INDEX idx_order_bill_email ON orders (billing_email);
```

---

### Anti-Pattern 8: Large IN() Lists

```sql
-- ❌ Large IN list is slow
SELECT * FROM products WHERE product_id IN (1,2,3,...,10000);

-- ✅ Use temp table or JOIN
CREATE TEMPORARY TABLE ids (id INT PRIMARY KEY);
INSERT INTO ids VALUES (1),(2),(3),...;
SELECT p.* FROM products p JOIN ids ON p.product_id = ids.id;
```

---

## ⚡ Index Optimization Strategies

### Strategy 1: Composite Index Column Order

```sql
-- Rule: Equality conditions first, range conditions last
-- WHERE dept = 'Eng' AND salary BETWEEN 50000 AND 90000

-- ✅ CORRECT order: equality (dept) first, range (salary) last
CREATE INDEX idx_dept_sal ON employees (dept, salary);

-- ❌ WRONG order: range (salary) first blocks dept from being used in range
CREATE INDEX idx_sal_dept ON employees (salary, dept);
```

### Strategy 2: Index for ORDER BY

```sql
-- Avoid "Using filesort" by indexing ORDER BY column
-- If your query is: WHERE status = 'active' ORDER BY created_at DESC

CREATE INDEX idx_status_date ON orders (status, created_at);
-- Now: WHERE status = 'active' ORDER BY created_at DESC → no filesort!
```

### Strategy 3: Covering Index Design

```sql
-- Query: SELECT user_id, status, amount FROM orders WHERE user_id = 5
-- Create index that covers ALL columns in the query
CREATE INDEX idx_covering ON orders (user_id, status, amount);
-- EXPLAIN Extra: "Using index" → zero table row lookups!
```

---

## 📊 Query Rewriting Techniques

### Technique 1: EXISTS instead of COUNT

```sql
-- ❌ Counts all matching rows (unnecessary work)
SELECT * FROM users WHERE (SELECT COUNT(*) FROM orders WHERE user_id = u.user_id) > 0;

-- ✅ Stops at first match
SELECT * FROM users u WHERE EXISTS (SELECT 1 FROM orders WHERE user_id = u.user_id);
```

### Technique 2: Pagination Optimization (Keyset vs OFFSET)

```sql
-- ❌ SLOW on page 10,000: OFFSET 100000 still scans 100,000 rows
SELECT * FROM orders ORDER BY order_id LIMIT 20 OFFSET 100000;

-- ✅ FAST: Keyset pagination (cursor-based)
SELECT * FROM orders
WHERE order_id > 100000       -- last seen id from previous page
ORDER BY order_id
LIMIT 20;
```

### Technique 3: Batch Large Operations

```sql
-- ❌ SLOW + LOCKS: Delete 10M rows at once
DELETE FROM logs WHERE log_date < '2023-01-01';

-- ✅ FAST: Delete in small batches
SET @deleted = 1;
WHILE @deleted > 0 DO
    DELETE FROM logs WHERE log_date < '2023-01-01' LIMIT 1000;
    SET @deleted = ROW_COUNT();
    DO SLEEP(0.1);   -- yield briefly to avoid lock contention
END WHILE;
-- OR: Drop partition if partitioned (instant!)
ALTER TABLE logs DROP PARTITION p2022;
```

### Technique 4: Avoid DISTINCT When JOIN Causes Duplicates

```sql
-- ❌ Using DISTINCT to hide JOIN duplication problem
SELECT DISTINCT u.user_id, u.name
FROM users u JOIN orders o ON u.user_id = o.user_id;

-- ✅ Use EXISTS (cleaner intent, no duplicates)
SELECT user_id, name FROM users u
WHERE EXISTS (SELECT 1 FROM orders WHERE user_id = u.user_id);
```

---

## 📈 MySQL Configuration Tips (for Data Engineers)

```sql
-- Check key performance variables
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';  -- should be 70-80% of RAM
SHOW VARIABLES LIKE 'query_cache%';             -- query cache (deprecated in 8.0)
SHOW VARIABLES LIKE 'sort_buffer_size';
SHOW VARIABLES LIKE 'join_buffer_size';

-- Slow query log
SHOW VARIABLES LIKE 'slow_query_log';
SHOW VARIABLES LIKE 'long_query_time';

-- Enable slow query log
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 1;   -- log queries > 1 second
```

---

## 💼 Interview Questions

1. **A query takes 30 seconds. Walk me through optimization.**
   > 1. EXPLAIN → find type=ALL or key=NULL. 2. Identify bottleneck. 3. Add index on WHERE/JOIN columns. 4. Rewrite anti-patterns (functions on columns, correlated subqueries). 5. EXPLAIN ANALYZE to measure improvement.

2. **Why does wrapping a column in a function break index usage?**
   > Indexes store column values, not computed values. `YEAR(date_col)` computes a new value — MySQL can't look that up in the index without scanning all rows.

3. **What is the difference between index scan and full table scan?**
   > Index scan (type=index) reads the entire index in order — better than full table scan but still reads everything. Range scan (type=range) reads a portion of the index. Type=ALL reads the entire table.

4. **When would you choose NOT EXISTS over NOT IN?**
   > NOT EXISTS handles NULLs correctly and short-circuits. NOT IN with NULLs in the subquery returns an empty result set.

5. **What is keyset pagination and why is it faster than OFFSET?**
   > Keyset (cursor-based) filters with `WHERE id > last_seen_id`. OFFSET scans and discards all previous rows — gets slower as page number increases.

---

## 🔗 Related Topics

- [[25 - Indexes]] — Indexing fundamentals
- [[26 - EXPLAIN ANALYZE]] — Measuring performance
- [[27 - Partitioning]] — Large table optimization
- [[20 - Subqueries]] — Rewriting correlated subqueries
- [[22 - Joins]] — JOIN optimization
- [[09 - WHERE Clause]] — Writing index-friendly WHERE clauses
