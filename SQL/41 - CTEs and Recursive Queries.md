# 41 — CTEs (Common Table Expressions)

#advanced #interview #readability

---

## 🧠 Introduction

A **CTE (WITH clause)** is a named temporary result set defined at the beginning of a query. Think of it as giving a name to a subquery so it can be referenced multiple times — dramatically improving readability for complex queries.

> CTEs don't improve performance on their own, but they make your code maintainable and often let you break a monster query into logical steps.

---

## 📐 Syntax

```sql
-- Single CTE
WITH cte_name AS (
    SELECT ...
)
SELECT * FROM cte_name;

-- Multiple CTEs
WITH
cte_one AS (SELECT ...),
cte_two AS (SELECT ... FROM cte_one),  -- can reference previous CTEs
cte_three AS (SELECT ...)
SELECT * FROM cte_two JOIN cte_three ...;
```

---

## 🔷 CTE vs Subquery — When to Use Which

```sql
-- SUBQUERY: fine for simple, single-use derived tables
SELECT * FROM (
    SELECT dept, AVG(salary) avg FROM employees GROUP BY dept
) t WHERE t.avg > 50000;

-- CTE: better when:
-- 1. You reuse the same result multiple times
-- 2. The query has multiple logical steps
-- 3. You want readable, self-documenting code
WITH dept_avg AS (
    SELECT dept, AVG(salary) AS avg_sal FROM employees GROUP BY dept
)
SELECT e.name, e.salary, d.avg_sal
FROM employees e
JOIN dept_avg d ON e.dept = d.dept
WHERE e.salary > d.avg_sal * 1.2;
```

---

## 🔶 Multi-Step CTEs (Pipeline-Style Queries)

```sql
-- Business question: "Who are the top customers by revenue who joined in 2023
--                    and have not ordered in the last 90 days?"

WITH
-- Step 1: Customers who joined in 2023
new_customers AS (
    SELECT user_id, name, email, joined_at
    FROM users
    WHERE YEAR(joined_at) = 2023
),

-- Step 2: Their total revenue
customer_revenue AS (
    SELECT
        o.user_id,
        SUM(oi.quantity * oi.unit_price) AS total_revenue,
        MAX(o.order_date)                AS last_order_date
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    WHERE o.status = 'delivered'
    GROUP BY o.user_id
),

-- Step 3: Join and filter
at_risk_high_value AS (
    SELECT
        n.user_id, n.name, n.email,
        cr.total_revenue,
        cr.last_order_date,
        DATEDIFF(CURDATE(), cr.last_order_date) AS days_inactive
    FROM new_customers n
    JOIN customer_revenue cr ON n.user_id = cr.user_id
    WHERE cr.total_revenue > 10000
      AND DATEDIFF(CURDATE(), cr.last_order_date) > 90
)

-- Final output
SELECT *
FROM at_risk_high_value
ORDER BY total_revenue DESC
LIMIT 50;
```

---

## 🔵 Recursive CTEs (MySQL 8.0+)

Recursive CTEs allow querying **hierarchical or graph data** — employee org charts, category trees, date series.

```sql
WITH RECURSIVE cte_name AS (
    -- Base case (anchor member)
    SELECT ...
    
    UNION ALL
    
    -- Recursive case (references cte_name)
    SELECT ... FROM cte_name WHERE [stop condition]
)
SELECT * FROM cte_name;
```

### Use Case 1: Employee Hierarchy (Org Chart)

```sql
WITH RECURSIVE org_chart AS (
    -- Base: top-level employees (no manager)
    SELECT emp_id, name, manager_id, dept, salary, 0 AS level, CAST(name AS CHAR(500)) AS path
    FROM employees
    WHERE manager_id IS NULL

    UNION ALL

    -- Recursive: each employee's reports
    SELECT e.emp_id, e.name, e.manager_id, e.dept, e.salary,
           oc.level + 1,
           CONCAT(oc.path, ' → ', e.name)
    FROM employees e
    JOIN org_chart oc ON e.manager_id = oc.emp_id
)
SELECT
    REPEAT('  ', level) AS indent,
    emp_id,
    name,
    level AS hierarchy_level,
    path  AS reporting_chain
FROM org_chart
ORDER BY path;
```

### Use Case 2: Generate Date Series

```sql
-- Generate every date for 2024
WITH RECURSIVE date_series AS (
    SELECT DATE('2024-01-01') AS dt
    
    UNION ALL
    
    SELECT DATE_ADD(dt, INTERVAL 1 DAY)
    FROM date_series
    WHERE dt < '2024-12-31'
)
SELECT dt FROM date_series;

-- Left join to find days with no orders (gaps analysis)
WITH RECURSIVE date_series AS (
    SELECT DATE('2024-01-01') AS dt
    UNION ALL
    SELECT DATE_ADD(dt, INTERVAL 1 DAY) FROM date_series WHERE dt < '2024-12-31'
)
SELECT
    ds.dt,
    COALESCE(COUNT(o.order_id), 0) AS order_count
FROM date_series ds
LEFT JOIN orders o ON DATE(o.order_date) = ds.dt
GROUP BY ds.dt
ORDER BY ds.dt;
```

