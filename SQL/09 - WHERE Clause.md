# 09 — WHERE Clause

#beginner #filtering #interview

---

## 🧠 Introduction

The `WHERE` clause is the **primary filter** in SQL. It's used in SELECT, UPDATE, DELETE — and understanding it deeply is the difference between efficient queries and full table scans.

> Think of WHERE as "the bouncer" — it decides which rows get through.

---

## 📐 Syntax

```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

---

## ⚡ Comparison Operators

```sql
-- Equality
SELECT * FROM employees WHERE dept = 'Engineering';

-- Not equal
SELECT * FROM employees WHERE dept != 'HR';
SELECT * FROM employees WHERE dept <> 'HR';  -- same thing

-- Greater than / Less than
SELECT * FROM orders WHERE amount > 5000;
SELECT * FROM orders WHERE amount >= 5000;
SELECT * FROM employees WHERE hire_date < '2023-01-01';

-- BETWEEN (inclusive on both ends)
SELECT * FROM orders WHERE amount BETWEEN 1000 AND 5000;

-- IN (list of values)
SELECT * FROM employees WHERE dept IN ('Engineering', 'Marketing', 'Sales');

-- NOT IN
SELECT * FROM employees WHERE dept NOT IN ('HR', 'Admin');

-- LIKE (pattern matching)
SELECT * FROM users WHERE name LIKE 'A%';       -- starts with A
SELECT * FROM users WHERE email LIKE '%@gmail.com'; -- Gmail users
SELECT * FROM users WHERE name LIKE '_avi%';    -- second char is 'a', e.g. Ravi

-- IS NULL / IS NOT NULL
SELECT * FROM employees WHERE manager_id IS NULL;      -- top-level managers
SELECT * FROM orders WHERE delivered_at IS NOT NULL;   -- completed orders
```

---

## 🔗 Logical Operators in WHERE

```sql
-- AND (both must be true)
SELECT * FROM orders
WHERE status = 'shipped' AND amount > 1000;

-- OR (either must be true)
SELECT * FROM employees
WHERE dept = 'Engineering' OR dept = 'Data';

-- NOT (negate condition)
SELECT * FROM users
WHERE NOT city = 'Chennai';

-- Combining AND + OR (use parentheses!)
SELECT * FROM orders
WHERE (status = 'pending' OR status = 'processing')
  AND amount > 500;
```

> ⚠️ Operator precedence: AND is evaluated before OR. **Always use parentheses** with mixed operators.

---

## 🧮 WHERE with Functions

```sql
-- Date functions in WHERE
SELECT * FROM orders WHERE YEAR(order_date) = 2024;
SELECT * FROM orders WHERE DATE(created_at) = CURDATE();
SELECT * FROM orders WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY);

-- String functions in WHERE
SELECT * FROM users WHERE UPPER(city) = 'CHENNAI';
SELECT * FROM users WHERE LENGTH(name) > 20;
SELECT * FROM products WHERE TRIM(name) = 'iPhone 15';
```

---

## 📊 WHERE Execution Order (Important for Performance)

```sql
-- SQL logical execution order:
-- FROM → JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT

-- WHERE filters happen BEFORE aggregation
-- HAVING filters happen AFTER aggregation
SELECT dept, COUNT(*) AS headcount
FROM employees
WHERE salary > 40000      -- filter rows first (WHERE)
GROUP BY dept
HAVING headcount > 3;     -- then filter groups (HAVING)
```

---

## 🌍 Real-World Use Case — Ride-Sharing (Uber)

```sql
-- Find all completed trips over 20km in Bangalore today
SELECT trip_id, driver_id, distance_km, fare
FROM trips
WHERE city = 'Bangalore'
  AND status = 'completed'
  AND distance_km > 20
  AND trip_date = CURDATE();

-- Identify surge pricing candidates (high demand areas in last 1 hour)
SELECT pickup_area, COUNT(*) AS pending_trips
FROM trips
WHERE status = 'pending'
  AND created_at >= NOW() - INTERVAL 1 HOUR
GROUP BY pickup_area
HAVING pending_trips > 10;
```

---

## 💼 Interview Questions

1. **What is the difference between WHERE and HAVING?**
   > WHERE filters individual rows before grouping; HAVING filters groups after GROUP BY. You can't use aggregate functions in WHERE.

2. **Can you use column aliases in the WHERE clause?**
   > No — aliases defined in SELECT are not available in WHERE because WHERE is evaluated before SELECT. Use the expression directly.

3. **What is the difference between `!=` and `NOT IN`?**
   > `!=` compares to a single value; `NOT IN` filters against a list. But `NOT IN` with NULLs in the list returns no results!

4. **How does LIKE work? What does `%` vs `_` mean?**
   > `%` matches any sequence of characters (zero or more). `_` matches exactly one character.

5. **Scenario:** *"Find all customers who placed orders in 2024 but never in 2023."*
   ```sql
   SELECT DISTINCT user_id FROM orders WHERE YEAR(order_date) = 2024
   AND user_id NOT IN (
       SELECT user_id FROM orders WHERE YEAR(order_date) = 2023
   );
   ```

---

## 💡 Tips & Best Practices

- **Filter early** — a WHERE clause that filters more rows early makes JOINs and aggregations faster
- Avoid `WHERE UPPER(column) = 'VALUE'` — wrapping columns in functions **prevents index usage**
- Use `BETWEEN` for range checks — more readable and sometimes faster
- Check NULL handling carefully — `column != 'value'` does NOT return rows where column IS NULL

---

## ⚠️ Common Mistakes

```sql
-- WRONG: NULL comparison doesn't work with =
WHERE manager_id = NULL   ❌

-- CORRECT
WHERE manager_id IS NULL  ✅

-- WRONG: OR without parentheses
WHERE status = 'pending' OR status = 'processing' AND amount > 500
-- AND runs first! Only 'processing' rows with amount > 500 are filtered

-- CORRECT
WHERE (status = 'pending' OR status = 'processing') AND amount > 500  ✅
```

---

## 🔗 Related Topics

- [[13 - Logical Operators]] — AND, OR, NOT, IN, NOT IN in depth
- [[09 - WHERE Clause]] → closely related to [[25 - Indexes]] — WHERE conditions affect index usage
- [[16 - NULL Handling]] — NULL in WHERE conditions
- [[20 - Subqueries]] — Subqueries in WHERE clauses
- [[26 - EXPLAIN ANALYZE]] — Check if your WHERE uses an index
