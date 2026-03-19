# 12 — Aggregation Functions

#intermediate #interview #performance

---

## 🧠 Introduction

Aggregation functions **summarize multiple rows into a single value**. They are the backbone of reporting, dashboards, and analytics queries.

> Every time a business asks "how many?", "what's the total?", "what's the average?" — aggregation functions are the answer.

---

## 📊 Core Aggregation Functions

| Function | Description |
|---------|-------------|
| `COUNT()` | Number of rows / non-null values |
| `SUM()` | Total of numeric values |
| `AVG()` | Average of numeric values |
| `MAX()` | Maximum value |
| `MIN()` | Minimum value |
| `GROUP_CONCAT()` | Concatenates values from multiple rows |

---

## 🔢 COUNT

```sql
-- Count all rows
SELECT COUNT(*) FROM orders;

-- Count non-NULL values only
SELECT COUNT(delivered_at) FROM orders;  -- only delivered orders

-- Count distinct values
SELECT COUNT(DISTINCT user_id) FROM orders;  -- unique customers who ordered

-- Count with condition
SELECT COUNT(*) FROM orders WHERE status = 'completed';
```

---

## ➕ SUM

```sql
-- Total revenue
SELECT SUM(amount) AS total_revenue FROM orders;

-- Revenue by status
SELECT status, SUM(amount) AS revenue
FROM orders
GROUP BY status;

-- Revenue from last 30 days
SELECT SUM(amount)
FROM orders
WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY);
```

---

## 📈 AVG, MAX, MIN

```sql
-- Average, max, min salary by department
SELECT 
    dept,
    ROUND(AVG(salary), 2) AS avg_salary,
    MAX(salary)            AS highest_salary,
    MIN(salary)            AS lowest_salary
FROM employees
GROUP BY dept;

-- Latest and earliest order per user
SELECT 
    user_id,
    MAX(order_date) AS last_order,
    MIN(order_date) AS first_order
FROM orders
GROUP BY user_id;
```

---

## 🔗 GROUP BY — Required with Aggregations

```sql
-- Without GROUP BY: one row for whole table
SELECT COUNT(*), SUM(amount) FROM orders;

-- With GROUP BY: one row per group
SELECT 
    city,
    COUNT(*)       AS customer_count,
    SUM(amount)    AS city_revenue,
    AVG(amount)    AS avg_order_value
FROM orders o
JOIN users u ON o.user_id = u.user_id
GROUP BY city
ORDER BY city_revenue DESC;
```

---

## 🚦 HAVING — Filter After Aggregation

```sql
-- Departments with more than 5 employees
SELECT dept, COUNT(*) AS headcount
FROM employees
GROUP BY dept
HAVING headcount > 5;

-- High-value customers (spent > 10000 total)
SELECT 
    user_id,
    SUM(amount) AS total_spent
FROM orders
WHERE status = 'delivered'
GROUP BY user_id
HAVING total_spent > 10000
ORDER BY total_spent DESC;

-- WHERE vs HAVING
SELECT dept, AVG(salary) AS avg_sal
FROM employees
WHERE hire_date > '2020-01-01'    -- filter rows FIRST (WHERE)
GROUP BY dept
HAVING avg_sal > 50000;            -- filter groups AFTER (HAVING)
```

---

## 🔗 GROUP_CONCAT

```sql
-- List all products per order
SELECT 
    order_id,
    GROUP_CONCAT(product_name ORDER BY product_name SEPARATOR ', ') AS products
FROM order_items
GROUP BY order_id;

-- With DISTINCT
SELECT 
    user_id,
    GROUP_CONCAT(DISTINCT category SEPARATOR ' | ') AS categories_purchased
FROM orders
GROUP BY user_id;
```

---

## 🌍 Real-World Use Case — Analytics Dashboard (E-Commerce)

```sql
-- Daily Revenue Report
SELECT 
    DATE(order_date)       AS date,
    COUNT(*)               AS total_orders,
    COUNT(DISTINCT user_id) AS unique_customers,
    SUM(amount)            AS revenue,
    AVG(amount)            AS avg_order_value,
    MAX(amount)            AS largest_order
FROM orders
WHERE status = 'delivered'
  AND order_date >= '2024-01-01'
GROUP BY DATE(order_date)
ORDER BY date DESC;

-- Category performance
SELECT 
    p.category,
    COUNT(DISTINCT o.order_id)  AS orders,
    SUM(oi.quantity)            AS units_sold,
    SUM(oi.quantity * p.price)  AS revenue
FROM order_items oi
JOIN products p ON oi.product_id = p.product_id
JOIN orders o   ON oi.order_id = o.order_id
WHERE o.status = 'delivered'
GROUP BY p.category
HAVING revenue > 100000
ORDER BY revenue DESC;
```

---

## 💼 Interview Questions

1. **What is the difference between WHERE and HAVING?**
   > WHERE filters rows before grouping (cannot use aggregate functions). HAVING filters groups after GROUP BY (can use aggregate functions).

2. **What does `COUNT(*)` vs `COUNT(column)` return?**
   > `COUNT(*)` counts all rows including NULLs. `COUNT(column)` counts only non-NULL values in that column.

3. **Can you use an aggregate function in a WHERE clause?**
   > No — use HAVING instead.

4. **Find employees earning more than the average salary:**
   ```sql
   SELECT * FROM employees
   WHERE salary > (SELECT AVG(salary) FROM employees);
   ```

5. **Scenario:** *"Find the top 3 cities by total order revenue this year."*
   ```sql
   SELECT u.city, SUM(o.amount) AS revenue
   FROM orders o JOIN users u ON o.user_id = u.user_id
   WHERE YEAR(o.order_date) = YEAR(CURDATE())
   GROUP BY u.city
   ORDER BY revenue DESC
   LIMIT 3;
   ```

---

## 🚀 Advanced Insights

```sql
-- ROLLUP: subtotals and grand total
SELECT dept, job_title, SUM(salary)
FROM employees
GROUP BY dept, job_title WITH ROLLUP;

-- Conditional aggregation (pivot-like)
SELECT 
    dept,
    SUM(CASE WHEN gender = 'M' THEN salary END) AS male_payroll,
    SUM(CASE WHEN gender = 'F' THEN salary END) AS female_payroll,
    COUNT(CASE WHEN gender = 'M' THEN 1 END)    AS male_count,
    COUNT(CASE WHEN gender = 'F' THEN 1 END)    AS female_count
FROM employees
GROUP BY dept;
```

---

## 🔗 Related Topics

- [[23 - Window Functions]] — Aggregations without collapsing rows
- [[15 - CASE WHEN]] — Conditional aggregation
- [[09 - WHERE Clause]] — WHERE vs HAVING
- [[20 - Subqueries]] — Aggregates inside subqueries
- [[26 - EXPLAIN ANALYZE]] — Optimize GROUP BY performance
