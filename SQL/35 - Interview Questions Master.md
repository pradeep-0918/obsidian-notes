# 🎯 SQL Interview Questions — Master Sheet

#interview #revision #mustknow

> **How to use this file:** Review before every interview. Each question links to the full topic note. Questions are grouped by difficulty and type.

---

## 🟢 Tier 1 — Fresher / 0–1 Year (Always Asked)

### Q1. What is the difference between DELETE, TRUNCATE, and DROP?

| | DELETE | TRUNCATE | DROP |
|-|--------|----------|------|
| Category | DML | DDL | DDL |
| WHERE clause | ✅ Yes | ❌ No | ❌ No |
| Rollback | ✅ Yes | ❌ No (MySQL) | ❌ No |
| Resets AUTO_INCREMENT | ❌ No | ✅ Yes | N/A |
| Removes structure | ❌ No | ❌ No | ✅ Yes |
| Speed | Slow (logs each row) | Fast | Fastest |

→ See [[05 - DDL DML TCL DCL]]

---

### Q2. What is the difference between WHERE and HAVING?

| | WHERE | HAVING |
|-|-------|--------|
| Filters | Individual rows | Groups |
| Timing | Before GROUP BY | After GROUP BY |
| Aggregates | ❌ Cannot use | ✅ Can use |

```sql
-- WHERE filters rows first, HAVING filters groups after
SELECT dept, AVG(salary) AS avg_sal
FROM employees
WHERE hire_date > '2020-01-01'   -- row-level filter
GROUP BY dept
HAVING avg_sal > 50000;          -- group-level filter
```

→ See [[12 - Aggregation Functions]]

---

### Q3. What are the types of JOINs in SQL?

```
INNER JOIN  → only matching rows from BOTH tables
LEFT JOIN   → ALL from left + matching from right (NULL if no match)
RIGHT JOIN  → ALL from right + matching from left (NULL if no match)
FULL JOIN   → ALL from both (emulated in MySQL via UNION)
CROSS JOIN  → Cartesian product (every row × every row)
SELF JOIN   → Table joined with itself
```

→ See [[22 - Joins]]

---

### Q4. What is a PRIMARY KEY vs UNIQUE KEY?

| | PRIMARY KEY | UNIQUE KEY |
|-|-------------|------------|
| NULL allowed | ❌ No | ✅ One NULL |
| Per table | One only | Multiple |
| Auto-index | ✅ Yes | ✅ Yes |
| Purpose | Row identifier | Alternate unique identifier |

→ See [[11 - Keys]], [[10 - SQL Constraints]]

---

### Q5. What is NULL? How do you check for it?

> NULL = unknown / missing. It is NOT zero, NOT empty string.

```sql
-- WRONG
WHERE column = NULL     -- always evaluates to UNKNOWN

-- CORRECT
WHERE column IS NULL
WHERE column IS NOT NULL
```

→ See [[16 - NULL Handling]]

---

### Q6. What is the difference between UNION and UNION ALL?

```sql
UNION     → removes duplicates (slower — requires dedup sort)
UNION ALL → keeps all rows (faster — no dedup)
```

Use UNION ALL whenever you know rows are distinct or duplicates are acceptable.

→ See [[24 - UNION and UNION ALL]]

---

### Q7. What are constraints in SQL?

```
NOT NULL    → column must have a value
UNIQUE      → no duplicate values
PRIMARY KEY → NOT NULL + UNIQUE, one per table
FOREIGN KEY → references another table's PK
CHECK       → value must satisfy a condition
DEFAULT     → value assigned when none provided
```

→ See [[10 - SQL Constraints]]

---

### Q8. What is an INDEX? Why use it?

> An index is a data structure (B-Tree) that speeds up data retrieval. Without it, MySQL scans every row (O(n)). With it, MySQL uses tree traversal (O(log n)).

**Trade-off:** Faster SELECTs, slower INSERTs/UPDATEs.

→ See [[25 - Indexes]]

---

### Q9. What does `SELECT *` return?

> All columns from the table. Avoid in production — wastes bandwidth, breaks if schema changes, prevents covering indexes.

---

### Q10. What is ACID?

```
A → Atomicity   : All or nothing
C → Consistency : Valid state → valid state
I → Isolation   : Concurrent transactions don't interfere
D → Durability  : Committed data survives crashes
```

