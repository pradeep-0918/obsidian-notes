# 22 — Joins

#intermediate #advanced #interview

---

## 🧠 Introduction

JOINs combine rows from **two or more tables** based on a related column. This is the most tested SQL concept in interviews — know every type cold.

> The entire power of relational databases comes from JOINs. If you can't JOIN, you can't query real-world data.

---

## 🗺️ Visual Overview

```
Table A          Table B
+----+------+   +----+-------+
| id | name |   | id | order |
+----+------+   +----+-------+
|  1 | Priya|   |  1 | ₹500  |
|  2 | Ravi |   |  1 | ₹300  |
|  3 | Anita|   |  3 | ₹700  |
+----+------+   +----+-------+

INNER JOIN:  rows 1 (Priya - ₹500), 1 (Priya - ₹300), 3 (Anita - ₹700)
LEFT JOIN:   all from A + matched from B (Ravi has NULL order)
RIGHT JOIN:  all from B + matched from A
FULL JOIN:   all from both (emulated in MySQL)
```

---

## 🔷 INNER JOIN

Returns only rows that **match in both tables**.

```sql
-- Basic INNER JOIN
SELECT u.name, o.order_id, o.amount
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id;
-- Only users who have orders appear

-- Multiple JOINs
SELECT
    u.name        AS customer,
    o.order_id,
    p.name        AS product,
    oi.quantity,
    oi.quantity * p.price AS line_total
FROM users u
JOIN orders o     ON u.user_id     = o.user_id
JOIN order_items oi ON o.order_id  = oi.order_id
JOIN products p   ON oi.product_id = p.product_id
WHERE o.status = 'delivered';
```

---

## 🔶 LEFT JOIN (LEFT OUTER JOIN)

Returns **all rows from the left table**, and matching rows from the right. Non-matching right rows get NULL.

```sql
-- All users, even those with no orders
SELECT u.name, o.order_id, o.amount
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id;
-- Ravi appears with NULL order_id and amount

-- Find users who have NEVER ordered (classic pattern)
SELECT u.user_id, u.name
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
WHERE o.order_id IS NULL;
```

---

## 🔵 RIGHT JOIN (RIGHT OUTER JOIN)

Returns **all rows from the right table**, matching from left. Rarely used — rewrite as LEFT JOIN instead.

```sql
-- All orders, even if user was deleted (orphaned orders)
SELECT u.name, o.order_id, o.amount
FROM users u
RIGHT JOIN orders o ON u.user_id = o.user_id;
-- Equivalent (preferred):
SELECT u.name, o.order_id, o.amount
FROM orders o
LEFT JOIN users u ON o.user_id = u.user_id;
```

---

## 🟡 FULL OUTER JOIN (Emulated in MySQL)

MySQL doesn't natively support FULL OUTER JOIN. Emulate with UNION:

```sql
-- All rows from both tables, NULLs where no match
SELECT u.user_id, u.name, o.order_id, o.amount
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id

UNION

SELECT u.user_id, u.name, o.order_id, o.amount
FROM users u
RIGHT JOIN orders o ON u.user_id = o.user_id
WHERE u.user_id IS NULL;
```

---

## 🔴 CROSS JOIN

Returns the **Cartesian product** — every row in A combined with every row in B.

```sql
-- All possible size-color combinations for a product
SELECT s.size_name, c.color_name
FROM sizes s
CROSS JOIN colors c;
-- 4 sizes × 6 colors = 24 rows

-- Generate date range (practical use)
SELECT DATE_ADD('2024-01-01', INTERVAL t.n DAY) AS dt
FROM (SELECT 0 n UNION ALL SELECT 1 UNION ALL SELECT 2 ... SELECT 365) t;
```

---

## 🟢 SELF JOIN

Joining a table to **itself**. Requires table aliases. Used for hierarchical data.

```sql
-- Employee and their manager (both in same table)
SELECT
    e.emp_id,
    e.name          AS employee,
    e.salary,
    m.name          AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;

-- Find employees in the same department
SELECT
    a.name AS emp1,
    b.name AS emp2,
    a.dept
FROM employees a
JOIN employees b ON a.dept = b.dept AND a.emp_id < b.emp_id;
```

---

## 🔧 JOIN with Multiple Conditions

```sql
-- Join with complex conditions
SELECT *
FROM orders o
JOIN promotions p ON o.user_id = p.user_id
                  AND o.order_date BETWEEN p.start_date AND p.end_date;

-- Non-equi JOIN (range-based)
SELECT e.name, e.salary, sg.grade
FROM employees e
JOIN salary_grades sg ON e.salary BETWEEN sg.min_salary AND sg.max_salary;
```

---

## 🌍 Real-World Use Case — E-Commerce Order Report

```sql
-- Complete order report with all related data
SELECT
    o.order_id,
    u.name                          AS customer,
    u.city,
    GROUP_CONCAT(p.name SEPARATOR ', ') AS products,
    SUM(oi.quantity)                AS total_items,
    o.amount                        AS order_total,
    o.status,
    o.order_date,
    DATEDIFF(o.delivered_at, o.order_date) AS delivery_days
FROM orders o
JOIN users u         ON o.user_id     = u.user_id
JOIN order_items oi  ON o.order_id    = oi.order_id
JOIN products p      ON oi.product_id = p.product_id
LEFT JOIN deliveries d ON o.order_id  = d.order_id
WHERE o.order_date >= '2024-01-01'
GROUP BY o.order_id, u.name, u.city, o.amount, o.status, o.order_date, o.delivered_at
ORDER BY o.order_date DESC;
```

---

## 💼 Interview Questions

1. **What is the difference between INNER JOIN and LEFT JOIN?**
   > INNER JOIN returns only matching rows from both tables. LEFT JOIN returns all rows from the left table plus matching rows from right (NULLs where no match).

2. **How do you find records in Table A that don't exist in Table B?**
   ```sql
   SELECT a.* FROM a LEFT JOIN b ON a.id = b.a_id WHERE b.a_id IS NULL;
   ```

3. **What is a SELF JOIN? Give an example.**
   > Joining a table with itself. Example: finding employee-manager relationships where both are in the same employees table.

4. **What is a CROSS JOIN? When would you use one?**
   > Returns Cartesian product of two tables. Use for: generating all combinations (size × color), date/number series generation.

5. **Scenario:** *"Find customers who ordered in 2023 but NOT in 2024."*
   ```sql
   SELECT DISTINCT u.user_id, u.name
   FROM users u
   JOIN orders o23 ON u.user_id = o23.user_id AND YEAR(o23.order_date) = 2023
   LEFT JOIN orders o24 ON u.user_id = o24.user_id AND YEAR(o24.order_date) = 2024
   WHERE o24.order_id IS NULL;
   ```

---

## 🚀 Performance Tips

- Join on **indexed columns** — always index foreign key columns
- Filter with WHERE before JOIN when possible (use subqueries or CTEs)
- Avoid joining on functions: `JOIN ON YEAR(a.date) = YEAR(b.date)` → can't use indexes
- Use `EXPLAIN` to check join type (ref, range, ALL) → [[26 - EXPLAIN ANALYZE]]

---

## 🔗 Related Topics

- [[20 - Subqueries]] — Alternative to some JOINs
- [[11 - Keys]] — JOINs happen via FK → PK relationships
- [[31 - Normalization]] — Normalization creates the need for JOINs
- [[25 - Indexes]] — Index FK columns for JOIN performance
- [[26 - EXPLAIN ANALYZE]] — Measure and optimize JOIN performance
- [[24 - UNION and UNION ALL]] — Emulating FULL OUTER JOIN
