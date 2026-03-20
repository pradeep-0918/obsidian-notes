# 24 — UNION / UNION ALL

#intermediate #interview

---

## 🧠 Introduction

`UNION` combines the results of **two or more SELECT statements** into a single result set. It's used to merge data from different tables or queries that have the same column structure.

---

## 📐 Syntax

```sql
SELECT col1, col2 FROM table1
UNION [ALL]
SELECT col1, col2 FROM table2;
```

**Rules:**
- Both SELECT statements must have the **same number of columns**
- Corresponding columns must have **compatible data types**
- Column names from the **first SELECT** are used as headers

---

## 🔷 UNION vs UNION ALL

| Feature | UNION | UNION ALL |
|---------|-------|-----------|
| Duplicates | Removed | Kept |
| Performance | Slower (deduplication) | Faster |
| Use when | You need distinct rows | You know rows are unique OR want all |

```sql
-- UNION: removes duplicates
SELECT user_id FROM buyers_2023
UNION
SELECT user_id FROM buyers_2024;
-- If someone bought in both years, they appear ONCE

-- UNION ALL: keeps duplicates
SELECT user_id FROM buyers_2023
UNION ALL
SELECT user_id FROM buyers_2024;
-- Same user appears TWICE if they bought in both years
-- Much faster — no dedup needed
```

---

## 🔶 Practical Examples

```sql
-- Combine active and archived orders into one report
SELECT order_id, user_id, amount, order_date, 'active' AS source
FROM orders
UNION ALL
SELECT order_id, user_id, amount, order_date, 'archived' AS source
FROM orders_archive;

-- Top 5 spenders from each city combined
SELECT * FROM (
    SELECT name, city, total_spend FROM customers WHERE city = 'Chennai'
    ORDER BY total_spend DESC LIMIT 5
) t1
UNION ALL
SELECT * FROM (
    SELECT name, city, total_spend FROM customers WHERE city = 'Mumbai'
    ORDER BY total_spend DESC LIMIT 5
) t2
ORDER BY city, total_spend DESC;

-- Merge logs from multiple servers
SELECT server_id, log_time, message, 'server_1' AS source FROM logs_server1
UNION ALL
SELECT server_id, log_time, message, 'server_2' AS source FROM logs_server2
UNION ALL
SELECT server_id, log_time, message, 'server_3' AS source FROM logs_server3
ORDER BY log_time DESC;
```

---

## 🟡 ORDER BY and LIMIT with UNION

```sql
-- ORDER BY applies to the ENTIRE union result (put at the end)
SELECT name, salary FROM employees WHERE dept = 'Engineering'
UNION ALL
SELECT name, salary FROM employees WHERE dept = 'Data'
ORDER BY salary DESC;  -- sorts entire result

-- LIMIT on entire result
SELECT name, city FROM users WHERE city = 'Chennai'
UNION ALL
SELECT name, city FROM users WHERE city = 'Bangalore'
LIMIT 20;

-- LIMIT per subquery (wrap in parentheses)
(SELECT name, city FROM users WHERE city = 'Chennai' ORDER BY name LIMIT 10)
UNION ALL
(SELECT name, city FROM users WHERE city = 'Bangalore' ORDER BY name LIMIT 10);
```

---

## 🔧 UNION for FULL OUTER JOIN Emulation

```sql
-- Full outer join (MySQL doesn't have FULL OUTER JOIN)
SELECT a.id, a.name, b.order_id
FROM users a LEFT JOIN orders b ON a.user_id = b.user_id
UNION
SELECT a.id, a.name, b.order_id
FROM users a RIGHT JOIN orders b ON a.user_id = b.user_id
WHERE a.user_id IS NULL;
```

---

## 🌍 Real-World Use Case — Data Pipeline / ETL

```sql
-- Unified transaction view across multiple payment tables
CREATE VIEW vw_all_transactions AS
SELECT
    txn_id, user_id, amount, txn_date, 'credit_card' AS payment_method
FROM cc_transactions
UNION ALL
SELECT
    txn_id, user_id, amount, txn_date, 'upi' AS payment_method
FROM upi_transactions
UNION ALL
SELECT
    txn_id, user_id, amount, txn_date, 'netbanking' AS payment_method
FROM nb_transactions;

-- Usage
SELECT payment_method, COUNT(*), SUM(amount)
FROM vw_all_transactions
WHERE txn_date >= '2024-01-01'
GROUP BY payment_method;
```

---

## 💼 Interview Questions

1. **What is the difference between UNION and UNION ALL?**
   > UNION removes duplicate rows (requires extra sort/dedup operation). UNION ALL keeps all rows including duplicates — significantly faster.

2. **When would you use UNION instead of JOIN?**
   > JOIN combines columns from different tables (horizontal). UNION combines rows from multiple queries (vertical/stacking). Use UNION when merging same-structure data from multiple sources.

3. **Can UNION combine queries with different numbers of columns?**
   > No — both SELECT statements must return the same number of columns with compatible data types.

4. **How do you apply ORDER BY to a UNION query?**
   > Put ORDER BY at the very end, after the last SELECT. To sort each subquery separately, wrap each in parentheses and apply ORDER BY+LIMIT inside.

5. **Scenario:** *"You have separate tables for desktop and mobile users. How do you get a single list?"*
   ```sql
   SELECT user_id, name, 'desktop' AS platform FROM desktop_users
   UNION ALL
   SELECT user_id, name, 'mobile' AS platform FROM mobile_users;
   ```

---

## ⚠️ Common Mistakes

- Using UNION when UNION ALL is sufficient — unnecessary dedup overhead
- Different column counts in the two SELECTs — runtime error
- Putting ORDER BY inside UNION subqueries without parentheses — ignored or error
- UNION with NULL columns — pad missing columns with `NULL AS col_name`

---

## 🔗 Related Topics

- [[22 - Joins]] — JOINs (horizontal) vs UNION (vertical)
- [[20 - Subqueries]] — Subqueries with UNION
- [[21 - Views]] — UNION inside views
- [[34 - Data Engineering Projects]] — Merging data from multiple sources
