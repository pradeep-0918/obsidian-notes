# ⚡ SQL Cheat Sheet — Quick Reference

#revision #cheatsheet #syntax

> **Print-ready quick reference. Use before interviews.**

---

## 📋 DDL Commands

```sql
-- Create
CREATE DATABASE mydb;
CREATE TABLE t (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(100) NOT NULL);
CREATE TABLE t2 AS SELECT * FROM t WHERE 1=0;  -- copy structure only
CREATE TABLE t3 LIKE t;                          -- copy structure + constraints

-- Modify
ALTER TABLE t ADD COLUMN email VARCHAR(255);
ALTER TABLE t DROP COLUMN email;
ALTER TABLE t MODIFY COLUMN name VARCHAR(200) NOT NULL;
ALTER TABLE t RENAME COLUMN old_name TO new_name;
ALTER TABLE t ADD INDEX idx_name (name);
ALTER TABLE t DROP INDEX idx_name;

-- Remove
DROP TABLE IF EXISTS t;
TRUNCATE TABLE t;                -- remove all data, reset AUTO_INCREMENT
DROP DATABASE mydb;
```

---

## 📋 DML Commands

```sql
-- Insert
INSERT INTO t (col1, col2) VALUES (v1, v2);
INSERT INTO t (col1, col2) VALUES (v1,v2), (v3,v4), (v5,v6);  -- bulk
INSERT INTO t SELECT col1, col2 FROM other_t WHERE condition;

-- Select
SELECT col1, col2 FROM t WHERE cond ORDER BY col1 DESC LIMIT 10 OFFSET 20;
SELECT DISTINCT col1 FROM t;
SELECT COUNT(*), SUM(col), AVG(col), MAX(col), MIN(col) FROM t;

-- Update
UPDATE t SET col1 = val1, col2 = val2 WHERE condition;

-- Delete
DELETE FROM t WHERE condition;
```

---

## 📋 Filtering & Operators

```sql
WHERE col = 'value'
WHERE col != 'value'          -- or <>
WHERE col > 100
WHERE col BETWEEN 10 AND 100  -- inclusive
WHERE col IN ('a', 'b', 'c')
WHERE col NOT IN ('x', 'y')
WHERE col IS NULL
WHERE col IS NOT NULL
WHERE col LIKE 'A%'           -- starts with A
WHERE col LIKE '%kumar'       -- ends with kumar
WHERE col LIKE '_avi%'        -- second char is a
WHERE col REGEXP '^[0-9]{10}$'
WHERE (cond1 OR cond2) AND cond3   -- always parenthesise OR with AND
```

---

## 📋 Aggregation + GROUP BY + HAVING

```sql
SELECT dept,
    COUNT(*)            AS count,
    COUNT(DISTINCT id)  AS unique_count,
    SUM(salary)         AS total,
    AVG(salary)         AS avg,
    MAX(salary)         AS max,
    MIN(salary)         AS min,
    GROUP_CONCAT(name ORDER BY name SEPARATOR ', ') AS names
FROM employees
WHERE hire_date > '2020-01-01'  -- filter rows FIRST
GROUP BY dept
HAVING AVG(salary) > 50000      -- filter groups AFTER
ORDER BY total DESC
LIMIT 5;
```

---

## 📋 JOIN Syntax

```sql
-- INNER JOIN (only matches)
FROM a INNER JOIN b ON a.id = b.a_id

-- LEFT JOIN (all from a)
FROM a LEFT JOIN b ON a.id = b.a_id

-- RIGHT JOIN (all from b)
FROM a RIGHT JOIN b ON a.id = b.a_id

-- SELF JOIN
FROM employees e JOIN employees m ON e.manager_id = m.emp_id

-- CROSS JOIN
FROM sizes CROSS JOIN colors

-- Find rows in A with no match in B
FROM a LEFT JOIN b ON a.id = b.a_id WHERE b.a_id IS NULL
```

---

## 📋 Window Functions

```sql
-- Ranking
ROW_NUMBER() OVER (PARTITION BY dept ORDER BY salary DESC)
RANK()        OVER (PARTITION BY dept ORDER BY salary DESC)
DENSE_RANK()  OVER (PARTITION BY dept ORDER BY salary DESC)
NTILE(4)      OVER (ORDER BY salary)
PERCENT_RANK() OVER (ORDER BY salary)

-- Value
LAG(salary, 1, 0) OVER (PARTITION BY dept ORDER BY hire_date)
LEAD(salary)       OVER (ORDER BY hire_date)
FIRST_VALUE(salary) OVER (PARTITION BY dept ORDER BY salary DESC)

-- Aggregate
SUM(amount)  OVER (PARTITION BY user_id ORDER BY date)
AVG(amount)  OVER (ORDER BY date ROWS BETWEEN 6 PRECEDING AND CURRENT ROW)
COUNT(*)     OVER (PARTITION BY dept)
```

---

## 📋 Subqueries

```sql
-- In WHERE
WHERE salary > (SELECT AVG(salary) FROM employees)
WHERE dept_id IN (SELECT dept_id FROM depts WHERE city = 'Mumbai')
WHERE EXISTS (SELECT 1 FROM orders WHERE user_id = u.user_id)

-- In FROM (derived table)
FROM (SELECT dept, AVG(salary) avg_sal FROM employees GROUP BY dept) t

-- CTE (WITH clause)
WITH dept_avg AS (SELECT dept, AVG(salary) avg FROM employees GROUP BY dept)
SELECT e.*, d.avg FROM employees e JOIN dept_avg d ON e.dept = d.dept
```

