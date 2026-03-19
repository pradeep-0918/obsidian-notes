# 25 — Indexes

#advanced #performance #interview

---

## 🧠 Introduction

An index is a **data structure that speeds up data retrieval** — like a book's index that lets you jump to a page without reading every page. Without indexes, MySQL does a full table scan for every query.

> Indexes are the #1 performance tool for SQL — understanding them separates junior from senior engineers.

---

## 🗺️ How Indexes Work

Without index: MySQL reads every row → **O(n)** 
With index (B-Tree): MySQL navigates tree → **O(log n)**

```
Table: 10 million orders
Without index on user_id → reads 10M rows
With index on user_id    → reads ~23 rows (log₂ of 10M ≈ 23)
```

---

## 📐 Types of Indexes

| Type | Description | Use Case |
|------|-------------|---------|
| `PRIMARY KEY` | Auto-created, unique, not null | Row identifier |
| `UNIQUE` | Unique values, allows one NULL | Email, username |
| `INDEX` (B-Tree) | Default index type | General lookups, range queries |
| `FULLTEXT` | For text search | Articles, product descriptions |
| `SPATIAL` | Geographic data | Location-based queries |
| `COMPOSITE` | Multi-column index | Multi-column WHERE/ORDER BY |

---

## 🔧 Creating Indexes

```sql
-- On CREATE TABLE
CREATE TABLE orders (
    order_id  INT PRIMARY KEY AUTO_INCREMENT,
    user_id   INT,
    status    VARCHAR(20),
    amount    DECIMAL(10,2),
    order_date DATE,
    
    INDEX idx_user_id (user_id),               -- single column
    INDEX idx_status_date (status, order_date), -- composite
    UNIQUE INDEX idx_email (email)              -- unique
);

-- On existing table (ALTER TABLE)
ALTER TABLE orders ADD INDEX idx_user_id (user_id);
ALTER TABLE orders ADD UNIQUE INDEX idx_email (email);
ALTER TABLE employees ADD INDEX idx_dept_salary (dept, salary);

-- CREATE INDEX syntax
CREATE INDEX idx_order_date ON orders (order_date);
CREATE UNIQUE INDEX idx_email ON users (email);
CREATE INDEX idx_name_prefix ON users (name(10));  -- prefix index

-- Drop index
DROP INDEX idx_user_id ON orders;
ALTER TABLE orders DROP INDEX idx_user_id;
```

---

## 🔍 SHOW / Check Indexes

```sql
-- Show indexes on a table
SHOW INDEX FROM orders;
SHOW INDEXES FROM employees;

-- Check if query uses index
EXPLAIN SELECT * FROM orders WHERE user_id = 5;
-- Look for 'key' column — shows which index was used
```

---

## 📊 Composite Index — Column Order Matters!

```sql
-- Composite index on (dept, salary)
CREATE INDEX idx_dept_sal ON employees (dept, salary);

-- ✅ Uses index (leftmost prefix rule)
WHERE dept = 'Engineering'                    -- uses idx_dept_sal
WHERE dept = 'Engineering' AND salary > 60000 -- uses idx_dept_sal fully

-- ❌ Does NOT use index (not using leftmost column)
WHERE salary > 60000  -- dept is leftmost; skipping it = no index use
```

> **Leftmost Prefix Rule:** A composite index (A, B, C) can be used for queries on A, or A+B, or A+B+C — but NOT for B alone, C alone, or B+C.

---

## ❌ When Indexes Are NOT Used

```sql
-- Functions on indexed columns BREAK index usage
WHERE UPPER(email) = 'TEST@EMAIL.COM'     -- ❌ no index
WHERE YEAR(order_date) = 2024             -- ❌ no index

-- ✅ Rewrite to use index
WHERE email = LOWER('TEST@EMAIL.COM')     -- ✅ (function on constant)
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31' -- ✅

-- Leading wildcard breaks LIKE index usage
WHERE name LIKE '%Kumar'   -- ❌ full scan
WHERE name LIKE 'Kumar%'   -- ✅ uses index (prefix search)

-- NULL check — IS NULL can use index in MySQL
WHERE manager_id IS NULL   -- ✅ can use index

-- OR conditions may skip index
WHERE dept = 'Eng' OR city = 'Chennai'   -- may not use index on either
-- Consider UNION instead
```

---

## 🔧 Index Strategies for Data Engineers

```sql
-- Covering index: all needed columns are IN the index (no table lookup)
CREATE INDEX idx_covering ON orders (user_id, status, amount, order_date);

-- Query only touches the index, never the table
SELECT user_id, status, amount FROM orders WHERE user_id = 5;
-- All 3 columns in the index → "Using index" in EXPLAIN

-- Partial/Prefix index (saves space)
CREATE INDEX idx_email_prefix ON users (email(20));
-- Only first 20 chars indexed — good if emails are unique in first 20 chars
```

---

## ⚖️ Index Costs and Tradeoffs

| Benefit | Cost |
|---------|------|
| Faster SELECT | Slower INSERT/UPDATE/DELETE |
| | More disk space |
| | Maintenance overhead |

```sql
-- Rule of thumb:
-- Index columns used in: WHERE, JOIN ON, ORDER BY, GROUP BY
-- Don't index: columns with low cardinality (e.g., boolean, gender)
-- Don't over-index: each index slows writes
```

---

## 🌍 Real-World Use Case — Performance Tuning

```sql
-- BEFORE: Query takes 8 seconds on 5M rows
SELECT * FROM orders WHERE user_id = 1234 AND status = 'delivered';

-- Check EXPLAIN
EXPLAIN SELECT * FROM orders WHERE user_id = 1234 AND status = 'delivered';
-- type: ALL → full table scan!

-- Add composite index
CREATE INDEX idx_user_status ON orders (user_id, status);

-- AFTER: Query takes 2ms
EXPLAIN SELECT * FROM orders WHERE user_id = 1234 AND status = 'delivered';
-- type: ref, key: idx_user_status → index used!
```

---

## 💼 Interview Questions

1. **What is an index and why do we use it?**
   > An index is a data structure (usually B-Tree) that allows MySQL to find rows quickly without scanning the entire table. It trades write speed and disk space for read speed.

2. **What is the difference between a PRIMARY KEY and a regular index?**
   > PK is a unique, non-null index that is also the physical row identifier in InnoDB (clustered). Regular indexes are secondary — they contain the indexed value plus the PK to look up the full row.

3. **What is the leftmost prefix rule?**
   > A composite index (A, B, C) can only be used if queries include the leftmost column(s): A, A+B, or A+B+C. Queries on B or C alone won't use the index.

4. **What is a covering index?**
   > An index that contains all columns needed by a query — MySQL can answer the query from the index alone without touching the main table rows.

5. **Scenario:** *"A query runs slowly. How do you diagnose and fix it?"*
   > Run EXPLAIN, look for type=ALL (full scan), check 'key' column. If no index used, add index on the WHERE/JOIN column(s). Use composite index if multiple columns filtered.

---

## 🔗 Related Topics

- [[26 - EXPLAIN ANALYZE]] — Diagnose index usage
- [[09 - WHERE Clause]] — Write WHERE clauses that use indexes
- [[22 - Joins]] — Index FK columns for JOIN performance
- [[27 - Partitioning]] — Partitioning + indexing strategies
- [[25 - Indexes]] → [[08 - ALTER TABLE]] — Adding indexes to existing tables
