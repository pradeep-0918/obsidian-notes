# 23 — Window Functions

#advanced #interview #performance

---

## 🧠 Introduction

Window functions perform **calculations across a set of related rows** without collapsing them (unlike GROUP BY). They're one of the most powerful and interview-critical SQL features for Data Engineers.

> The key difference: GROUP BY collapses rows into one per group. Window functions keep all rows AND add the calculation alongside each row.

---

## 📐 Syntax

```sql
function_name() OVER (
    [PARTITION BY column]   -- divide into groups (like GROUP BY but keeps rows)
    [ORDER BY column]       -- define row order within the window
    [ROWS/RANGE BETWEEN ... AND ...]  -- frame specification
)
```

---

## 🏷️ Ranking Functions

### ROW_NUMBER — Unique sequential number per row

```sql
-- Rank employees by salary within each department
SELECT
    name,
    dept,
    salary,
    ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS row_num
FROM employees;
-- Every row gets a unique number 1,2,3... within dept
```

### RANK — Leaves gaps after ties

```sql
SELECT
    name, salary,
    RANK() OVER (ORDER BY salary DESC) AS rnk
FROM employees;
-- If two employees earn 80000: both get rank 2, next gets rank 4
```

### DENSE_RANK — No gaps after ties

```sql
SELECT
    name, salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rnk
FROM employees;
-- If two employees earn 80000: both get rank 2, next gets rank 3
```

### NTILE — Divide into N buckets

```sql
-- Divide employees into 4 salary quartiles
SELECT
    name, salary,
    NTILE(4) OVER (ORDER BY salary) AS quartile
FROM employees;
-- quartile 1 = lowest 25%, quartile 4 = highest 25%
```

### PERCENT_RANK and CUME_DIST

```sql
SELECT
    name, salary,
    ROUND(PERCENT_RANK() OVER (ORDER BY salary) * 100, 2) AS percentile,
    ROUND(CUME_DIST()    OVER (ORDER BY salary) * 100, 2) AS cumulative_pct
FROM employees;
```

---

## 🔢 Value Functions

### LAG — Access previous row's value

```sql
-- Month-over-month revenue change
SELECT
    month,
    revenue,
    LAG(revenue, 1) OVER (ORDER BY month)   AS prev_month_revenue,
    revenue - LAG(revenue, 1) OVER (ORDER BY month) AS mom_change,
    ROUND(
        (revenue - LAG(revenue, 1) OVER (ORDER BY month))
        / LAG(revenue, 1) OVER (ORDER BY month) * 100
    , 2) AS mom_pct_change
FROM monthly_sales;
```

### LEAD — Access next row's value

```sql
-- Next month's target vs current
SELECT
    month,
    revenue,
    LEAD(revenue, 1) OVER (ORDER BY month) AS next_month_revenue
FROM monthly_sales;
```

### FIRST_VALUE / LAST_VALUE

```sql
-- Highest salary in the department alongside each employee
SELECT
    name, dept, salary,
    FIRST_VALUE(salary) OVER (
        PARTITION BY dept ORDER BY salary DESC
    ) AS highest_in_dept,
    FIRST_VALUE(name) OVER (
        PARTITION BY dept ORDER BY salary DESC
    ) AS top_earner_in_dept
FROM employees;
```

---

## 📊 Aggregate Window Functions

These are regular aggregates used as window functions — don't collapse rows!

```sql
-- Running total (cumulative sum)
SELECT
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM orders
WHERE status = 'delivered';

-- Running average
SELECT
    order_date,
    amount,
    ROUND(AVG(amount) OVER (ORDER BY order_date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW), 2)
    AS rolling_7day_avg
FROM daily_sales;

-- Percentage of partition total
SELECT
    dept,
    name,
    salary,
    ROUND(salary / SUM(salary) OVER (PARTITION BY dept) * 100, 2) AS pct_of_dept_payroll
FROM employees;

-- Compare to department average
SELECT
    name, dept, salary,
    ROUND(AVG(salary) OVER (PARTITION BY dept), 2) AS dept_avg,
    salary - AVG(salary) OVER (PARTITION BY dept) AS diff_from_avg
FROM employees;
```

---

## 🔲 Frame Specification (ROWS vs RANGE)

```sql
-- Moving average (last 3 rows including current)
SELECT
    date,
    sales,
    AVG(sales) OVER (
        ORDER BY date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3
FROM daily_sales;

-- Running total from start to current
SUM(amount) OVER (ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)

-- Total of all rows in window (no frame)
SUM(amount) OVER (PARTITION BY dept)
```

---

## 🌍 Real-World Use Case — Ride-Sharing Analytics

```sql
-- Driver performance ranking with peer comparison
SELECT
    driver_id,
    month,
    trips_completed,
    avg_rating,
    earnings,
    -- Rank within month
    RANK() OVER (PARTITION BY month ORDER BY earnings DESC) AS earnings_rank,
    -- Running total of trips
    SUM(trips_completed) OVER (
        PARTITION BY driver_id ORDER BY month
        ROWS UNBOUNDED PRECEDING
    ) AS cumulative_trips,
    -- Month-over-month earnings change
    earnings - LAG(earnings) OVER (PARTITION BY driver_id ORDER BY month) AS mom_earnings_change,
    -- Percentile in earnings
    ROUND(PERCENT_RANK() OVER (PARTITION BY month ORDER BY earnings) * 100, 1) AS earnings_percentile
FROM driver_monthly_stats;
```

---

## 🔍 Top-N Per Group (Classic Interview Pattern)

```sql
-- Top 3 highest-paid employees per department
SELECT *
FROM (
    SELECT
        name, dept, salary,
        ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS rn
    FROM employees
) ranked
WHERE rn <= 3;

-- Second highest salary per department
SELECT dept, name, salary
FROM (
    SELECT name, dept, salary,
           DENSE_RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS dr
    FROM employees
) r
WHERE dr = 2;
```

---

## 💼 Interview Questions

1. **What is the difference between ROW_NUMBER, RANK, and DENSE_RANK?**
   > All rank rows. ROW_NUMBER: always unique, no ties. RANK: ties get same rank, skips next rank (1,2,2,4). DENSE_RANK: ties get same rank, no gaps (1,2,2,3).

2. **What is PARTITION BY in window functions?**
   > Divides result set into groups for the window calculation — like GROUP BY but keeps all rows.

3. **How do you find the top N records per group?**
   > Use ROW_NUMBER() OVER (PARTITION BY group ORDER BY value DESC), wrap in subquery, filter WHERE rn <= N.

4. **What is LAG and LEAD used for?**
   > LAG accesses previous rows; LEAD accesses future rows. Used for period-over-period comparisons (MoM revenue change).

5. **Scenario:** *"Calculate running total of orders per customer, ordered by date."*
   ```sql
   SELECT user_id, order_date, amount,
     SUM(amount) OVER (PARTITION BY user_id ORDER BY order_date) AS running_total
   FROM orders;
   ```

---

## 🔗 Related Topics

- [[12 - Aggregation Functions]] — Regular aggregates vs window aggregates
- [[20 - Subqueries]] — Used to filter window function results
- [[22 - Joins]] — Window functions work on joined result sets
- [[26 - EXPLAIN ANALYZE]] — Window function performance
- [[15 - CASE WHEN]] — CASE inside window functions