→ See [[30 - ACID Properties]]

---

## 🟡 Tier 2 — Junior / 1–3 Years (Commonly Asked)

### Q11. Find the second highest salary

```sql
-- Method 1: Subquery
SELECT MAX(salary) FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 2: LIMIT/OFFSET
SELECT DISTINCT salary FROM employees
ORDER BY salary DESC LIMIT 1 OFFSET 1;

-- Method 3: Dense Rank (best — handles ties)
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr
    FROM employees
) t WHERE dr = 2;
```

→ See [[23 - Window Functions]], [[20 - Subqueries]]

---

### Q12. Find duplicate records

```sql
-- Find emails that appear more than once
SELECT email, COUNT(*) AS cnt
FROM users
GROUP BY email
HAVING cnt > 1;

-- Show the full duplicate rows
SELECT * FROM users
WHERE email IN (
    SELECT email FROM users
    GROUP BY email HAVING COUNT(*) > 1
)
ORDER BY email;

-- Delete duplicates, keep the one with lowest id
DELETE FROM users
WHERE user_id NOT IN (
    SELECT MIN(user_id) FROM users GROUP BY email
);
```

→ See [[12 - Aggregation Functions]], [[20 - Subqueries]]

---

### Q13. What is a self-join? Write an example.

```sql
-- Find all employees and their manager (both in employees table)
SELECT
    e.name     AS employee,
    m.name     AS manager,
    e.salary   AS emp_salary
FROM employees e
LEFT JOIN employees m ON e.manager_id = m.emp_id;
```

→ See [[22 - Joins]]

---

### Q14. What is a subquery? What is a correlated subquery?

> **Subquery:** A nested query that runs independently first.
> **Correlated subquery:** References the outer query — runs once per outer row.

```sql
-- Regular subquery (runs once)
SELECT * FROM employees
WHERE dept_id IN (SELECT dept_id FROM departments WHERE location = 'Mumbai');

-- Correlated subquery (runs per row — slower)
SELECT * FROM employees e
WHERE salary > (SELECT AVG(salary) FROM employees WHERE dept = e.dept);
```

→ See [[20 - Subqueries]]

---

### Q15. What is the difference between CHAR and VARCHAR?

| | CHAR(n) | VARCHAR(n) |
|-|---------|-----------|
| Storage | Fixed n bytes always | Variable (actual length + 1-2 bytes) |
| Speed | Slightly faster | Slightly slower |
| Trailing spaces | Padded to n | Preserved as-is |
| Best for | Fixed-length codes (postal codes, CHAR(10) PANs) | Variable text (names, emails) |

→ See [[07 - CREATE TABLE Variants]]

---

### Q16. Explain window functions with an example.

> Window functions perform calculations across related rows **without collapsing them** into one row (unlike GROUP BY).

```sql
-- Running total of orders per customer
SELECT
    user_id,
    order_date,
    amount,
    SUM(amount) OVER (PARTITION BY user_id ORDER BY order_date) AS running_total
FROM orders;
-- Every order row is preserved + running total added!
```

→ See [[23 - Window Functions]]

---

### Q17. What is normalization? Explain 1NF, 2NF, 3NF.

```
1NF → Atomic values, no repeating groups, unique rows
2NF → 1NF + no partial dependencies (all non-keys depend on FULL PK)
3NF → 2NF + no transitive dependencies (non-key can't depend on non-key)
```

→ See [[31 - Normalization]]

---

### Q18. How do you get the top N records per group?

```sql
-- Top 3 earners per department
SELECT * FROM (
    SELECT name, dept, salary,
           ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC) AS rn
    FROM employees
) t WHERE rn <= 3;
```

→ See [[23 - Window Functions]]

---

### Q19. What is a VIEW? Can you UPDATE through a view?

> A view is a stored SELECT query — a virtual table. It stores the query definition, not data.

> Views are updatable only if: single table, no aggregations, no DISTINCT, no GROUP BY/HAVING.

→ See [[21 - Views]]

---

### Q20. Write a query to find customers who never placed an order.

