# 15 — CASE WHEN

#intermediate #interview

---

## 🧠 Introduction

`CASE WHEN` is SQL's **if-else statement**. It's used to create conditional logic inside queries — categorizing data, transforming values, and building conditional aggregations.

---

## 📐 Syntax

```sql
-- Simple CASE (equality check)
CASE column
    WHEN value1 THEN result1
    WHEN value2 THEN result2
    ELSE default_result
END

-- Searched CASE (condition-based — more flexible)
CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ELSE default_result
END
```

---

## 🔷 Basic Examples

```sql
-- Categorize salary bands
SELECT 
    name,
    salary,
    CASE
        WHEN salary >= 100000 THEN 'Senior'
        WHEN salary >= 60000  THEN 'Mid-Level'
        WHEN salary >= 30000  THEN 'Junior'
        ELSE 'Intern'
    END AS level
FROM employees;

-- Map status codes to labels
SELECT 
    order_id,
    amount,
    CASE status
        WHEN 'P' THEN 'Pending'
        WHEN 'S' THEN 'Shipped'
        WHEN 'D' THEN 'Delivered'
        WHEN 'C' THEN 'Cancelled'
        ELSE 'Unknown'
    END AS order_status
FROM orders;
```

---

## 🔶 CASE in ORDER BY

```sql
-- Custom sort order: Delivered first, then Pending, then others
SELECT order_id, status, amount
FROM orders
ORDER BY 
    CASE status
        WHEN 'delivered' THEN 1
        WHEN 'pending'   THEN 2
        WHEN 'cancelled' THEN 3
        ELSE 4
    END;
```

---

## 🔵 CASE in Conditional Aggregation (Powerful!)

```sql
-- Count by category in one query (pivot table)
SELECT
    YEAR(order_date) AS year,
    COUNT(CASE WHEN status = 'delivered'  THEN 1 END) AS delivered,
    COUNT(CASE WHEN status = 'cancelled'  THEN 1 END) AS cancelled,
    COUNT(CASE WHEN status = 'pending'    THEN 1 END) AS pending,
    SUM(CASE WHEN status = 'delivered' THEN amount ELSE 0 END) AS delivered_revenue
FROM orders
GROUP BY YEAR(order_date);

-- Gender-based salary analysis
SELECT
    dept,
    ROUND(AVG(CASE WHEN gender = 'M' THEN salary END), 2) AS avg_male_salary,
    ROUND(AVG(CASE WHEN gender = 'F' THEN salary END), 2) AS avg_female_salary,
    COUNT(CASE WHEN gender = 'M' THEN 1 END)              AS male_count,
    COUNT(CASE WHEN gender = 'F' THEN 1 END)              AS female_count
FROM employees
GROUP BY dept;
```

---

## 🔧 CASE in UPDATE

```sql
-- Bulk update based on condition
UPDATE employees
SET salary = salary * 
    CASE
        WHEN dept = 'Engineering' THEN 1.15   -- 15% raise
        WHEN dept = 'Sales'       THEN 1.10   -- 10% raise
        WHEN dept = 'HR'          THEN 1.05   -- 5% raise
        ELSE 1.03                              -- 3% for others
    END;
```

---

## 🌍 Real-World Use Case — Ride-Sharing Analytics

```sql
-- Driver tier classification + surge eligibility
SELECT
    driver_id,
    completed_trips,
    avg_rating,
    CASE
        WHEN completed_trips >= 1000 AND avg_rating >= 4.8 THEN 'Diamond'
        WHEN completed_trips >= 500  AND avg_rating >= 4.5 THEN 'Gold'
        WHEN completed_trips >= 100  AND avg_rating >= 4.0 THEN 'Silver'
        ELSE 'Bronze'
    END AS tier,
    CASE
        WHEN avg_rating >= 4.5 THEN 'Yes'
        ELSE 'No'
    END AS surge_eligible
FROM driver_stats;
```

---

## 💼 Interview Questions

1. **What is the difference between Simple CASE and Searched CASE?**
   > Simple CASE checks equality against one column. Searched CASE evaluates independent conditions per WHEN — more flexible.

2. **Can CASE WHEN return NULL?**
   > Yes — if no condition matches and there's no ELSE, CASE returns NULL.

3. **How do you create a pivot table in MySQL using CASE?**
   > Use conditional aggregation: `SUM(CASE WHEN category = 'A' THEN amount ELSE 0 END)` for each category column.

4. **Scenario:** *"Classify customers as 'High Value', 'Medium', or 'Low' based on their total spend."*
   ```sql
   SELECT user_id, SUM(amount) AS total,
     CASE WHEN SUM(amount) > 50000 THEN 'High Value'
          WHEN SUM(amount) > 10000 THEN 'Medium'
          ELSE 'Low' END AS segment
   FROM orders GROUP BY user_id;
   ```

---

## 🔗 Related Topics

- [[12 - Aggregation Functions]] — Conditional aggregation with CASE
- [[16 - NULL Handling]] — CASE returns NULL without ELSE
- [[23 - Window Functions]] — CASE inside window functions
- [[09 - WHERE Clause]] — CASE in WHERE conditions
