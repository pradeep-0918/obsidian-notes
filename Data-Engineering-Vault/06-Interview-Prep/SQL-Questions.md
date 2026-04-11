# 🎯 SQL Interview Questions — Complete Guide
#interview #sql #questions #answers

> 50+ SQL questions with complete solutions. Covers everything asked in FAANG and top data companies.

---

## 📚 Question Categories

1. [Basic Queries](#basic)
2. [Aggregations & GROUP BY](#aggregations)
3. [JOINs](#joins)
4. [Window Functions](#windows)
5. [CTEs & Subqueries](#ctes)
6. [Data Engineering Specific](#de-specific)

---

## Basic Queries {#basic}

### Q1: Find all customers who placed more than 3 orders

```sql
SELECT customer_id, COUNT(*) AS order_count
FROM orders
GROUP BY customer_id
HAVING COUNT(*) > 3
ORDER BY order_count DESC;
```

### Q2: Find the 2nd highest salary (classic)

```sql
-- Method 1: Subquery
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 2: Window function (better)
SELECT salary AS second_highest
FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
) ranked
WHERE rnk = 2
LIMIT 1;

-- Method 3: OFFSET
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

### Q3: Delete duplicate rows, keeping the one with the lowest id

```sql
-- PostgreSQL
DELETE FROM users
WHERE id NOT IN (
    SELECT MIN(id)
    FROM users
    GROUP BY email  -- The column that defines duplicates
);

-- Alternative (more efficient for large tables)
WITH duplicates AS (
    SELECT id,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) AS rn
    FROM users
)
DELETE FROM users
WHERE id IN (SELECT id FROM duplicates WHERE rn > 1);
```

---

## Aggregations {#aggregations}

### Q4: Monthly revenue with MoM growth rate

```sql
WITH monthly AS (
    SELECT
        DATE_TRUNC('month', order_date) AS month,
        SUM(amount) AS revenue
    FROM orders
    WHERE status = 'completed'
    GROUP BY DATE_TRUNC('month', order_date)
),
with_prev AS (
    SELECT
        month,
        revenue,
        LAG(revenue) OVER (ORDER BY month) AS prev_revenue
    FROM monthly
)
SELECT
    TO_CHAR(month, 'YYYY-MM') AS month,
    revenue,
    prev_revenue,
    ROUND(100.0 * (revenue - prev_revenue) / NULLIF(prev_revenue, 0), 2) AS mom_growth_pct
FROM with_prev
ORDER BY month;
```

### Q5: Rolling 7-day average

```sql
SELECT
    order_date,
    daily_revenue,
    AVG(daily_revenue) OVER (
        ORDER BY order_date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) AS rolling_7d_avg
FROM (
    SELECT
        order_date,
        SUM(amount) AS daily_revenue
    FROM orders
    GROUP BY order_date
) daily
ORDER BY order_date;
```

### Q6: Percentage of total within group

```sql
SELECT
    region,
    product_category,
    SUM(revenue) AS category_revenue,
    SUM(SUM(revenue)) OVER (PARTITION BY region) AS region_total,
    ROUND(100.0 * SUM(revenue) / SUM(SUM(revenue)) OVER (PARTITION BY region), 2) AS pct_of_region
FROM sales
GROUP BY region, product_category
ORDER BY region, category_revenue DESC;
```

---

## JOINs {#joins}

### Q7: Customers who have never placed an order

```sql
-- Method 1: LEFT JOIN
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.customer_id IS NULL;

-- Method 2: NOT EXISTS (often faster)
SELECT customer_id, name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o
    WHERE o.customer_id = c.customer_id
);

-- Method 3: NOT IN (careful with NULLs!)
SELECT customer_id, name
FROM customers
WHERE customer_id NOT IN (
    SELECT customer_id FROM orders WHERE customer_id IS NOT NULL
);
```

### Q8: Self join — Find employees who earn more than their manager

```sql
SELECT
    e.name AS employee,
    e.salary AS employee_salary,
    m.name AS manager,
    m.salary AS manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

### Q9: Find all products that were never sold

```sql
SELECT p.product_id, p.product_name
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;
```

---

## Window Functions {#windows}

### Q10: Rank customers by revenue within each region

```sql
SELECT
    region,
    customer_name,
    total_revenue,
    RANK() OVER (PARTITION BY region ORDER BY total_revenue DESC) AS revenue_rank,
    DENSE_RANK() OVER (PARTITION BY region ORDER BY total_revenue DESC) AS dense_rank,
    ROW_NUMBER() OVER (PARTITION BY region ORDER BY total_revenue DESC) AS row_num,
    PERCENT_RANK() OVER (PARTITION BY region ORDER BY total_revenue DESC) AS pct_rank
FROM customer_revenue;
```

**Difference: RANK vs DENSE_RANK vs ROW_NUMBER**
```
Revenue: 100, 90, 90, 80

RANK:        1, 2, 2, 4   (skip 3)
DENSE_RANK:  1, 2, 2, 3   (no skip)
ROW_NUMBER:  1, 2, 3, 4   (always unique)
```

### Q11: Running total and moving average

```sql
SELECT
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) AS running_total,
    AVG(amount) OVER (
        ORDER BY order_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3day,
    amount - LAG(amount, 1, amount) OVER (ORDER BY order_date) AS daily_change
FROM daily_orders
ORDER BY order_date;
```

### Q12: First and last purchase per customer

```sql
SELECT DISTINCT
    customer_id,
    FIRST_VALUE(order_date) OVER (
        PARTITION BY customer_id ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS first_purchase,
    LAST_VALUE(order_date) OVER (
        PARTITION BY customer_id ORDER BY order_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) AS last_purchase,
    COUNT(*) OVER (PARTITION BY customer_id) AS total_orders
FROM orders;
```

---

## CTEs & Subqueries {#ctes}

### Q13: Customer retention cohort analysis

```sql
WITH first_purchase AS (
    SELECT customer_id, DATE_TRUNC('month', MIN(order_date)) AS cohort_month
    FROM orders GROUP BY customer_id
),
monthly_activity AS (
    SELECT
        fp.customer_id,
        fp.cohort_month,
        DATE_TRUNC('month', o.order_date) AS activity_month,
        EXTRACT(MONTH FROM AGE(DATE_TRUNC('month', o.order_date), fp.cohort_month)) AS month_number
    FROM orders o
    JOIN first_purchase fp ON o.customer_id = fp.customer_id
),
cohort_sizes AS (
    SELECT cohort_month, COUNT(*) AS cohort_size
    FROM first_purchase GROUP BY cohort_month
)
SELECT
    ma.cohort_month,
    ma.month_number,
    COUNT(DISTINCT ma.customer_id) AS active_customers,
    cs.cohort_size,
    ROUND(100.0 * COUNT(DISTINCT ma.customer_id) / cs.cohort_size, 1) AS retention_rate
FROM monthly_activity ma
JOIN cohort_sizes cs ON ma.cohort_month = cs.cohort_month
GROUP BY ma.cohort_month, ma.month_number, cs.cohort_size
ORDER BY ma.cohort_month, ma.month_number;
```

### Q14: Find gaps in a sequence (detect missing order IDs)

```sql
WITH sequence AS (
    SELECT order_id, LEAD(order_id) OVER (ORDER BY order_id) AS next_id
    FROM orders
)
SELECT order_id + 1 AS gap_start, next_id - 1 AS gap_end
FROM sequence
WHERE next_id - order_id > 1;
```

---

## Data Engineering Specific {#de-specific}

### Q15: Slowly Changing Dimension query — get current value

```sql
SELECT customer_id, segment, region
FROM dim_customer
WHERE is_current = TRUE;

-- Or using effective dates
SELECT customer_id, segment, region
FROM dim_customer
WHERE effective_from <= CURRENT_DATE
  AND (effective_to IS NULL OR effective_to >= CURRENT_DATE);
```

### Q16: Find duplicate records

```sql
-- Count duplicates
SELECT email, COUNT(*) AS count
FROM customers
GROUP BY email
HAVING COUNT(*) > 1
ORDER BY count DESC;

-- See full duplicate records
SELECT *
FROM customers
WHERE email IN (
    SELECT email FROM customers GROUP BY email HAVING COUNT(*) > 1
)
ORDER BY email, created_at;
```

### Q17: Pivot table (rows to columns)

```sql
-- Monthly sales per region, pivoted
SELECT
    year,
    SUM(CASE WHEN region = 'North' THEN revenue ELSE 0 END) AS north,
    SUM(CASE WHEN region = 'South' THEN revenue ELSE 0 END) AS south,
    SUM(CASE WHEN region = 'East' THEN revenue ELSE 0 END) AS east,
    SUM(CASE WHEN region = 'West' THEN revenue ELSE 0 END) AS west
FROM regional_sales
GROUP BY year
ORDER BY year;
```

### Q18: Top N per group (top 3 products per category)

```sql
WITH ranked AS (
    SELECT
        category,
        product_name,
        total_sales,
        ROW_NUMBER() OVER (PARTITION BY category ORDER BY total_sales DESC) AS rn
    FROM product_sales
)
SELECT category, product_name, total_sales
FROM ranked
WHERE rn <= 3
ORDER BY category, rn;
```

### Q19: Session-ize user activity (30-min gap = new session)

```sql
WITH activity AS (
    SELECT
        user_id,
        event_time,
        LAG(event_time) OVER (PARTITION BY user_id ORDER BY event_time) AS prev_time,
        EXTRACT(EPOCH FROM (event_time - LAG(event_time) OVER (
            PARTITION BY user_id ORDER BY event_time
        ))) / 60 AS gap_minutes
    FROM user_events
),
session_boundaries AS (
    SELECT
        user_id,
        event_time,
        SUM(CASE WHEN gap_minutes > 30 OR gap_minutes IS NULL THEN 1 ELSE 0 END)
            OVER (PARTITION BY user_id ORDER BY event_time) AS session_id
    FROM activity
)
SELECT
    user_id,
    session_id,
    MIN(event_time) AS session_start,
    MAX(event_time) AS session_end,
    EXTRACT(EPOCH FROM MAX(event_time) - MIN(event_time)) / 60 AS session_minutes,
    COUNT(*) AS events_in_session
FROM session_boundaries
GROUP BY user_id, session_id
ORDER BY user_id, session_start;
```

### Q20: Date spine (generate all dates in a range)

```sql
-- PostgreSQL: generate_series
SELECT
    g::DATE AS date,
    EXTRACT(DOW FROM g) AS day_of_week,
    EXTRACT(WEEK FROM g) AS week,
    EXTRACT(MONTH FROM g) AS month,
    EXTRACT(YEAR FROM g) AS year
FROM generate_series(
    '2024-01-01'::DATE,
    '2024-12-31'::DATE,
    '1 day'::INTERVAL
) AS g
ORDER BY date;
```

---

## 🔑 Key SQL Interview Tips

1. **Always clarify** before coding: "What should happen with NULLs? Duplicates?"
2. **Think out loud** — interviewers want to see your process
3. **Start with the logic**, then write SQL
4. **Know EXPLAIN** — be ready to discuss query optimization
5. **Practice on real databases** — SQLFiddle, db-fiddle.com, local PostgreSQL

### Common Mistakes to Avoid
```sql
-- MISTAKE: WHERE with aggregate (use HAVING)
-- WRONG:
SELECT customer_id, COUNT(*) FROM orders WHERE COUNT(*) > 5 GROUP BY customer_id;
-- RIGHT:
SELECT customer_id, COUNT(*) FROM orders GROUP BY customer_id HAVING COUNT(*) > 5;

-- MISTAKE: NOT IN with NULLs
-- If subquery returns any NULL, the entire NOT IN returns no rows!
-- WRONG (can return 0 rows silently):
SELECT * FROM orders WHERE customer_id NOT IN (SELECT customer_id FROM blacklist);
-- RIGHT:
SELECT * FROM orders WHERE customer_id NOT IN (
    SELECT customer_id FROM blacklist WHERE customer_id IS NOT NULL
);
-- BEST:
SELECT * FROM orders o WHERE NOT EXISTS (
    SELECT 1 FROM blacklist b WHERE b.customer_id = o.customer_id
);
```

---

*Related: [[06-Interview-Prep/System-Design]] | [[06-Interview-Prep/Case-Studies]] | [[09-Cheat-Sheets]] | Back to [[00-Index]]*