---

## 📋 String Functions

```sql
UPPER(str) / LOWER(str)
TRIM(str) / LTRIM(str) / RTRIM(str)
LENGTH(str) / CHAR_LENGTH(str)
CONCAT(s1, s2, s3)
CONCAT_WS(sep, s1, s2, s3)         -- ignores NULLs
SUBSTRING(str, start, length)
LEFT(str, n) / RIGHT(str, n)
REPLACE(str, find, replace)
LOCATE(find, str)                   -- returns position
LPAD(str, len, pad) / RPAD(...)
FORMAT(number, decimals)
REGEXP_REPLACE(str, pattern, replacement)
```

---

## 📋 Date Functions

```sql
NOW() / CURDATE() / CURTIME()
YEAR(d) / MONTH(d) / DAY(d) / HOUR(t) / MINUTE(t)
EXTRACT(YEAR FROM date_col)
DATE_ADD(date, INTERVAL n DAY|MONTH|YEAR|HOUR)
DATE_SUB(date, INTERVAL n DAY|MONTH|YEAR)
DATEDIFF(d1, d2)                    -- days between
TIMESTAMPDIFF(UNIT, d1, d2)        -- YEAR/MONTH/DAY/HOUR
DATE_FORMAT(date, '%Y-%m-%d')
STR_TO_DATE('15/07/2024', '%d/%m/%Y')
LAST_DAY(date)
DAYNAME(date) / MONTHNAME(date)
WEEKDAY(date)                       -- 0=Monday, 6=Sunday
```

---

## 📋 NULL Functions

```sql
IFNULL(col, default)                 -- if NULL, use default
COALESCE(c1, c2, c3, default)       -- first non-NULL value
NULLIF(a, b)                         -- returns NULL if a = b (div by zero!)
ISNULL(col)                          -- returns 1 if NULL
```

---

## 📋 CASE WHEN

```sql
-- Searched CASE
CASE
    WHEN salary >= 100000 THEN 'Senior'
    WHEN salary >= 50000  THEN 'Mid'
    ELSE 'Junior'
END AS level

-- Simple CASE
CASE status
    WHEN 'P' THEN 'Pending'
    WHEN 'D' THEN 'Delivered'
    ELSE 'Other'
END

-- Conditional aggregation
SUM(CASE WHEN status='delivered' THEN amount ELSE 0 END) AS delivered_revenue
COUNT(CASE WHEN gender='F' THEN 1 END) AS female_count
```

---

## 📋 Indexes

```sql
CREATE INDEX idx_name ON table (column);
CREATE UNIQUE INDEX idx_email ON users (email);
CREATE INDEX idx_composite ON orders (user_id, status, order_date);
ALTER TABLE t ADD INDEX idx_col (col);
DROP INDEX idx_name ON table;
SHOW INDEX FROM table;

-- Check if index used
EXPLAIN SELECT * FROM orders WHERE user_id = 5;
```

---

## 📋 Transactions

```sql
START TRANSACTION;
    DML statements...
    SAVEPOINT sp1;
    DML statements...
    ROLLBACK TO SAVEPOINT sp1;
COMMIT;     -- save
ROLLBACK;   -- undo all

SET autocommit = 0;  -- disable auto-commit
SET autocommit = 1;  -- re-enable
```

---

## 📋 User Management

```sql
CREATE USER 'user'@'host' IDENTIFIED BY 'password';
GRANT SELECT, INSERT ON db.table TO 'user'@'host';
GRANT ALL PRIVILEGES ON db.* TO 'user'@'host';
REVOKE INSERT ON db.table FROM 'user'@'host';
SHOW GRANTS FOR 'user'@'host';
DROP USER 'user'@'host';
FLUSH PRIVILEGES;
```

---

## 📋 EXPLAIN Quick Reading

```
type column (best → worst):
  const → eq_ref → ref → range → index → ALL

key column:
  NULL = no index used (investigate!)
  index_name = good

Extra column:
  "Using index"     → covering index (fast!)
  "Using filesort"  → add index on ORDER BY column
  "Using temporary" → add index on GROUP BY column
  "Using where"     → post-filter (normal)
```

---

## 📋 Partitioning Quick Reference

```sql
-- RANGE partition
PARTITION BY RANGE (YEAR(order_date)) (
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);

-- Manage
ALTER TABLE t ADD PARTITION (PARTITION p2025 VALUES LESS THAN (2026));
ALTER TABLE t DROP PARTITION p2021;      -- instant delete!
ALTER TABLE t TRUNCATE PARTITION p2021;

-- Verify pruning
EXPLAIN PARTITIONS SELECT * FROM t WHERE order_date >= '2024-01-01';
```

---

## 🔢 Key Numbers to Remember

| Item | Value |
|------|-------|
| MySQL default port | 3306 |
| INT range | -2.1B to 2.1B |
| BIGINT range | -9.2 × 10¹⁸ to 9.2 × 10¹⁸ |
| VARCHAR max | 65,535 bytes |
| TIMESTAMP max year | 2038 |
| DATETIME max year | 9999 |
| Default isolation level | REPEATABLE READ |
| Autocommit default | ON |

---

## 🔗 Related Topics

- [[35 - Interview Questions Master]] — Full question bank
- [[22 - Joins]] — JOIN deep dive
- [[23 - Window Functions]] — Window function deep dive
- [[25 - Indexes]] — Index deep dive
- [[34 - Data Engineering Projects]] — Apply everything
