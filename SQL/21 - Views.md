# 21 — Views

#intermediate #interview

---

## 🧠 Introduction

A **view** is a stored SQL query that acts like a virtual table. It doesn't store data itself — it runs the underlying query each time you access it. Views simplify complex queries and enforce security.

> Think of a view as a "saved query with a name" — like a shortcut to a complex SELECT statement.

---

## 📐 Syntax

```sql
-- Create a view
CREATE VIEW view_name AS
SELECT ...;

-- Replace existing view
CREATE OR REPLACE VIEW view_name AS
SELECT ...;

-- Use a view (same as querying a table)
SELECT * FROM view_name;

-- Drop a view
DROP VIEW view_name;
```

---

## 🔷 Basic View Examples

```sql
-- View: Active customers with order summary
CREATE VIEW vw_active_customers AS
SELECT
    u.user_id,
    u.name,
    u.email,
    u.city,
    COUNT(o.order_id)  AS total_orders,
    SUM(o.amount)      AS lifetime_value
FROM users u
JOIN orders o ON u.user_id = o.user_id
WHERE o.status != 'cancelled'
GROUP BY u.user_id, u.name, u.email, u.city;

-- Now use it simply
SELECT * FROM vw_active_customers WHERE city = 'Chennai';
SELECT * FROM vw_active_customers ORDER BY lifetime_value DESC LIMIT 10;

-- View: Employee full info (join-free access)
CREATE VIEW vw_employee_details AS
SELECT
    e.emp_id,
    e.name,
    e.salary,
    d.dept_name,
    d.location,
    m.name AS manager_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id
LEFT JOIN employees m   ON e.manager_id = m.emp_id;
```

---

## 🔐 Security Views (Column/Row Level Security)

```sql
-- Expose only non-sensitive columns to analysts
CREATE VIEW vw_users_public AS
SELECT user_id, name, city, created_at
FROM users;
-- Analysts NEVER see email or phone

-- Row-level security: only show active accounts
CREATE VIEW vw_active_accounts AS
SELECT acc_id, holder, acc_type, balance
FROM accounts
WHERE is_active = TRUE;

-- GRANT access to view, not base table
GRANT SELECT ON mydb.vw_users_public TO 'analyst'@'%';
REVOKE SELECT ON mydb.users FROM 'analyst'@'%';
```

---

## 🔄 Updatable Views

Views can support INSERT/UPDATE/DELETE IF:
- Based on single table
- No DISTINCT, GROUP BY, HAVING, UNION
- No aggregate functions

```sql
CREATE VIEW vw_chennai_users AS
SELECT user_id, name, email, city
FROM users
WHERE city = 'Chennai';

-- This works (updatable view)
UPDATE vw_chennai_users SET name = 'Priya K.' WHERE user_id = 5;
INSERT INTO vw_chennai_users (name, email, city) VALUES ('Karthik', 'k@test.com', 'Chennai');

-- WITH CHECK OPTION: prevent inserting rows that won't be visible in the view
CREATE VIEW vw_chennai_users AS
SELECT user_id, name, email, city
FROM users
WHERE city = 'Chennai'
WITH CHECK OPTION;

-- Now this FAILS (Chennai view can't insert Bangalore rows)
INSERT INTO vw_chennai_users VALUES (NULL, 'Ravi', 'r@test.com', 'Bangalore');
-- ERROR: CHECK OPTION failed
```

---

## 🌍 Real-World Use Case — Data Engineering / BI Layer

```sql
-- Business-facing view for dashboards (hides complexity)
CREATE OR REPLACE VIEW vw_sales_dashboard AS
SELECT
    DATE_FORMAT(o.order_date, '%Y-%m')          AS month,
    p.category,
    u.city,
    COUNT(DISTINCT o.order_id)                   AS orders,
    SUM(oi.quantity)                             AS units_sold,
    ROUND(SUM(oi.quantity * p.price), 2)         AS revenue,
    COUNT(DISTINCT o.user_id)                    AS unique_customers
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
JOIN products p     ON oi.product_id = p.product_id
JOIN users u        ON o.user_id = u.user_id
WHERE o.status = 'delivered'
GROUP BY month, p.category, u.city;

-- BI tool just queries:
SELECT * FROM vw_sales_dashboard WHERE month = '2024-07' ORDER BY revenue DESC;
```

---

## ⚠️ Views vs Materialized Views

| Feature | Regular View | Materialized View |
|---------|-------------|-------------------|
| Stores data? | No (virtual) | Yes (cached) |
| Always fresh? | Yes | Only on refresh |
| Performance | Same as query | Fast (pre-computed) |
| MySQL support | ✅ Yes | ❌ Not natively (use tables) |

```sql
-- Simulating materialized view in MySQL:
-- Create a real table and refresh periodically
CREATE TABLE mv_monthly_revenue AS
SELECT DATE_FORMAT(order_date, '%Y-%m') AS month, SUM(amount) AS revenue
FROM orders GROUP BY month;

-- Refresh (run via scheduled job / event)
TRUNCATE TABLE mv_monthly_revenue;
INSERT INTO mv_monthly_revenue
SELECT DATE_FORMAT(order_date, '%Y-%m') AS month, SUM(amount) AS revenue
FROM orders GROUP BY month;
```

---

## 💼 Interview Questions

1. **What is a view? Does it store data?**
   > A view is a named, stored SELECT query — a virtual table. In MySQL, it does NOT store data; it executes the underlying query each time it's accessed.

2. **What is WITH CHECK OPTION?**
   > Ensures that INSERT/UPDATE through the view don't create rows that would be invisible in the view (violate the view's WHERE condition).

3. **When is a view NOT updatable?**
   > When it contains DISTINCT, GROUP BY, HAVING, aggregate functions, UNION, or joins to multiple tables.

4. **What is a materialized view? Does MySQL support it?**
   > A materialized view caches query results as real data. MySQL doesn't natively support it — simulate with a scheduled refresh table.

5. **Scenario:** *"Analysts need access to sales data but should NOT see customer PII (phone, email). How do you implement this?"*
   > Create a view excluding PII columns, GRANT SELECT on the view, REVOKE SELECT on the base table.

---

## 🔗 Related Topics

- [[20 - Subqueries]] — Views are like named subqueries
- [[29 - GRANT REVOKE]] — Controlling view permissions
- [[22 - Joins]] — Views often wrap complex joins
- [[26 - EXPLAIN ANALYZE]] — Check view performance