```sql
-- Method 1: LEFT JOIN + IS NULL (fast)
SELECT u.user_id, u.name
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
WHERE o.order_id IS NULL;

-- Method 2: NOT EXISTS (semantically clear)
SELECT user_id, name FROM users u
WHERE NOT EXISTS (SELECT 1 FROM orders WHERE user_id = u.user_id);

-- Method 3: NOT IN (careful with NULLs!)
SELECT user_id, name FROM users
WHERE user_id NOT IN (SELECT DISTINCT user_id FROM orders WHERE user_id IS NOT NULL);
```

→ See [[22 - Joins]], [[13 - Logical Operators]]

---

## 🔴 Tier 3 — Senior / 3+ Years / Data Engineer (Advanced)

### Q21. Explain query execution order in SQL.

```
1. FROM       → identify tables
2. JOIN       → combine tables
3. WHERE      → filter rows
4. GROUP BY   → group rows
5. HAVING     → filter groups
6. SELECT     → choose columns (aliases defined here)
7. DISTINCT   → remove duplicates
8. ORDER BY   → sort (can use aliases from SELECT)
9. LIMIT      → restrict rows
```

> **Why it matters:** You can't use SELECT aliases in WHERE (WHERE runs before SELECT).

---

### Q22. What is the difference between ROW_NUMBER, RANK, DENSE_RANK?

```sql
-- Dataset: salaries 100, 90, 90, 80
-- ROW_NUMBER:  1, 2, 3, 4  (always unique)
-- RANK:        1, 2, 2, 4  (tie → same rank, then gap)
-- DENSE_RANK:  1, 2, 2, 3  (tie → same rank, no gap)
```

→ See [[23 - Window Functions]]

---

### Q23. Explain EXPLAIN output. What does `type: ALL` mean?

> `type: ALL` = full table scan — MySQL reads every row. Fix by adding an index on the WHERE/JOIN column.

```sql
EXPLAIN SELECT * FROM orders WHERE user_id = 5;
-- Check: type (ideally 'ref' or 'const'), key (index name), rows (lower = better)
```

Key `type` values (best → worst): `const` → `eq_ref` → `ref` → `range` → `index` → `ALL`

→ See [[26 - EXPLAIN ANALYZE]]

---

### Q24. What is a covering index?

> An index that contains ALL columns needed by a query — MySQL satisfies the query entirely from the index without touching the main table rows.

```sql
CREATE INDEX idx_covering ON orders (user_id, status, amount);
SELECT user_id, status, amount FROM orders WHERE user_id = 5;
-- EXPLAIN Extra: "Using index" → covering index!
```

→ See [[25 - Indexes]]

---

### Q25. What is SCD Type 2? How do you implement it?

> SCD Type 2 handles slowly changing dimension data by adding a **new row** for each change, with effective/expiry dates and an `is_current` flag.

```sql
-- When attribute changes:
-- 1. Expire old record
UPDATE dim_customer SET expiry_date = CURDATE()-1, is_current = FALSE
WHERE customer_id = 1001 AND is_current = TRUE;
-- 2. Insert new current record
INSERT INTO dim_customer (customer_id, ..., effective_date, is_current)
VALUES (1001, ..., CURDATE(), TRUE);
```

→ See [[32 - Slowly Changing Dimensions]]

---

### Q26. What is partition pruning?

> MySQL automatically **skips partitions** that can't contain matching rows, reducing scan scope.

```sql
EXPLAIN PARTITIONS SELECT * FROM orders
WHERE order_date BETWEEN '2024-01-01' AND '2024-12-31';
-- Only scans p2024 partition, skips p2022, p2023!
```

→ See [[27 - Partitioning]]

---

### Q27. LAG and LEAD — explain with example.

```sql
-- Month-over-month revenue change
SELECT month, revenue,
    LAG(revenue)  OVER (ORDER BY month) AS prev_month,
    LEAD(revenue) OVER (ORDER BY month) AS next_month,
    revenue - LAG(revenue) OVER (ORDER BY month) AS mom_change
FROM monthly_revenue;
```

→ See [[23 - Window Functions]]

---

### Q28. How do you handle NOT IN with NULLs?

> If the subquery in `NOT IN` returns any NULL, the entire result is empty (NULL propagation).

```sql
-- DANGEROUS
WHERE id NOT IN (SELECT id FROM table);  -- if any id is NULL → empty result!

-- SAFE
WHERE id NOT IN (SELECT id FROM table WHERE id IS NOT NULL);
-- OR use NOT EXISTS instead
```

