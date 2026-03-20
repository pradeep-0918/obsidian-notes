# 42 — Company-Specific Interview Prep

#interview #company #strategy

---

## 🎯 Interview Strategy Overview

Different companies test SQL at different depths. This guide maps what each major company focuses on so you can prioritize your preparation.

---

## 🏢 Infosys

**Round Structure:** Written test → Technical interview → HR

**Focus Areas:**
- SQL basics: CRUD, JOINs, GROUP BY
- Normalization theory (1NF, 2NF, 3NF) — very common
- ACID properties
- Basic subqueries
- DDL/DML/TCL/DCL classification

**Most-Asked Questions:**
```sql
-- 1. Write a query to find employees with salary above average
SELECT * FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);

-- 2. Display the Nth highest salary
SELECT salary FROM employees
ORDER BY salary DESC LIMIT 1 OFFSET N-1;

-- 3. Find duplicates
SELECT col, COUNT(*) FROM table GROUP BY col HAVING COUNT(*) > 1;

-- 4. Self-join for manager-employee
SELECT e.name AS emp, m.name AS manager
FROM employees e LEFT JOIN employees m ON e.manager_id = m.emp_id;
```

**Theory Questions to Prepare:**
- What is normalization? Explain 1NF, 2NF, 3NF with examples
- What are ACID properties?
- Difference between DELETE, TRUNCATE, DROP
- What is a primary key vs foreign key?
- Difference between WHERE and HAVING

**Preparation Notes:**
→ [[31 - Normalization]], [[30 - ACID Properties]], [[05 - DDL DML TCL DCL]], [[11 - Keys]]

---

## 🏢 TCS (Tata Consultancy Services)

**Round Structure:** TCS NQT (Ninja/Digital) → Technical interview → Manager round

**Focus Areas:**
- Intermediate to advanced SQL
- Window functions (ROW_NUMBER, RANK, LAG/LEAD)
- Complex JOINs
- Indexes and performance
- Stored procedures (Digital track)

**Most-Asked Questions:**
```sql
-- 1. Rank employees by salary in each department
SELECT name, dept, salary,
    RANK() OVER (PARTITION BY dept ORDER BY salary DESC) AS dept_rank
FROM employees;

-- 2. Find employees not in a specific department
SELECT * FROM employees WHERE dept NOT IN ('HR', 'Admin');

-- 3. Cumulative sum
SELECT order_date, amount,
    SUM(amount) OVER (ORDER BY order_date) AS running_total
FROM orders;

-- 4. Month-over-month growth
SELECT month, revenue,
    LAG(revenue) OVER (ORDER BY month) AS prev_revenue,
    ROUND((revenue - LAG(revenue) OVER (ORDER BY month)) /
          LAG(revenue) OVER (ORDER BY month) * 100, 2) AS growth_pct
FROM monthly_sales;
```

**Focus Preparation:**
→ [[23 - Window Functions]], [[22 - Joins]], [[25 - Indexes]], [[20 - Subqueries]]

---

## 🏢 Zoho

**Round Structure:** Programming test → SQL test → Technical → HR

**Focus areas:**
- String functions (heavily tested)
- Date functions
- Complex filtering with REGEX
- Performance optimization
- Stored procedures and triggers

**Most-Asked Questions:**
```sql
-- 1. Extract domain from email
SELECT SUBSTRING(email, LOCATE('@', email) + 1) AS domain FROM users;

-- 2. Find names starting with a vowel
SELECT name FROM employees WHERE name REGEXP '^[AEIOUaeiou]';

-- 3. Standardize phone numbers
SELECT REGEXP_REPLACE(phone, '[^0-9]', '') AS clean_phone FROM contacts;

-- 4. Date: customers active in last 6 months
SELECT * FROM users
WHERE last_login >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH);

-- 5. Pivot: count orders by status for each month
SELECT
    DATE_FORMAT(order_date, '%Y-%m') AS month,
    SUM(status = 'delivered') AS delivered,
    SUM(status = 'cancelled') AS cancelled
FROM orders GROUP BY month;
```

**Focus Preparation:**
→ [[17 - String Functions]], [[19 - Regex]], [[18 - Date and Time]], [[15 - CASE WHEN]]

