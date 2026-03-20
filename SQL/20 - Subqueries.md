# 20 — Subqueries

#intermediate #advanced #interview

---

## 🧠 Introduction

A subquery is a **query nested inside another query**. They allow you to use the result of one query as input to another — enabling complex, multi-step logic in a single SQL statement.

> Think of a subquery as a "question within a question": *"Find employees who earn more than [the average salary]"* — the bracketed part is the subquery.

---

## 📐 Subquery Positions

A subquery can appear in:
- `WHERE` clause — most common
- `FROM` clause — derived table / inline view
- `SELECT` clause — scalar subquery
- `HAVING` clause

---

## 🔷 Subquery in WHERE

```sql
-- Employees earning above average
SELECT name, salary
FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);

-- Orders from customers in specific cities
SELECT * FROM orders
WHERE user_id IN (
    SELECT user_id FROM users WHERE city IN ('Chennai', 'Bangalore')
);

-- Products never ordered
SELECT * FROM products
WHERE product_id NOT IN (
    SELECT DISTINCT product_id FROM order_items WHERE product_id IS NOT NULL
);

-- Most recent order for each user (subquery in WHERE)
SELECT * FROM orders o
WHERE order_date = (
    SELECT MAX(order_date) FROM orders WHERE user_id = o.user_id
);
```

---

## 🔶 Subquery in FROM (Derived Table)

```sql
-- Average of department averages
SELECT AVG(dept_avg) AS company_avg_of_avgs
FROM (
    SELECT dept, AVG(salary) AS dept_avg
    FROM employees
    GROUP BY dept
) AS dept_summary;

-- Top 10% earners
SELECT *
FROM (
    SELECT name, salary,
           PERCENT_RANK() OVER (ORDER BY salary DESC) AS pct
    FROM employees
) ranked
WHERE pct <= 0.10;

-- Daily stats summary with derived table
SELECT
    month,
    AVG(daily_orders) AS avg_daily_orders,
    MAX(daily_revenue) AS peak_revenue
FROM (
    SELECT
        DATE_FORMAT(order_date, '%Y-%m') AS month,
        DATE(order_date)                 AS day,
        COUNT(*)                         AS daily_orders,
        SUM(amount)                      AS daily_revenue
    FROM orders
    GROUP BY DATE(order_date)
) daily
GROUP BY month;
```

---

## 🔵 Scalar Subquery in SELECT

Returns exactly ONE value per row.

```sql
-- Employee name with their department name (without JOIN)
SELECT
    emp_id,
    name,
    (SELECT dept_name FROM departments WHERE dept_id = e.dept_id) AS department
FROM employees e;

-- Percentage of total for each order
SELECT
    order_id,
    amount,
    ROUND(amount / (SELECT SUM(amount) FROM orders) * 100, 2) AS pct_of_total
FROM orders;
```

---

## 🟡 Correlated Subquery

A subquery that **references the outer query's table**. Runs once per outer row — can be slow on large tables.

```sql
-- Find employees earning more than their department average
SELECT name, dept, salary
FROM employees e
WHERE salary > (
    SELECT AVG(salary)
    FROM employees
    WHERE dept = e.dept   -- references outer query!
);

-- Most recent order per customer
SELECT u.name, o.order_id, o.amount, o.order_date
FROM users u
JOIN orders o ON u.user_id = o.user_id
WHERE o.order_date = (
    SELECT MAX(order_date)
    FROM orders
    WHERE user_id = u.user_id  -- correlated
);
```

---

## 🟢 EXISTS / NOT EXISTS

More efficient than IN/NOT IN for existence checks — short-circuits on first match.

```sql
-- Customers who have placed at least one order
SELECT * FROM users u
WHERE EXISTS (
    SELECT 1 FROM orders WHERE user_id = u.user_id
);

-- Customers who have NEVER placed an order
SELECT * FROM users u
WHERE NOT EXISTS (
    SELECT 1 FROM orders WHERE user_id = u.user_id
);

-- Products that need restocking
SELECT * FROM products p
WHERE NOT EXISTS (
    SELECT 1 FROM inventory WHERE product_id = p.product_id AND quantity > 10
);
```