→ See [[13 - Logical Operators]], [[16 - NULL Handling]]

---

### Q29. What is the difference between OLTP and OLAP?

| | OLTP | OLAP |
|-|------|------|
| Purpose | Day-to-day transactions | Analytics/reporting |
| Schema | Normalized (3NF) | Denormalized (star/snowflake) |
| Queries | Simple, fast, many | Complex, slow, few |
| Example | MySQL app database | Snowflake data warehouse |
| Typical tables | Millions of small rows | Billions of rows, wide tables |

→ See [[01 - Introduction]], [[34 - Data Engineering Projects]]

---

### Q30. How do you calculate a running total?

```sql
SELECT
    order_date,
    amount,
    SUM(amount) OVER (ORDER BY order_date
                      ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)
    AS running_total
FROM orders;
```

→ See [[23 - Window Functions]]

---

## 📝 Scenario-Based Questions (Most Important!)

### S1. "Find orders placed in 2023 but NOT in 2024 (returning customers who stopped)"

```sql
SELECT DISTINCT user_id FROM orders WHERE YEAR(order_date) = 2023
AND user_id NOT IN (
    SELECT DISTINCT user_id FROM orders WHERE YEAR(order_date) = 2024
);
```

---

### S2. "Find the department with the highest average salary"

```sql
SELECT dept, ROUND(AVG(salary),2) AS avg_sal
FROM employees
GROUP BY dept
ORDER BY avg_sal DESC
LIMIT 1;
```

---

### S3. "Employees earning more than their manager"

```sql
SELECT e.name AS employee, e.salary,
       m.name AS manager,  m.salary AS manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.emp_id
WHERE e.salary > m.salary;
```

---

### S4. "Nth highest salary (general solution)"

```sql
-- N = 3 (third highest)
SELECT salary FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS dr
    FROM employees
) t WHERE dr = 3;
```

---

### S5. "Monthly active users (MAU) for last 6 months"

```sql
SELECT
    DATE_FORMAT(login_date, '%Y-%m') AS month,
    COUNT(DISTINCT user_id)           AS mau
FROM user_logins
WHERE login_date >= DATE_SUB(CURDATE(), INTERVAL 6 MONTH)
GROUP BY DATE_FORMAT(login_date, '%Y-%m')
ORDER BY month;
```

---

### S6. "Find products never ordered"

```sql
SELECT p.product_id, p.name
FROM products p
LEFT JOIN order_items oi ON p.product_id = oi.product_id
WHERE oi.product_id IS NULL;
```

---

### S7. "Cumulative revenue crossed 1 million — on which date?"

```sql
SELECT order_date, running_total
FROM (
    SELECT order_date,
           SUM(amount) OVER (ORDER BY order_date) AS running_total
    FROM orders WHERE status = 'delivered'
) t
WHERE running_total >= 1000000
ORDER BY order_date
LIMIT 1;
```

---

### S8. "Users who ordered every day for 7 consecutive days"

```sql
SELECT user_id
FROM (
    SELECT user_id,
           order_date,
           DATE_SUB(order_date, INTERVAL ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY order_date) DAY) AS grp
    FROM (SELECT DISTINCT user_id, DATE(order_date) AS order_date FROM orders) d
) t
GROUP BY user_id, grp
HAVING COUNT(*) >= 7;
```

---

## 💡 Quick Tips for Interviews

1. **Always think aloud** — explain your approach before writing SQL
2. **Mention trade-offs** — "I'd use NOT EXISTS instead of NOT IN because of NULL safety"
3. **Talk about performance** — "I'd add an index on user_id for this query"
4. **Ask clarifying questions** — "Should this include cancelled orders?"
5. **Multiple approaches** — show you know more than one way
6. **NULL awareness** — mention NULL handling for every filter/comparison

---

## 🔗 Related Topics

- [[23 - Window Functions]] — Most tested advanced topic
- [[22 - Joins]] — Core topic, always tested
- [[20 - Subqueries]] — Scenario questions need these
- [[25 - Indexes]] — Performance questions
- [[30 - ACID Properties]] — Theory questions
- [[31 - Normalization]] — Design questions
- [[32 - Slowly Changing Dimensions]] — Data Engineer specific
