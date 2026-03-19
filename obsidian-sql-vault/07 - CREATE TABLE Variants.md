# 07 — CREATE TABLE Variants

#beginner #ddl

---

## 🧠 Introduction

`CREATE TABLE` has multiple forms — understanding all variants is essential for schema design, data pipelines, and ETL operations.

---

## 📐 Syntax

```sql
CREATE TABLE table_name (
    column1 datatype [constraints],
    column2 datatype [constraints],
    ...
    [table_constraints]
);
```

---

## 1️⃣ Basic CREATE TABLE

```sql
CREATE TABLE employees (
    emp_id     INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(50) NOT NULL,
    last_name  VARCHAR(50) NOT NULL,
    email      VARCHAR(255) UNIQUE,
    salary     DECIMAL(10,2) DEFAULT 30000.00,
    hire_date  DATE,
    dept_id    INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

---

## 2️⃣ CREATE TABLE IF NOT EXISTS

Prevents error if table already exists — **essential in scripts and pipelines**.

```sql
CREATE TABLE IF NOT EXISTS logs (
    log_id      INT PRIMARY KEY AUTO_INCREMENT,
    event_type  VARCHAR(50),
    event_time  DATETIME DEFAULT CURRENT_TIMESTAMP,
    message     TEXT
);
```

> Use this in ETL scripts so they don't fail on re-run.

---

## 3️⃣ CREATE TABLE AS SELECT (CTAS)

Creates a new table from the result of a SELECT query. Copies **data + structure** but NOT constraints.

```sql
-- Create a summary table from existing data
CREATE TABLE high_value_orders AS
SELECT 
    user_id,
    SUM(amount)  AS total_spent,
    COUNT(*)     AS order_count
FROM orders
WHERE amount > 1000
GROUP BY user_id;
```

```sql
-- Create a backup before risky operation
CREATE TABLE orders_backup AS SELECT * FROM orders;
```

> ⚠️ CTAS does NOT copy PRIMARY KEY, FOREIGN KEY, or UNIQUE constraints — you must add them separately.

---

## 4️⃣ CREATE TABLE LIKE

Copies the **structure** (columns + constraints) but NOT the data.

```sql
-- Create a staging table with same structure
CREATE TABLE orders_staging LIKE orders;

-- Useful for: temp tables, staging in ETL pipelines
```

---

## 5️⃣ Temporary Tables

Exists only for the current session — automatically dropped when session ends.

```sql
-- Create temp table
CREATE TEMPORARY TABLE temp_sales AS
SELECT product, SUM(amount) AS revenue
FROM orders
WHERE status = 'delivered'
GROUP BY product;

-- Use it
SELECT * FROM temp_sales ORDER BY revenue DESC LIMIT 10;

-- Drop manually (or session end will auto-drop)
DROP TEMPORARY TABLE temp_sales;
```

> Common in **stored procedures** and complex multi-step queries.

---

## 📊 MySQL Data Types Cheat Sheet

| Category | Type | Use For |
|----------|------|---------|
| Integer | `INT`, `BIGINT`, `TINYINT` | IDs, counts, flags |
| Decimal | `DECIMAL(p,s)`, `FLOAT`, `DOUBLE` | Money, measurements |
| String | `VARCHAR(n)`, `CHAR(n)`, `TEXT` | Names, descriptions |
| Date/Time | `DATE`, `DATETIME`, `TIMESTAMP` | Dates, events |
| Boolean | `BOOLEAN` / `TINYINT(1)` | True/False flags |
| Enum | `ENUM('val1','val2')` | Fixed categories |
| JSON | `JSON` | Semi-structured data |

```sql
-- Money: always use DECIMAL, never FLOAT
salary DECIMAL(10, 2)  -- Up to 99,999,999.99

-- Status fields: use ENUM for fixed values
status ENUM('active', 'inactive', 'suspended') DEFAULT 'active'

-- Large text: TEXT vs VARCHAR
description TEXT      -- up to 65,535 chars, no default
name VARCHAR(255)     -- max 255 chars, can have default
```

---

## 🌍 Real-World Use Case — Data Pipeline

```sql
-- Step 1: Create staging table (same structure as production)
CREATE TABLE orders_staging LIKE orders;

-- Step 2: Load raw data into staging
LOAD DATA INFILE '/tmp/orders_today.csv'
INTO TABLE orders_staging
FIELDS TERMINATED BY ','
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;

-- Step 3: Create validated subset
CREATE TABLE orders_validated AS
SELECT * FROM orders_staging
WHERE amount > 0 AND user_id IS NOT NULL;

-- Step 4: Merge into production
INSERT INTO orders SELECT * FROM orders_validated;
```

---

## 💼 Interview Questions

1. **What is the difference between CREATE TABLE AS SELECT and CREATE TABLE LIKE?**
   > CTAS copies structure + data, no constraints. LIKE copies structure + constraints, no data.

2. **When would you use a temporary table?**
   > For intermediate results in complex queries, storing session-specific data, multi-step ETL transformations.

3. **Why should you use DECIMAL instead of FLOAT for monetary values?**
   > FLOAT uses binary floating-point — imprecise for exact decimal values. `DECIMAL(10,2)` stores exact decimal values.

4. **What does `CREATE TABLE IF NOT EXISTS` do differently?**
   > It suppresses the "table already exists" error — essential for idempotent scripts.

---

## ⚠️ Common Mistakes

- Using `FLOAT` for currency — leads to rounding errors
- Forgetting that CTAS doesn't copy constraints
- Using `CHAR` for variable-length strings (wastes storage) — use `VARCHAR`
- Creating a TEMPORARY table but forgetting it won't persist after the session

---

## 🔗 Related Topics

- [[08 - ALTER TABLE]] — Modify table structure after creation
- [[10 - SQL Constraints]] — NOT NULL, UNIQUE, CHECK, etc.
- [[11 - Keys]] — Primary and Foreign keys
- [[05 - DDL DML TCL DCL]] — DDL category overview
- [[31 - Normalization]] — Designing table structures properly