---

## 🏢 Wipro

**Round Structure:** WILP/NLTH exam → Technical → HR

**Focus Areas:**
- SQL joins and subqueries
- Aggregation
- Index concepts (theory)
- Normalization
- Basic query optimization

**Most-Asked Questions:**
```sql
-- 1. Inner join with filter
SELECT e.name, d.dept_name, e.salary
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id
WHERE e.salary > 50000;

-- 2. Count employees per department
SELECT dept, COUNT(*) AS emp_count FROM employees GROUP BY dept;

-- 3. Find second highest salary
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

**Focus Preparation:**
→ [[22 - Joins]], [[12 - Aggregation Functions]], [[20 - Subqueries]]

---

## 🏢 Accenture

**Focus Areas:**
- All JOINs
- Aggregation and GROUP BY
- Subqueries
- Basic window functions
- Data modeling questions

---

## 🏢 Startup / Product Companies (FAANG-adjacent)

**Round Structure:** 2-3 technical rounds, each 45-60 minutes

**Focus Areas:**
- Complex multi-table queries
- Window functions (advanced)
- Performance optimization (EXPLAIN, indexes)
- Schema design questions
- Scenario-based problems

**Typical Problems:**
```sql
-- "Find the top 3 products by revenue in each category for the past year"
WITH ranked AS (
    SELECT
        p.category, p.name,
        SUM(oi.quantity * oi.unit_price) AS revenue,
        DENSE_RANK() OVER (PARTITION BY p.category
                           ORDER BY SUM(oi.quantity*oi.unit_price) DESC) AS dr
    FROM products p
    JOIN order_items oi ON p.product_id = oi.product_id
    JOIN orders o ON oi.order_id = o.order_id
    WHERE o.status='delivered' AND YEAR(o.order_date)=YEAR(CURDATE())
    GROUP BY p.category, p.product_id, p.name
)
SELECT * FROM ranked WHERE dr <= 3;

-- "Design a schema for [ride-sharing / food delivery / social network]"
-- "This query is slow — here's the EXPLAIN output, optimize it"
-- "How would you handle SCD in your customer dimension?"
```

**Focus Preparation:**
→ [[23 - Window Functions]], [[38 - Query Optimization Patterns]], [[26 - EXPLAIN ANALYZE]], [[32 - Slowly Changing Dimensions]]

---

## 📋 Pre-Interview Checklist

### Day Before
- [ ] Review [[36 - SQL Cheat Sheet]] — all syntax fresh
- [ ] Do 5 problems from [[39 - SQL Practice Problems]]
- [ ] Review [[35 - Interview Questions Master]] — Tier 1 + 2
- [ ] Know your SCD2 explanation cold
- [ ] Practice explaining ACID and Normalization out loud

### Hour Before
- [ ] Re-read your top 10 key concepts
- [ ] Remember: always mention indexing for performance questions
- [ ] Remember: always mention NULL handling for filtering questions
- [ ] Remember: always ask clarifying questions ("Should I include cancelled orders?")

### During the Interview
- [ ] **Think out loud** — explain your approach before writing
- [ ] **Start simple**, then add complexity
- [ ] **Mention optimizations** even if not asked ("I'd add an index here")
- [ ] **Handle edge cases** out loud ("If user_id is NULL, NOT IN would fail, so I'd use NOT EXISTS")
- [ ] **Know multiple approaches** ("I could also do this with a window function...")

---

## 💬 Phrases That Impress Interviewers

- *"I'd check with EXPLAIN first before assuming I need an index"*
- *"For large tables, I'd consider partitioning the date column"*
- *"NOT IN has a NULL gotcha — I prefer NOT EXISTS for safety"*
- *"This is a correlated subquery which runs N times — a JOIN would be more efficient"*
- *"For historical accuracy, we'd need SCD Type 2 rather than an overwrite"*
- *"I'd wrap this in a transaction to ensure atomicity"*

---

## 🔗 Related Topics

- [[35 - Interview Questions Master]] — Master question bank
- [[36 - SQL Cheat Sheet]] — Quick syntax reference
- [[39 - SQL Practice Problems]] — Practice problems by difficulty
- [[34 - Data Engineering Projects]] — Show real project experience
