# 18 — Date & Time Functions

#intermediate #interview #realworld

---

## 🧠 Introduction

Date and time manipulation is **critical in Data Engineering**. Almost every real-world dataset has timestamps — order dates, login times, event logs. MySQL has powerful built-in functions to work with them.

---

## 📅 Date/Time Data Types

| Type | Format | Range | Use For |
|------|--------|-------|---------|
| `DATE` | `YYYY-MM-DD` | 1000-01-01 to 9999-12-31 | Dates only |
| `TIME` | `HH:MM:SS` | -838:59:59 to 838:59:59 | Time only |
| `DATETIME` | `YYYY-MM-DD HH:MM:SS` | 1000 to 9999 | Combined, no timezone |
| `TIMESTAMP` | `YYYY-MM-DD HH:MM:SS` | 1970 to 2038 | Auto-updates, timezone-aware |
| `YEAR` | `YYYY` | 1901 to 2155 | Year only |

---

## ⏰ Current Date/Time Functions

```sql
-- Get current values
SELECT NOW();           -- 2024-07-15 14:30:00  (datetime)
SELECT CURDATE();       -- 2024-07-15            (date only)
SELECT CURTIME();       -- 14:30:00              (time only)
SELECT SYSDATE();       -- same as NOW() but evaluated per row
SELECT UTC_TIMESTAMP(); -- current UTC datetime

-- In INSERT statements
INSERT INTO events (event_name, created_at)
VALUES ('user_signup', NOW());
```

---

## 🔢 Extracting Date Parts

```sql
-- Individual parts
SELECT YEAR('2024-07-15');    -- 2024
SELECT MONTH('2024-07-15');   -- 7
SELECT DAY('2024-07-15');     -- 15
SELECT HOUR('14:30:45');      -- 14
SELECT MINUTE('14:30:45');    -- 30
SELECT SECOND('14:30:45');    -- 45

-- EXTRACT (ANSI standard)
SELECT EXTRACT(YEAR FROM order_date) FROM orders;
SELECT EXTRACT(MONTH FROM order_date) FROM orders;
SELECT EXTRACT(QUARTER FROM order_date) FROM orders;  -- Q1=1, Q4=4

-- Day of week/year
SELECT DAYOFWEEK('2024-07-15');   -- 2 (1=Sunday, 2=Monday)
SELECT WEEKDAY('2024-07-15');     -- 0 (0=Monday, 6=Sunday)
SELECT DAYOFYEAR('2024-07-15');   -- 197
SELECT WEEK('2024-07-15');        -- 29 (week number)

-- DAYNAME, MONTHNAME
SELECT DAYNAME('2024-07-15');     -- Monday
SELECT MONTHNAME('2024-07-15');   -- July
```

---

## ➕ Date Arithmetic

```sql
-- DATE_ADD / DATE_SUB
SELECT DATE_ADD('2024-07-15', INTERVAL 30 DAY);   -- 2024-08-14
SELECT DATE_SUB('2024-07-15', INTERVAL 1 MONTH);  -- 2024-06-15
SELECT DATE_ADD(NOW(), INTERVAL 2 HOUR);

-- Interval units: SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, YEAR
SELECT DATE_ADD('2024-01-31', INTERVAL 1 MONTH);  -- 2024-02-29 (leap year aware)

-- DATEDIFF: days between two dates
SELECT DATEDIFF('2024-12-31', '2024-01-01');   -- 365
SELECT DATEDIFF(NOW(), hire_date) AS days_employed FROM employees;

-- TIMEDIFF: time difference
SELECT TIMEDIFF('18:00:00', '09:30:00');  -- 08:30:00

-- TIMESTAMPDIFF: flexible unit
SELECT TIMESTAMPDIFF(YEAR,  birth_date, CURDATE()) AS age FROM employees;
SELECT TIMESTAMPDIFF(MONTH, hire_date,  CURDATE()) AS months_employed FROM employees;
SELECT TIMESTAMPDIFF(DAY,   order_date, delivered_at) AS delivery_days FROM orders;
```

---

## 🎨 Formatting Dates