---

## 🔧 WITH Clause (CTE — Common Table Expression)

A cleaner, reusable alternative to subqueries in FROM.

```sql
-- CTE: much more readable than nested subqueries
WITH dept_averages AS (
    SELECT dept, AVG(salary) AS avg_sal
    FROM employees
    GROUP BY dept
),
high_earners AS (
    SELECT e.*, d.avg_sal
    FROM employees e
    JOIN dept_averages d ON e.dept = d.dept
    WHERE e.salary > d.avg_sal * 1.2
)
SELECT * FROM high_earners ORDER BY salary DESC;

-- Multiple CTEs
WITH
monthly_revenue AS (
    SELECT DATE_FORMAT(order_date, '%Y-%m') AS month, SUM(amount) AS revenue
    FROM orders GROUP BY month
),
ranked_months AS (
    SELECT *, RANK() OVER (ORDER BY revenue DESC) AS rnk
    FROM monthly_revenue
)
SELECT * FROM ranked_months WHERE rnk <= 3;
```

---

## 🌍 Real-World Use Case — Banking

```sql
-- Find accounts with balance below their account type's average
SELECT acc_id, holder, acc_type, balance
FROM accounts a
WHERE balance < (
    SELECT AVG(balance)
    FROM accounts
    WHERE acc_type = a.acc_type  -- correlated
);

-- Find customers with more than 3 failed transactions in last 7 days
SELECT user_id, COUNT(*) AS failed_count
FROM transactions
WHERE status = 'failed'
  AND created_at >= NOW() - INTERVAL 7 DAY
GROUP BY user_id
HAVING failed_count > 3
  AND user_id IN (SELECT user_id FROM accounts WHERE is_active = TRUE);
```

---

## 💼 Interview Questions

1. **What is a correlated subquery? How does it differ from a regular subquery?**
   > A regular subquery runs once independently. A correlated subquery references the outer query and runs once per outer row — it's slower but necessary for row-level comparisons.

2. **What is the difference between IN and EXISTS?**
   > IN materializes the subquery result and checks membership. EXISTS short-circuits on the first match — faster when the subquery would return many rows.

3. **What is a CTE and when would you use it over a subquery?**
   > A CTE (WITH clause) is a named temporary result set. Use it for: readability, reusing the same subquery multiple times, recursive queries.

4. **Scenario:** *"Find the second highest salary in the employees table."*
   ```sql
   SELECT MAX(salary) FROM employees
   WHERE salary < (SELECT MAX(salary) FROM employees);
   ```

5. **Scenario:** *"Find departments where all employees earn more than 50000."*
   ```sql
   SELECT dept FROM employees
   GROUP BY dept
   HAVING MIN(salary) > 50000;
   -- or with NOT EXISTS:
   SELECT DISTINCT dept FROM employees e1
   WHERE NOT EXISTS (
       SELECT 1 FROM employees e2
       WHERE e2.dept = e1.dept AND e2.salary <= 50000
   );
   ```

---

## 🚀 Performance Notes

- **Correlated subqueries** run N times (once per outer row) — replace with JOINs when possible
- **EXISTS** is faster than `IN (subquery)` for large result sets
- **CTEs** are generally the same performance as subqueries in MySQL 8.0 (not materialized by default)
- Use `EXPLAIN` to compare correlated subquery vs JOIN performance → [[26 - EXPLAIN ANALYZE]]

---

## 🔗 Related Topics

- [[22 - Joins]] — Often more efficient than subqueries
- [[23 - Window Functions]] — Alternative to correlated subqueries for ranking
- [[13 - Logical Operators]] — IN, NOT IN, EXISTS
- [[21 - Views]] — Stored subqueries as views
- [[26 - EXPLAIN ANALYZE]] — Measure subquery performance
