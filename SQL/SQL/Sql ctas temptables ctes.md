# SQL — CTAS, Temporary Tables & CTEs

#sql #learning #database #ctas #temp-tables #cte

---

## 📌 Overview

This note covers three powerful SQL techniques for managing intermediate data within queries and sessions:

- **CTAS** — Create Table As Select
- **Temporary Tables** — Session-scoped tables
- **CTEs** — Common Table Expressions (used with both above)

---

## 1. CTAS — Create Table As Select

> [!info] Definition
> CTAS creates a **new permanent table** by selecting data from an existing table or query. The structure and data are both derived from the `SELECT` statement.

### Syntax

```sql
CREATE TABLE new_table AS
SELECT column1, column2, ...
FROM source_table
WHERE condition;
```

### Example

```sql
-- Create a summary table of high-value orders
CREATE TABLE high_value_orders AS
SELECT
    order_id,
    customer_id,
    total_amount,
    order_date
FROM orders
WHERE total_amount > 1000;
```

### Key Properties

| Property | Detail |
|---|---|
| **Persistence** | Permanent — survives session end |
| **Indexes** | NOT copied from source; must be added manually |
| **Constraints** | NOT copied (PKs, FKs, NOT NULL not inherited) |
| **Data** | Copied at time of creation (snapshot) |

### Use Cases

- Archiving a filtered subset of data
- Creating denormalised reporting tables
- Snapshotting data for performance testing
- Migrating/restructuring table schemas

### ⚠️ Gotchas

> [!warning] Watch Out
> - The new table has **no indexes** — add them explicitly after creation
> - CTAS creates a **snapshot**, not a live view; data won't auto-update
> - Some databases (e.g., PostgreSQL) require `CREATE TABLE ... AS` while others vary slightly

---

## 2. Temporary Tables

> [!info] Definition
> A **temporary table** exists only for the duration of a **session** (or transaction, depending on the DB). It is automatically dropped when the session ends.

### Syntax

```sql
-- Method 1: CREATE TEMP TABLE
CREATE TEMP TABLE temp_table_name (
    column1 datatype,
    column2 datatype
);

-- Method 2: CTAS style (temp)
CREATE TEMP TABLE temp_table_name AS
SELECT column1, column2
FROM source_table
WHERE condition;
```

### Example

```sql
-- Temp table for intermediate calculations
CREATE TEMP TABLE customer_totals AS
SELECT
    customer_id,
    SUM(total_amount) AS lifetime_value,
    COUNT(order_id)   AS order_count
FROM orders
GROUP BY customer_id;

-- Now use it in a further query
SELECT *
FROM customer_totals
WHERE lifetime_value > 5000;
```

### Key Properties

| Property | Detail |
|---|---|
| **Persistence** | Dropped at end of session (or transaction) |
| **Visibility** | Only visible to the creating session |
| **Indexes** | Can be added manually |
| **Name conflicts** | Temp tables can shadow permanent tables of same name |

### Dialect Differences

| Database | Syntax |
|---|---|
| PostgreSQL | `CREATE TEMP TABLE` or `CREATE TEMPORARY TABLE` |
| MySQL | `CREATE TEMPORARY TABLE` |
| SQL Server | `CREATE TABLE #table_name` (single `#`) |
| SQLite | `CREATE TEMP TABLE` |
| BigQuery | `CREATE TEMP TABLE` (within scripts) |

### Use Cases

- Breaking a complex query into readable steps
- Storing intermediate results to avoid repeating expensive joins
- Staging data for multi-step transformations
- Iterative / procedural logic in stored procedures

### ⚠️ Gotchas

> [!warning] Watch Out
> - Temp tables **consume real storage** (usually in `tempdb` or equivalent)
> - Avoid reusing temp table names in the same session without `DROP TABLE` first
> - Not suitable for use across multiple sessions or users

---

## 3. CTEs — Common Table Expressions

> [!info] Definition
> A CTE is a **named, temporary result set** defined within a single SQL statement using the `WITH` clause. It exists only for the duration of that query.

### Syntax

```sql
WITH cte_name AS (
    SELECT column1, column2
    FROM source_table
    WHERE condition
)
SELECT *
FROM cte_name;
```

### Multiple CTEs

```sql
WITH
cte_one AS (
    SELECT ...
    FROM table_a
),
cte_two AS (
    SELECT ...
    FROM table_b
    JOIN cte_one ON ...   -- CTEs can reference earlier CTEs
)
SELECT *
FROM cte_two;
```

### Example — Step-by-step breakdown