### Use Case 3: Category Tree (e-commerce)

```sql
WITH RECURSIVE category_tree AS (
    -- Root categories (no parent)
    SELECT category_id, name, parent_id, 0 AS depth,
           CAST(name AS CHAR(500)) AS full_path
    FROM categories
    WHERE parent_id IS NULL

    UNION ALL

    SELECT c.category_id, c.name, c.parent_id,
           ct.depth + 1,
           CONCAT(ct.full_path, ' > ', c.name)
    FROM categories c
    JOIN category_tree ct ON c.parent_id = ct.category_id
)
SELECT * FROM category_tree ORDER BY full_path;
```

### Use Case 4: Number Series (Utility)

```sql
WITH RECURSIVE numbers AS (
    SELECT 1 AS n
    UNION ALL
    SELECT n + 1 FROM numbers WHERE n < 100
)
SELECT n FROM numbers;
-- Generates 1 to 100

-- Practical: generate test data
INSERT INTO dim_date (full_date, year, month_num, day_num)
WITH RECURSIVE dates AS (
    SELECT DATE('2020-01-01') AS dt
    UNION ALL
    SELECT DATE_ADD(dt, INTERVAL 1 DAY) FROM dates WHERE dt < '2030-12-31'
)
SELECT dt, YEAR(dt), MONTH(dt), DAY(dt) FROM dates;
```

---

## 🔧 CTE with Window Functions (Power Combo)

```sql
-- Find employees who jumped salary tiers between years
WITH yearly_salary AS (
    SELECT
        emp_id,
        YEAR(review_date) AS year,
        salary,
        LAG(salary) OVER (PARTITION BY emp_id ORDER BY YEAR(review_date)) AS prev_salary
    FROM salary_history
),
salary_changes AS (
    SELECT *,
        salary - prev_salary AS salary_change,
        ROUND((salary - prev_salary) / prev_salary * 100, 2) AS pct_change
    FROM yearly_salary
    WHERE prev_salary IS NOT NULL
)
SELECT * FROM salary_changes
WHERE pct_change >= 20   -- significant raise
ORDER BY pct_change DESC;
```

---

## ⚠️ Recursive CTE Safety

```sql
-- Prevent infinite loops with MAXRECURSION hint or depth limit
WITH RECURSIVE safe_tree AS (
    SELECT emp_id, name, manager_id, 0 AS depth
    FROM employees WHERE manager_id IS NULL
    
    UNION ALL
    
    SELECT e.emp_id, e.name, e.manager_id, t.depth + 1
    FROM employees e
    JOIN safe_tree t ON e.manager_id = t.emp_id
    WHERE t.depth < 50   -- safety: stop at 50 levels deep
)
SELECT * FROM safe_tree;

-- MySQL system variable (default 1000)
SET SESSION cte_max_recursion_depth = 5000;
```

---

## 💼 Interview Questions

1. **What is a CTE and how does it differ from a subquery?**
   > A CTE (WITH clause) is a named temporary result set. Subqueries are anonymous and embedded. CTEs are reusable within the same query, more readable, and support recursion.

2. **When would you use a recursive CTE?**
   > For hierarchical data (org charts, category trees), generating series (dates, numbers), or graph traversal.

3. **Does a CTE improve performance in MySQL?**
   > Not inherently — MySQL 8.0 CTEs are not materialized by default (treated similarly to subqueries). Performance depends on the underlying query. They primarily improve readability.

4. **How do you prevent infinite recursion in a recursive CTE?**
   > Add a termination condition in the WHERE clause of the recursive part (e.g., `WHERE depth < 50`). MySQL also has `cte_max_recursion_depth` system variable (default 1000).

5. **Scenario:** *"You need to find all subordinates of a given manager, regardless of depth."*
   ```sql
   WITH RECURSIVE subordinates AS (
       SELECT emp_id, name FROM employees WHERE emp_id = 5  -- manager ID
       UNION ALL
       SELECT e.emp_id, e.name FROM employees e
       JOIN subordinates s ON e.manager_id = s.emp_id
   )
   SELECT * FROM subordinates;
   ```

---

## 🔗 Related Topics

- [[20 - Subqueries]] — CTEs vs subqueries
- [[23 - Window Functions]] — CTEs + window functions combo
- [[22 - Joins]] — CTEs to simplify multi-join queries
- [[34 - Data Engineering Projects]] — CTEs in real pipeline queries
- [[39 - SQL Practice Problems]] — P29 uses recursive CTE
