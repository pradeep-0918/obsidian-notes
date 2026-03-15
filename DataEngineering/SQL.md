# 🗄️ SQL for Data Engineering — Master Notes

> **Goal:** Write complex analytical queries for real-world data engineering tasks. **Tags:** #sql #data-engineering #analytics #practice

---

## 📚 Table of Contents

- [[#1. SELECT — The Foundation]]
- [[#2. JOIN — Combining Tables]]
- [[#3. GROUP BY — Aggregations]]
- [[#4. Window Functions — Advanced Analytics]]
- [[#5. Subqueries — Queries Within Queries]]
- [[#6. CTEs — Common Table Expressions]]
- [[#7. Data Cleaning]]
- [[#8. Mini Challenge — Full Analytical Dataset]]

---

## 🧱 Sample Dataset Schema

> Use these tables across all examples below.

```sql
-- Users Table
CREATE TABLE users (
    user_id     INT PRIMARY KEY,
    name        VARCHAR(100),
    email       VARCHAR(100),
    city        VARCHAR(50),
    created_at  DATE
);

-- Orders Table
CREATE TABLE orders (
    order_id    INT PRIMARY KEY,
    user_id     INT,
    amount      DECIMAL(10,2),
    status      VARCHAR(20),  -- 'completed', 'pending', 'cancelled'
    created_at  TIMESTAMP
);

-- Products Table
CREATE TABLE products (
    product_id  INT PRIMARY KEY,
    name        VARCHAR(100),
    category    VARCHAR(50),
    price       DECIMAL(10,2)
);

-- Order Items Table
CREATE TABLE order_items (
    item_id     INT PRIMARY KEY,
    order_id    INT,
    product_id  INT,
    quantity    INT,
    unit_price  DECIMAL(10,2)
);
```

---

## 1. SELECT — The Foundation

> **Concept:** Retrieve, filter, sort, and shape data from tables.

### Basic Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE condition
ORDER BY column1 ASC
LIMIT 10;
```

### Key Clauses

|Clause|Purpose|
|---|---|
|`SELECT`|Choose columns|
|`FROM`|Source table|
|`WHERE`|Filter rows (before aggregation)|
|`ORDER BY`|Sort results|
|`LIMIT`|Cap result rows|
|`DISTINCT`|Remove duplicates|

### Examples

```sql
-- All users from Chennai
SELECT name, email, city
FROM users
WHERE city = 'Chennai'
ORDER BY name ASC;

-- Unique cities in users table
SELECT DISTINCT city
FROM users
ORDER BY city;

-- Orders above ₹5000 in last 30 days
SELECT order_id, user_id, amount
FROM orders
WHERE amount > 5000
  AND created_at >= CURRENT_DATE - INTERVAL '30 days'
ORDER BY amount DESC;

-- Computed columns with aliases
SELECT
    order_id,
    amount,
    amount * 0.18 AS gst_amount,
    amount + (amount * 0.18) AS total_with_gst
FROM orders
WHERE status = 'completed';
```

### CASE WHEN (Conditional Logic)

```sql
SELECT
    order_id,
    amount,
    CASE
        WHEN amount < 500   THEN 'Small'
        WHEN amount < 5000  THEN 'Medium'
        ELSE                     'Large'
    END AS order_size
FROM orders;
```

> 💡 **Tip:** `WHERE` filters rows **before** `SELECT` computes columns. Use `HAVING` to filter after aggregation.

---

## 2. JOIN — Combining Tables

> **Concept:** Merge data from multiple tables using a common key.

### JOIN Types Visual

```
LEFT TABLE (A)     RIGHT TABLE (B)
    ┌──────┐           ┌──────┐
    │  A   │           │  B   │
    │ only ├───────────┤ only │
    │      │  INNER    │      │
    └──────┘           └──────┘

INNER JOIN  → Only matching rows
LEFT JOIN   → All of A + matching B (NULLs if no match)
RIGHT JOIN  → All of B + matching A (NULLs if no match)
FULL JOIN   → All rows from both sides
```

### Syntax

```sql
SELECT a.col1, b.col2
FROM table_a AS a
[INNER | LEFT | RIGHT | FULL] JOIN table_b AS b
    ON a.key = b.key;
```

### Examples

```sql
-- INNER JOIN: Users who have placed orders
SELECT u.name, u.city, o.order_id, o.amount
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id;

-- LEFT JOIN: All users, even those without orders
SELECT u.name, u.city, o.order_id, o.amount
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id;

-- Find users who NEVER placed an order
SELECT u.name, u.email
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
WHERE o.order_id IS NULL;

-- Multi-table JOIN: user → order → items → product
SELECT
    u.name        AS customer,
    p.name        AS product,
    oi.quantity,
    oi.unit_price,
    (oi.quantity * oi.unit_price) AS line_total
FROM users u
JOIN orders o       ON u.user_id   = o.user_id
JOIN order_items oi ON o.order_id  = oi.order_id
JOIN products p     ON oi.product_id = p.product_id
WHERE o.status = 'completed';
```

### SELF JOIN

```sql
-- Find users from the same city (self join)
SELECT a.name AS user1, b.name AS user2, a.city
FROM users a
JOIN users b ON a.city = b.city AND a.user_id < b.user_id;
```

> 💡 **Tip:** Always alias tables in JOINs. Use `IS NULL` after LEFT JOIN to find unmatched rows (anti-join pattern).

---

## 3. GROUP BY — Aggregations

> **Concept:** Collapse rows into groups and apply aggregate functions.

### Aggregate Functions

|Function|Description|
|---|---|
|`COUNT(*)`|Number of rows|
|`SUM(col)`|Total|
|`AVG(col)`|Average|
|`MIN(col)`|Minimum value|
|`MAX(col)`|Maximum value|
|`COUNT(DISTINCT col)`|Unique value count|

### Syntax

```sql
SELECT col, AGG_FUNC(other_col)
FROM table
WHERE condition           -- filters before grouping
GROUP BY col
HAVING AGG_FUNC(...) > n  -- filters after grouping
ORDER BY AGG_FUNC(...) DESC;
```

### Examples

```sql
-- Total revenue and order count per user
SELECT
    u.name,
    COUNT(o.order_id)  AS total_orders,
    SUM(o.amount)      AS total_revenue,
    AVG(o.amount)      AS avg_order_value
FROM users u
JOIN orders o ON u.user_id = o.user_id
WHERE o.status = 'completed'
GROUP BY u.user_id, u.name
ORDER BY total_revenue DESC;

-- Revenue by city (only cities with > ₹10,000)
SELECT
    u.city,
    COUNT(DISTINCT o.order_id) AS orders,
    SUM(o.amount)              AS revenue
FROM users u
JOIN orders o ON u.user_id = o.user_id
GROUP BY u.city
HAVING SUM(o.amount) > 10000
ORDER BY revenue DESC;

-- Monthly order trends
SELECT
    DATE_TRUNC('month', created_at) AS month,
    COUNT(*)                        AS order_count,
    SUM(amount)                     AS monthly_revenue
FROM orders
GROUP BY DATE_TRUNC('month', created_at)
ORDER BY month;

-- Revenue by product category
SELECT
    p.category,
    SUM(oi.quantity * oi.unit_price) AS category_revenue,
    COUNT(DISTINCT o.order_id)       AS orders
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders o   ON oi.order_id   = o.order_id
WHERE o.status = 'completed'
GROUP BY p.category
ORDER BY category_revenue DESC;
```

> 💡 **Tip:** Every column in `SELECT` that is NOT inside an aggregate function **must** appear in `GROUP BY`.

---

## 4. Window Functions — Advanced Analytics

> **Concept:** Perform calculations across related rows **without** collapsing them. Unlike GROUP BY, each row is preserved.

### Syntax

```sql
FUNC() OVER (
    PARTITION BY col     -- group within (like GROUP BY but keeps rows)
    ORDER BY col         -- order within the window
    ROWS/RANGE BETWEEN   -- frame specification (optional)
)
```

### Common Window Functions

|Function|Purpose|
|---|---|
|`ROW_NUMBER()`|Unique sequential row number|
|`RANK()`|Rank with gaps (1,1,3)|
|`DENSE_RANK()`|Rank without gaps (1,1,2)|
|`LAG(col, n)`|Value from n rows before|
|`LEAD(col, n)`|Value from n rows ahead|
|`SUM() OVER (...)`|Running total|
|`AVG() OVER (...)`|Moving average|
|`FIRST_VALUE(col) OVER`|First value in window|
|`LAST_VALUE(col) OVER`|Last value in window|
|`NTILE(n)`|Divide rows into n buckets|
|`PERCENT_RANK()`|Relative rank 0.0–1.0|

### Examples

```sql
-- Rank users by total revenue
SELECT
    u.name,
    SUM(o.amount)                                    AS revenue,
    RANK()       OVER (ORDER BY SUM(o.amount) DESC)  AS revenue_rank,
    DENSE_RANK() OVER (ORDER BY SUM(o.amount) DESC)  AS dense_rank
FROM users u
JOIN orders o ON u.user_id = o.user_id
GROUP BY u.user_id, u.name;

-- Running total of revenue over time
SELECT
    created_at::DATE           AS order_date,
    amount,
    SUM(amount) OVER (
        ORDER BY created_at
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    )                          AS running_total
FROM orders
WHERE status = 'completed';

-- MoM revenue change using LAG
SELECT
    month,
    revenue,
    LAG(revenue) OVER (ORDER BY month)            AS prev_month_revenue,
    revenue - LAG(revenue) OVER (ORDER BY month)  AS change,
    ROUND(
        100.0 * (revenue - LAG(revenue) OVER (ORDER BY month))
        / NULLIF(LAG(revenue) OVER (ORDER BY month), 0),
        2
    )                                             AS pct_change
FROM (
    SELECT
        DATE_TRUNC('month', created_at) AS month,
        SUM(amount)                     AS revenue
    FROM orders
    WHERE status = 'completed'
    GROUP BY 1
) monthly;

-- Top 3 products per category (PARTITION BY)
SELECT *
FROM (
    SELECT
        p.category,
        p.name AS product,
        SUM(oi.quantity * oi.unit_price) AS revenue,
        ROW_NUMBER() OVER (
            PARTITION BY p.category
            ORDER BY SUM(oi.quantity * oi.unit_price) DESC
        ) AS rn
    FROM order_items oi
    JOIN products p ON oi.product_id = p.product_id
    GROUP BY p.category, p.product_id, p.name
) ranked
WHERE rn <= 3;

-- 7-day moving average of revenue
SELECT
    created_at::DATE AS day,
    SUM(amount)      AS daily_revenue,
    AVG(SUM(amount)) OVER (
        ORDER BY created_at::DATE
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    )                AS moving_avg_7d
FROM orders
GROUP BY created_at::DATE;

-- Percentile buckets (quartiles)
SELECT
    user_id,
    total_spend,
    NTILE(4) OVER (ORDER BY total_spend) AS spend_quartile
FROM (
    SELECT user_id, SUM(amount) AS total_spend
    FROM orders
    GROUP BY user_id
) t;
```

> 💡 **Tip:** `PARTITION BY` is like `GROUP BY` inside the window — it resets the calculation for each group without collapsing rows.

---

## 5. Subqueries — Queries Within Queries

> **Concept:** A query nested inside another query. Can appear in `SELECT`, `FROM`, or `WHERE`.

### Types of Subqueries

|Type|Location|Returns|
|---|---|---|
|Scalar|SELECT|Single value|
|Inline view|FROM|Table|
|Correlated|WHERE|Varies|
|EXISTS/IN|WHERE|Boolean/list|

### Examples

```sql
-- Scalar subquery: compare each order to avg
SELECT
    order_id,
    amount,
    (SELECT AVG(amount) FROM orders WHERE status = 'completed') AS avg_order,
    amount - (SELECT AVG(amount) FROM orders WHERE status = 'completed') AS diff_from_avg
FROM orders
WHERE status = 'completed';

-- Inline view (subquery in FROM)
SELECT city, avg_revenue
FROM (
    SELECT
        u.city,
        AVG(o.amount) AS avg_revenue
    FROM users u
    JOIN orders o ON u.user_id = o.user_id
    GROUP BY u.city
) city_stats
WHERE avg_revenue > 2000;

-- IN subquery: users who ordered product in Electronics
SELECT name, email
FROM users
WHERE user_id IN (
    SELECT DISTINCT o.user_id
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p     ON oi.product_id = p.product_id
    WHERE p.category = 'Electronics'
);

-- NOT IN: users who never ordered Electronics
SELECT name, email
FROM users
WHERE user_id NOT IN (
    SELECT DISTINCT o.user_id
    FROM orders o
    JOIN order_items oi ON o.order_id  = oi.order_id
    JOIN products p     ON oi.product_id = p.product_id
    WHERE p.category = 'Electronics'
      AND o.user_id IS NOT NULL
);

-- EXISTS: faster alternative to IN for large datasets
SELECT u.name
FROM users u
WHERE EXISTS (
    SELECT 1
    FROM orders o
    WHERE o.user_id = u.user_id
      AND o.amount > 10000
);

-- Correlated subquery: orders above that user's own average
SELECT order_id, user_id, amount
FROM orders o1
WHERE amount > (
    SELECT AVG(amount)
    FROM orders o2
    WHERE o2.user_id = o1.user_id
);
```

> 💡 **Tip:** Prefer `EXISTS` over `IN` when the subquery returns many rows — it short-circuits on first match.

---

## 6. CTEs — Common Table Expressions

> **Concept:** Named temporary result sets using `WITH`. Makes complex queries readable and reusable within a single query.

### Syntax

```sql
WITH cte_name AS (
    -- your query here
),
another_cte AS (
    -- can reference previous CTEs
    SELECT * FROM cte_name WHERE ...
)
SELECT * FROM another_cte;
```

### Examples

```sql
-- CTE: Monthly revenue with MoM growth
WITH monthly_revenue AS (
    SELECT
        DATE_TRUNC('month', created_at) AS month,
        SUM(amount)                     AS revenue
    FROM orders
    WHERE status = 'completed'
    GROUP BY 1
),
with_growth AS (
    SELECT
        month,
        revenue,
        LAG(revenue) OVER (ORDER BY month) AS prev_revenue
    FROM monthly_revenue
)
SELECT
    month,
    revenue,
    prev_revenue,
    ROUND(100.0 * (revenue - prev_revenue) / NULLIF(prev_revenue, 0), 2) AS growth_pct
FROM with_growth
ORDER BY month;

-- CTE: Multi-step user segmentation
WITH user_spend AS (
    SELECT
        user_id,
        COUNT(order_id)  AS order_count,
        SUM(amount)      AS total_spend
    FROM orders
    WHERE status = 'completed'
    GROUP BY user_id
),
user_segments AS (
    SELECT
        user_id,
        total_spend,
        order_count,
        CASE
            WHEN total_spend >= 50000 THEN 'VIP'
            WHEN total_spend >= 10000 THEN 'Premium'
            WHEN total_spend >= 1000  THEN 'Regular'
            ELSE                           'New'
        END AS segment
    FROM user_spend
)
SELECT
    segment,
    COUNT(*)          AS user_count,
    AVG(total_spend)  AS avg_spend,
    SUM(total_spend)  AS total_revenue
FROM user_segments
GROUP BY segment
ORDER BY avg_spend DESC;

-- Recursive CTE: Generate a date series
WITH RECURSIVE date_series AS (
    SELECT '2024-01-01'::DATE AS dt
    UNION ALL
    SELECT dt + INTERVAL '1 day'
    FROM date_series
    WHERE dt < '2024-12-31'
)
SELECT
    ds.dt            AS date,
    COALESCE(SUM(o.amount), 0) AS revenue
FROM date_series ds
LEFT JOIN orders o ON o.created_at::DATE = ds.dt
                   AND o.status = 'completed'
GROUP BY ds.dt
ORDER BY ds.dt;
```

> 💡 **Tip:** CTEs are executed once and reused — they improve readability AND prevent recomputing the same subquery multiple times.

---

## 7. Data Cleaning

> **Concept:** Fix, standardize, and handle messy data before analysis.

### Common Issues & Fixes

```sql
-- ════════════════════════════════════════════
-- NULL HANDLING
-- ════════════════════════════════════════════

-- Replace NULLs with default value
SELECT
    user_id,
    COALESCE(email, 'no-email@unknown.com') AS email,
    COALESCE(city, 'Unknown')               AS city
FROM users;

-- Filter out rows where critical fields are NULL
SELECT * FROM orders
WHERE amount IS NOT NULL
  AND user_id IS NOT NULL;

-- NULLIF: treat zeros as NULL (avoid division by zero)
SELECT
    order_id,
    amount / NULLIF(quantity, 0) AS unit_price
FROM orders;

-- ════════════════════════════════════════════
-- STRING CLEANING
-- ════════════════════════════════════════════

-- Trim whitespace and standardize case
SELECT
    TRIM(name)        AS name,
    LOWER(TRIM(email)) AS email,
    INITCAP(city)     AS city
FROM users;

-- Remove special characters (PostgreSQL)
SELECT REGEXP_REPLACE(phone, '[^0-9]', '', 'g') AS clean_phone
FROM users;

-- Standardize status values
SELECT
    order_id,
    LOWER(TRIM(status)) AS status
FROM orders;

-- ════════════════════════════════════════════
-- DUPLICATE DETECTION & REMOVAL
-- ════════════════════════════════════════════

-- Find duplicate emails
SELECT email, COUNT(*) AS count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Keep only the latest record per user (dedup)
SELECT *
FROM (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY email ORDER BY created_at DESC) AS rn
    FROM users
) t
WHERE rn = 1;

-- ════════════════════════════════════════════
-- DATE/TYPE CASTING
-- ════════════════════════════════════════════

-- Safe date casting
SELECT
    order_id,
    CAST(created_at AS DATE)              AS order_date,
    TO_CHAR(created_at, 'YYYY-MM')        AS year_month,
    EXTRACT(YEAR FROM created_at)         AS order_year,
    EXTRACT(MONTH FROM created_at)        AS order_month,
    EXTRACT(DOW FROM created_at)          AS day_of_week  -- 0=Sun
FROM orders;

-- ════════════════════════════════════════════
-- OUTLIER DETECTION
-- ════════════════════════════════════════════

-- Flag statistical outliers (beyond 3 std deviations)
WITH stats AS (
    SELECT
        AVG(amount)    AS mean,
        STDDEV(amount) AS stddev
    FROM orders
    WHERE status = 'completed'
)
SELECT
    order_id,
    amount,
    CASE
        WHEN ABS(amount - stats.mean) > 3 * stats.stddev THEN 'Outlier'
        ELSE 'Normal'
    END AS flag
FROM orders, stats
WHERE status = 'completed';

-- ════════════════════════════════════════════
-- VALIDATION CHECKS
-- ════════════════════════════════════════════

-- Data quality report
SELECT
    COUNT(*)                                         AS total_rows,
    COUNT(email)                                     AS non_null_emails,
    COUNT(*) - COUNT(email)                          AS null_emails,
    COUNT(DISTINCT email)                            AS unique_emails,
    COUNT(*) - COUNT(DISTINCT email)                 AS duplicate_emails,
    ROUND(100.0 * COUNT(email) / COUNT(*), 2)        AS email_completeness_pct
FROM users;
```

---

## 8. Mini Challenge — Full Analytical Dataset

> 🎯 **Answer these 4 business questions using SQL:**

### Q1. Top 10 Users by Revenue

```sql
WITH user_revenue AS (
    SELECT
        u.user_id,
        u.name,
        u.city,
        u.email,
        COUNT(o.order_id)  AS total_orders,
        SUM(o.amount)      AS total_revenue,
        AVG(o.amount)      AS avg_order_value,
        MAX(o.created_at)  AS last_order_date
    FROM users u
    JOIN orders o ON u.user_id = o.user_id
    WHERE o.status = 'completed'
    GROUP BY u.user_id, u.name, u.city, u.email
)
SELECT
    RANK() OVER (ORDER BY total_revenue DESC) AS rank,
    name,
    city,
    total_orders,
    total_revenue,
    ROUND(avg_order_value, 2) AS avg_order_value,
    last_order_date
FROM user_revenue
ORDER BY total_revenue DESC
LIMIT 10;
```

---

### Q2. Average Revenue per Order & per User

```sql
WITH order_stats AS (
    SELECT
        u.city,
        COUNT(DISTINCT o.order_id)  AS total_orders,
        COUNT(DISTINCT o.user_id)   AS active_users,
        SUM(o.amount)               AS total_revenue
    FROM users u
    JOIN orders o ON u.user_id = o.user_id
    WHERE o.status = 'completed'
    GROUP BY u.city
)
SELECT
    city,
    total_orders,
    active_users,
    total_revenue,
    ROUND(total_revenue / total_orders, 2) AS avg_revenue_per_order,
    ROUND(total_revenue / active_users, 2)  AS avg_revenue_per_user
FROM order_stats
ORDER BY total_revenue DESC;
```

---

### Q3. Monthly Trends — Revenue & Growth

```sql
WITH monthly AS (
    SELECT
        DATE_TRUNC('month', created_at)::DATE AS month,
        COUNT(order_id)                       AS orders,
        COUNT(DISTINCT user_id)               AS unique_users,
        SUM(amount)                           AS revenue
    FROM orders
    WHERE status = 'completed'
    GROUP BY 1
)
SELECT
    TO_CHAR(month, 'Mon YYYY')                          AS month_label,
    orders,
    unique_users,
    revenue,
    LAG(revenue) OVER (ORDER BY month)                  AS prev_revenue,
    revenue - LAG(revenue) OVER (ORDER BY month)        AS delta,
    ROUND(
        100.0 * (revenue - LAG(revenue) OVER (ORDER BY month))
        / NULLIF(LAG(revenue) OVER (ORDER BY month), 0),
        2
    )                                                   AS mom_growth_pct
FROM monthly
ORDER BY month;
```

---

### Q4. Most Active Locations

```sql
WITH location_metrics AS (
    SELECT
        u.city,
        COUNT(DISTINCT u.user_id)       AS total_users,
        COUNT(DISTINCT o.order_id)      AS total_orders,
        COUNT(DISTINCT
            CASE WHEN o.status = 'completed' THEN o.order_id END
        )                               AS completed_orders,
        SUM(CASE WHEN o.status = 'completed' THEN o.amount ELSE 0 END) AS revenue
    FROM users u
    LEFT JOIN orders o ON u.user_id = o.user_id
    GROUP BY u.city
)
SELECT
    city,
    total_users,
    total_orders,
    completed_orders,
    ROUND(100.0 * completed_orders / NULLIF(total_orders, 0), 1) AS completion_rate_pct,
    revenue,
    ROUND(revenue / NULLIF(completed_orders, 0), 2)              AS avg_order_value,
    DENSE_RANK() OVER (ORDER BY revenue DESC)                    AS revenue_rank,
    DENSE_RANK() OVER (ORDER BY total_orders DESC)               AS activity_rank
FROM location_metrics
ORDER BY revenue DESC;
```

---

## 🧠 Quick Reference Cheat Sheet

```
┌─────────────────────────────────────────────────────────┐
│              SQL EXECUTION ORDER                         │
├─────────────────────────────────────────────────────────┤
│  1. FROM / JOIN     → identify source tables            │
│  2. WHERE           → filter rows                       │
│  3. GROUP BY        → group rows                        │
│  4. HAVING          → filter groups                     │
│  5. SELECT          → compute columns                   │
│  6. DISTINCT        → remove duplicates                 │
│  7. ORDER BY        → sort results                      │
│  8. LIMIT / OFFSET  → paginate results                  │
└─────────────────────────────────────────────────────────┘
```

### Window Function Frame Reference

```sql
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  -- running total
ROWS BETWEEN 6 PRECEDING AND CURRENT ROW          -- 7-row moving window
ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING  -- reverse running total
RANGE BETWEEN INTERVAL '30 days' PRECEDING AND CURRENT ROW  -- date-based
```

### Performance Tips

|Situation|Best Practice|
|---|---|
|Large IN lists|Use `EXISTS` or `JOIN` instead|
|Repeated subquery|Use a CTE|
|Filtering on date column|Index on date col; use `BETWEEN`|
|Aggregation + filtering|Use `HAVING` not `WHERE` after `GROUP BY`|
|Deduplication|`ROW_NUMBER()` + `WHERE rn = 1`|
|Division by zero|Wrap denominator with `NULLIF(col, 0)`|
|Null-safe comparison|Use `IS NULL` / `IS NOT NULL`, not `= NULL`|

---

## 🔗 Related Notes

- [[SQL Indexes & Performance]]
- [[PostgreSQL Functions Reference]]
- [[Data Pipeline Architecture]]
- [[dbt — Data Build Tool]]
- [[Python + SQL with Pandas & SQLAlchemy]]

---

_Last updated: {{date}}_ _Source: SQL for Data Engineering Practice Notes_