```sql
WITH
-- Step 1: Filter recent orders
recent_orders AS (
    SELECT order_id, customer_id, total_amount
    FROM orders
    WHERE order_date >= '2024-01-01'
),
-- Step 2: Aggregate per customer
customer_summary AS (
    SELECT
        customer_id,
        COUNT(order_id)        AS num_orders,
        SUM(total_amount)      AS total_spent
    FROM recent_orders
    GROUP BY customer_id
)
-- Step 3: Final output
SELECT
    c.customer_name,
    cs.num_orders,
    cs.total_spent
FROM customer_summary cs
JOIN customers c ON c.customer_id = cs.customer_id
ORDER BY cs.total_spent DESC;
```

### Recursive CTEs

> [!tip] Advanced Use
> CTEs can be **recursive** — useful for hierarchical data (org charts, category trees, graph traversal).

```sql
WITH RECURSIVE org_chart AS (
    -- Anchor: start at top-level employees
    SELECT employee_id, name, manager_id, 1 AS level
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive: find direct reports
    SELECT e.employee_id, e.name, e.manager_id, oc.level + 1
    FROM employees e
    JOIN org_chart oc ON e.manager_id = oc.employee_id
)
SELECT * FROM org_chart ORDER BY level;
```

### Key Properties

| Property | Detail |
|---|---|
| **Persistence** | Lives only within the single SQL statement |
| **Reusability** | Can be referenced **multiple times** within the same query |
| **Nesting** | CTEs can reference previously defined CTEs |
| **Recursion** | Supported with `RECURSIVE` keyword (most databases) |

---

## 4. Comparison — When to Use What?

| Feature | CTAS | Temp Table | CTE |
|---|---|---|---|
| **Scope** | Permanent table | Session | Single query |
| **Persists after query?** | ✅ Yes | ✅ Until session ends | ❌ No |
| **Can be indexed?** | ✅ Yes | ✅ Yes | ❌ No |
| **Reusable across queries?** | ✅ Yes | ✅ Yes (same session) | ❌ No |
| **Materialized?** | ✅ Yes | ✅ Yes | Depends on DB |
| **Supports recursion?** | ❌ No | ❌ No | ✅ Yes |
| **Best for** | Archiving, reporting | Multi-step transforms | Readable, complex queries |

---

## 5. CTE + CTAS & CTE + Temp Table Patterns

### CTE → CTAS

```sql
-- Use a CTE to build the logic, then persist as a table
CREATE TABLE monthly_revenue AS
WITH monthly AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(total_amount)               AS revenue
    FROM orders
    GROUP BY 1
)
SELECT * FROM monthly;
```

### CTE → Temp Table

```sql
-- Use a CTE to build the logic, then store temporarily
CREATE TEMP TABLE top_customers AS
WITH ranked AS (
    SELECT
        customer_id,
        SUM(total_amount) AS total_spent,
        RANK() OVER (ORDER BY SUM(total_amount) DESC) AS rnk
    FROM orders
    GROUP BY customer_id
)
SELECT customer_id, total_spent
FROM ranked
WHERE rnk <= 100;
```

---

## 6. Quick Reference Cheatsheet

```sql
-- ── CTAS ──────────────────────────────────────────────
CREATE TABLE new_table AS
SELECT * FROM source WHERE condition;

-- ── TEMP TABLE (explicit schema) ──────────────────────
CREATE TEMP TABLE t (id INT, name TEXT);
INSERT INTO t SELECT id, name FROM source;

-- ── TEMP TABLE (CTAS style) ───────────────────────────
CREATE TEMP TABLE t AS
SELECT * FROM source WHERE condition;

-- ── CTE (single) ──────────────────────────────────────
WITH cte AS (SELECT * FROM source)
SELECT * FROM cte;

-- ── CTE (chained) ─────────────────────────────────────
WITH
step1 AS (SELECT ...),
step2 AS (SELECT ... FROM step1)
SELECT * FROM step2;

-- ── CTE + CTAS ────────────────────────────────────────
CREATE TABLE result AS
WITH cte AS (SELECT ...)
SELECT * FROM cte;

-- ── CTE + TEMP ────────────────────────────────────────
CREATE TEMP TABLE result AS
WITH cte AS (SELECT ...)
SELECT * FROM cte;

-- ── RECURSIVE CTE ─────────────────────────────────────
WITH RECURSIVE cte AS (
    SELECT ... -- anchor
    UNION ALL
    SELECT ... FROM table JOIN cte ON ... -- recursive
)
SELECT * FROM cte;
```

---

## 7. Related Notes

- [[SQL Basics]]
- [[SQL Window Functions]]
- [[SQL Indexes and Performance]]
- [[SQL Joins]]

---

*Tags: #sql #ctas #temp-tables #cte #learning*