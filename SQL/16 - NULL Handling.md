# 16 — NULL Handling

#beginner #interview #gotcha

---

## 🧠 Introduction

`NULL` in SQL means **unknown, missing, or not applicable** — it is NOT zero, NOT empty string, NOT false. NULL has special rules that trip up even experienced developers.

> NULL is not a value — it's the absence of a value. This distinction matters in every comparison.

---

## ⚠️ NULL Comparison Rules

```sql
-- ❌ These NEVER work as expected
WHERE column = NULL      -- always false!
WHERE column != NULL     -- always false!
WHERE column = ''        -- checks for empty string, not NULL

-- ✅ Correct NULL checks
WHERE column IS NULL
WHERE column IS NOT NULL
```

---

## 🔧 NULL Functions

### IFNULL — Replace NULL with default

```sql
-- Replace NULL salary with 0
SELECT name, IFNULL(salary, 0) AS salary FROM employees;

-- Replace NULL with message
SELECT name, IFNULL(phone, 'N/A') AS phone FROM users;
```

### COALESCE — Return first non-NULL value

```sql
-- Try phone1, then phone2, then email, then 'No Contact'
SELECT 
    name,
    COALESCE(phone_primary, phone_secondary, email, 'No Contact') AS contact
FROM customers;

-- Great for data quality in pipelines
SELECT 
    COALESCE(actual_amount, estimated_amount, 0) AS revenue
FROM transactions;
```

### NULLIF — Return NULL if values match

```sql
-- Avoid division by zero
SELECT 
    total_revenue / NULLIF(total_orders, 0) AS avg_order_value
FROM daily_stats;
-- If total_orders = 0, returns NULL instead of dividing by zero

-- Replace empty string with NULL
SELECT NULLIF(phone_number, '') AS phone FROM users;
```

### ISNULL — Returns 1 if NULL (MySQL)

```sql
SELECT name, ISNULL(manager_id) AS is_top_manager FROM employees;
-- Returns 1 for top-level managers (no manager), 0 for others
```

---

## 📊 NULL in Arithmetic

```sql
-- NULL + anything = NULL
SELECT 100 + NULL;  -- NULL

-- NULL in expressions
SELECT salary + bonus AS total_comp FROM employees;
-- If bonus is NULL → total_comp is NULL for that row!

-- Fix with COALESCE
SELECT salary + COALESCE(bonus, 0) AS total_comp FROM employees;
```

---

## 📋 NULL in Aggregations

```sql
-- COUNT(*) counts all rows including NULLs
-- COUNT(column) ignores NULLs
SELECT 
    COUNT(*)        AS total_employees,
    COUNT(salary)   AS employees_with_salary,  -- excludes NULLs
    AVG(salary)     AS avg_salary  -- AVG also ignores NULLs
FROM employees;
```

---

## 🔴 NULL with NOT IN — Critical Gotcha

```sql
-- ❌ DANGEROUS: Returns empty set if subquery has any NULL
SELECT * FROM orders
WHERE user_id NOT IN (SELECT user_id FROM blacklist);
-- If blacklist has NULL user_id → returns NO orders at all!

-- ✅ Safe: filter NULLs from subquery
SELECT * FROM orders
WHERE user_id NOT IN (
    SELECT user_id FROM blacklist WHERE user_id IS NOT NULL
);

-- ✅ Or use NOT EXISTS
SELECT * FROM orders o
WHERE NOT EXISTS (
    SELECT 1 FROM blacklist b WHERE b.user_id = o.user_id
);
```

---

## 📊 NULL in Sorting

```sql
-- MySQL: NULLs sort FIRST in ASC, LAST in DESC
SELECT name, salary FROM employees ORDER BY salary ASC;  -- NULLs first

-- Force NULLs last
SELECT name, salary FROM employees
ORDER BY salary IS NULL, salary ASC;  -- FALSE(0) sorts before TRUE(1)

-- Force NULLs first
SELECT name, salary FROM employees
ORDER BY salary IS NOT NULL, salary DESC;
```

---

## 🌍 Real-World Use Case — Data Pipeline Quality Check

```sql
-- Data quality report: count NULLs per critical column
SELECT
    COUNT(*) AS total_records,
    SUM(ISNULL(customer_id))    AS missing_customer_id,
    SUM(ISNULL(email))          AS missing_email,
    SUM(ISNULL(order_amount))   AS missing_amount,
    SUM(ISNULL(order_date))     AS missing_date,
    ROUND(SUM(ISNULL(email)) / COUNT(*) * 100, 2) AS pct_missing_email
FROM orders_staging;

-- Clean data before loading to data warehouse
INSERT INTO orders_clean
SELECT
    order_id,
    COALESCE(customer_id, -1) AS customer_id,  -- sentinel for unknown
    COALESCE(amount, 0)       AS amount,
    COALESCE(status, 'unknown') AS status,
    order_date
FROM orders_raw
WHERE order_id IS NOT NULL;  -- must have a key
```

---

## 💼 Interview Questions

1. **What is NULL in SQL? Is NULL = NULL?**
   > NULL means unknown/missing. `NULL = NULL` evaluates to UNKNOWN (not TRUE) — use `IS NULL` instead.

2. **What is the difference between IFNULL and COALESCE?**
   > IFNULL takes exactly 2 arguments (column, replacement). COALESCE takes multiple and returns the first non-NULL — more flexible.

3. **What is NULLIF used for?**
   > Returns NULL when two values are equal. Most commonly used to prevent division-by-zero errors.

4. **How does AVG handle NULLs?**
   > AVG ignores NULL values — it sums non-NULL values and divides by count of non-NULL rows.

5. **Scenario:** *"Your pipeline has a column that sometimes contains empty strings instead of NULLs. How do you clean this?"*
   ```sql
   UPDATE staging_table SET phone = NULL WHERE TRIM(phone) = '';
   -- Or in SELECT: NULLIF(TRIM(phone), '') AS phone
   ```

---

## 🔗 Related Topics

- [[13 - Logical Operators]] — NOT IN NULL gotcha
- [[15 - CASE WHEN]] — CASE returns NULL without ELSE
- [[17 - String Functions]] — TRIM, handling empty strings
- [[09 - WHERE Clause]] — IS NULL in WHERE
- [[12 - Aggregation Functions]] — How aggregates handle NULLs
