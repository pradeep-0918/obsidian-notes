# 13 — NOT IN, AND, OR, IN Operators

#beginner #filtering #interview

---

## 🧠 Introduction

Logical operators allow you to **combine conditions** in WHERE, HAVING, and JOIN clauses. Master these for precise, efficient data filtering.

---

## 🔷 AND

Both conditions must be TRUE. Reduces the result set.

```sql
-- Employees in Engineering with salary > 60000
SELECT * FROM employees
WHERE dept = 'Engineering' AND salary > 60000;

-- Orders delivered in January 2024
SELECT * FROM orders
WHERE status = 'delivered'
  AND order_date >= '2024-01-01'
  AND order_date <= '2024-01-31';

-- Multiple AND conditions
SELECT * FROM products
WHERE category = 'Electronics'
  AND price < 50000
  AND stock > 0
  AND is_active = TRUE;
```

---

## 🔶 OR

At least one condition must be TRUE. Expands the result set.

```sql
-- Users from Chennai or Bangalore
SELECT * FROM users WHERE city = 'Chennai' OR city = 'Bangalore';

-- Orders that are pending OR cancelled
SELECT * FROM orders WHERE status = 'pending' OR status = 'cancelled';

-- Multiple OR — use IN instead (cleaner)
SELECT * FROM employees
WHERE dept = 'Engineering'
   OR dept = 'Data'
   OR dept = 'DevOps';
-- Better written as:
SELECT * FROM employees WHERE dept IN ('Engineering', 'Data', 'DevOps');
```

---

## ⚠️ Operator Precedence (Critical!)

**AND is evaluated before OR.** Always use parentheses with mixed conditions.

```sql
-- ❌ WRONG — AND runs first!
SELECT * FROM orders
WHERE status = 'pending' OR status = 'processing' AND amount > 1000;
-- Interpreted as: status='pending' OR (status='processing' AND amount>1000)

-- ✅ CORRECT
SELECT * FROM orders
WHERE (status = 'pending' OR status = 'processing') AND amount > 1000;
```

---

## 🔵 IN

Tests if a value is in a **list**. Cleaner alternative to multiple ORs.

```sql
-- Match a list of values
SELECT * FROM employees
WHERE dept IN ('Engineering', 'Marketing', 'Sales');

-- Equivalent to:
WHERE dept = 'Engineering' OR dept = 'Marketing' OR dept = 'Sales'

-- IN with numbers
SELECT * FROM orders WHERE order_id IN (101, 205, 307, 412);

-- IN with subquery (correlated)
SELECT * FROM employees
WHERE dept_id IN (
    SELECT dept_id FROM departments WHERE location = 'Mumbai'
);
```

---

## 🔴 NOT IN

Excludes rows that match any value in the list.

```sql
-- Employees NOT in HR or Admin
SELECT * FROM employees
WHERE dept NOT IN ('HR', 'Admin');

-- Orders not yet delivered or cancelled
SELECT * FROM orders
WHERE status NOT IN ('delivered', 'cancelled');
```

### ⚠️ NOT IN and NULL — Critical Gotcha!

```sql
-- If the subquery returns any NULL, NOT IN returns NO rows at all!
SELECT * FROM employees
WHERE dept_id NOT IN (SELECT dept_id FROM departments);
-- If any dept_id in departments is NULL → result is empty set!

-- ✅ Safe approach: filter NULLs in subquery
SELECT * FROM employees
WHERE dept_id NOT IN (
    SELECT dept_id FROM departments WHERE dept_id IS NOT NULL
);

-- Or use NOT EXISTS (handles NULLs correctly)
SELECT * FROM employees e
WHERE NOT EXISTS (
    SELECT 1 FROM departments d WHERE d.dept_id = e.dept_id
);
```

---

## 🔧 NOT

Negates a condition.

```sql
-- NOT with LIKE
SELECT * FROM users WHERE name NOT LIKE 'A%';

-- NOT with BETWEEN
SELECT * FROM products WHERE price NOT BETWEEN 1000 AND 5000;

-- NOT with NULL (always use IS NOT NULL)
SELECT * FROM employees WHERE manager_id IS NOT NULL;
```

---

## 📊 Truth Table Reference

| A | B | A AND B | A OR B | NOT A |
|---|---|---------|--------|-------|
| T | T | T | T | F |
| T | F | F | T | F |
| F | T | F | T | T |
| F | F | F | F | T |

---

## 🌍 Real-World Use Case — Log Analytics Pipeline

```sql
-- Find error logs from production servers, excluding known maintenance windows
SELECT 
    server_id,
    error_message,
    logged_at
FROM system_logs
WHERE log_level IN ('ERROR', 'CRITICAL', 'FATAL')
  AND server_id NOT IN (
      SELECT server_id FROM maintenance_schedule
      WHERE maintenance_start <= NOW() AND maintenance_end >= NOW()
  )
  AND logged_at >= NOW() - INTERVAL 1 HOUR
ORDER BY logged_at DESC;
```

---

## 💼 Interview Questions

1. **What happens when NOT IN encounters a NULL in the subquery?**
   > The entire NOT IN clause returns an empty result set (UNKNOWN logic propagates). Use `NOT EXISTS` or filter NULLs with `WHERE column IS NOT NULL`.

2. **When would you use IN vs OR?**
   > Use IN when checking against multiple values of the same column — cleaner and sometimes better optimized. Use OR for different columns or complex conditions.

3. **What is the difference between IN and EXISTS?**
   > IN fetches the full subquery result and matches. EXISTS checks for row existence (short-circuits when first match found). EXISTS is generally faster for large subqueries.

4. **Scenario:** *"Find customers who bought product A but NOT product B."*
   ```sql
   SELECT DISTINCT user_id FROM orders WHERE product = 'A'
   AND user_id NOT IN (SELECT user_id FROM orders WHERE product = 'B');
   ```

---

## 🔗 Related Topics

- [[09 - WHERE Clause]] — All WHERE filtering concepts
- [[16 - NULL Handling]] — NULL with NOT IN
- [[20 - Subqueries]] — IN/NOT IN with subqueries
- [[12 - Aggregation Functions]] — HAVING with logical operators
