# 14 — Alias (AS)

#beginner #readability

---

## 🧠 Introduction

Aliases give **temporary names** to columns or tables within a query. They make queries readable, shorter, and are essential for self-joins and subqueries.

---

## 📐 Syntax

```sql
-- Column alias
SELECT column_name AS alias_name FROM table;

-- Table alias
SELECT t.column FROM table_name AS t;

-- AS keyword is optional in MySQL (but use it for clarity)
SELECT name n FROM employees e;  -- works but less readable
```

---

## 🔷 Column Aliases

```sql
-- Rename columns in output
SELECT 
    emp_id          AS "Employee ID",
    CONCAT(first_name, ' ', last_name) AS full_name,
    salary / 12     AS monthly_salary,
    ROUND(salary * 0.10, 2) AS annual_bonus
FROM employees;

-- With aggregation
SELECT 
    dept                    AS department,
    COUNT(*)                AS headcount,
    ROUND(AVG(salary), 2)   AS avg_salary,
    MAX(salary)             AS top_salary
FROM employees
GROUP BY dept
ORDER BY avg_salary DESC;
```

---

## 🔶 Table Aliases

```sql
-- Short table names for JOIN readability
SELECT 
    u.name    AS customer,
    o.order_id,
    o.amount,
    o.status
FROM users AS u
JOIN orders AS o ON u.user_id = o.user_id
WHERE u.city = 'Chennai';

-- Self-join requires alias (mandatory!)
SELECT 
    e.name      AS employee,
    m.name      AS manager
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;
```

---

## ⚠️ Alias Scope Rules

```sql
-- ❌ WRONG: Can't use alias in WHERE (executed before SELECT)
SELECT salary * 12 AS annual_sal
FROM employees
WHERE annual_sal > 600000;  -- ERROR!

-- ✅ CORRECT options:
-- Option 1: Repeat the expression
WHERE salary * 12 > 600000;

-- Option 2: Use subquery
SELECT * FROM (
    SELECT emp_id, name, salary * 12 AS annual_sal FROM employees
) t
WHERE t.annual_sal > 600000;

-- ✅ CAN use alias in ORDER BY and HAVING
SELECT dept, AVG(salary) AS avg_sal
FROM employees
GROUP BY dept
HAVING avg_sal > 50000  -- ✅ OK in HAVING (MySQL specific)
ORDER BY avg_sal DESC;  -- ✅ OK in ORDER BY
```

---

## 💼 Interview Questions

1. **Can you use a column alias in the WHERE clause?**
   > No — WHERE is evaluated before SELECT. Use the full expression or wrap in a subquery.

2. **When is a table alias mandatory?**
   > In self-joins — you must alias the table twice to distinguish the two instances.

---

## 🔗 Related Topics

- [[22 - Joins]] — Table aliases in complex joins
- [[20 - Subqueries]] — Aliases on derived tables
- [[14 - Alias]] → [[23 - Window Functions]] — AS for window function columns