```sql
-- DATE_FORMAT: custom format strings
SELECT DATE_FORMAT(NOW(), '%d-%m-%Y');          -- 15-07-2024
SELECT DATE_FORMAT(NOW(), '%D %M %Y');          -- 15th July 2024
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s'); -- 2024-07-15 14:30:00
SELECT DATE_FORMAT(NOW(), '%W, %d %M %Y');      -- Monday, 15 July 2024

-- Common format codes
-- %Y=4-digit year, %y=2-digit year
-- %m=month 01-12, %M=month name, %b=month abbrev
-- %d=day 01-31, %D=1st/2nd/3rd
-- %H=hour 00-23, %h=hour 01-12
-- %i=minutes, %s=seconds
-- %W=weekday name, %w=weekday number (0=Sunday)

-- STR_TO_DATE: parse string to date
SELECT STR_TO_DATE('15/07/2024', '%d/%m/%Y');  -- 2024-07-15
-- Critical for importing CSV data with non-standard date formats!
```

---

## 📊 Truncating to Period

```sql
-- Start of day/month/year
SELECT DATE(NOW());                              -- 2024-07-15 (strips time)
SELECT DATE_FORMAT(NOW(), '%Y-%m-01');           -- 2024-07-01 (start of month)
SELECT DATE_FORMAT(NOW(), '%Y-01-01');           -- 2024-01-01 (start of year)

-- LAST_DAY: last day of month
SELECT LAST_DAY('2024-02-15');   -- 2024-02-29 (leap year!)
SELECT LAST_DAY('2024-07-15');   -- 2024-07-31
```

---

## 🌍 Real-World Use Case — E-Commerce Analytics

```sql
-- Monthly revenue trend (last 12 months)
SELECT
    DATE_FORMAT(order_date, '%Y-%m')     AS month,
    COUNT(*)                              AS total_orders,
    SUM(amount)                           AS revenue,
    COUNT(DISTINCT user_id)               AS unique_customers
FROM orders
WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 12 MONTH)
  AND status = 'delivered'
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
ORDER BY month;

-- Delivery time analysis
SELECT
    order_id,
    order_date,
    delivered_at,
    TIMESTAMPDIFF(HOUR, order_date, delivered_at)  AS delivery_hours,
    CASE
        WHEN TIMESTAMPDIFF(HOUR, order_date, delivered_at) <= 24 THEN 'Same Day'
        WHEN TIMESTAMPDIFF(HOUR, order_date, delivered_at) <= 72 THEN 'Express'
        ELSE 'Standard'
    END AS delivery_tier
FROM orders
WHERE delivered_at IS NOT NULL;

-- Daily active users (DAU) for last 30 days
SELECT
    DATE(login_at)  AS login_date,
    COUNT(DISTINCT user_id) AS dau
FROM user_sessions
WHERE login_at >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
GROUP BY DATE(login_at)
ORDER BY login_date DESC;

-- Weekend vs weekday order comparison
SELECT
    CASE WHEN WEEKDAY(order_date) IN (5, 6) THEN 'Weekend' ELSE 'Weekday' END AS day_type,
    COUNT(*)    AS orders,
    AVG(amount) AS avg_amount
FROM orders
GROUP BY day_type;
```

---

## 💼 Interview Questions

1. **What is the difference between NOW() and CURDATE()?**
   > NOW() returns date + time (DATETIME). CURDATE() returns date only (DATE).

2. **What is the difference between DATETIME and TIMESTAMP?**
   > DATETIME stores literal value regardless of timezone, range up to 9999. TIMESTAMP is timezone-aware, auto-updates on row change, range up to 2038.

3. **How do you find all orders placed in the last 7 days?**
   ```sql
   SELECT * FROM orders WHERE order_date >= DATE_SUB(CURDATE(), INTERVAL 7 DAY);
   ```

4. **How do you calculate age from a birth date?**
   ```sql
   SELECT TIMESTAMPDIFF(YEAR, birth_date, CURDATE()) AS age FROM employees;
   ```

5. **Scenario:** *"Your CSV file has dates in format 'DD/MM/YYYY'. How do you load them into MySQL?"*
   > Use `STR_TO_DATE(date_column, '%d/%m/%Y')` during INSERT or UPDATE.

---

## ⚠️ Common Mistakes

- Using `YEAR(order_date) = 2024` in WHERE — prevents index usage! Use range: `order_date >= '2024-01-01' AND order_date < '2025-01-01'`
- Confusing DATEDIFF (days only) with TIMESTAMPDIFF (flexible units)
- Not handling the 2038 TIMESTAMP problem for systems expected to run long-term

---

## 🔗 Related Topics

- [[09 - WHERE Clause]] — Date filtering best practices
- [[12 - Aggregation Functions]] — GROUP BY date periods
- [[15 - CASE WHEN]] — Date-based categorization
- [[25 - Indexes]] — Avoid functions on indexed date columns
- [[19 - Regex]] — Pattern matching on date strings
