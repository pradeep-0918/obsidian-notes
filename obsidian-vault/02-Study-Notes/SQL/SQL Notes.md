# 📚 SQL Notes — Finance & Data Engineering Focus

> SQL is asked in EVERY data interview. Master these patterns.

---

## 🗄️ SQL Fundamentals

### Basic SELECT
```sql
SELECT column1, column2
FROM table_name
WHERE condition
ORDER BY column1 DESC
LIMIT 100;
```

### Filtering
```sql
-- Multiple conditions
SELECT * FROM loans
WHERE credit_score >= 680 AND loan_amount > 200000;

-- OR condition
SELECT * FROM customers
WHERE state = 'TX' OR state = 'CA' OR state = 'FL';

-- IN operator (cleaner than multiple ORs)
SELECT * FROM customers
WHERE state IN ('TX', 'CA', 'FL');

-- BETWEEN
SELECT * FROM loans
WHERE origination_date BETWEEN '2022-01-01' AND '2023-12-31';

-- LIKE (pattern matching)
SELECT * FROM customers
WHERE email LIKE '%@gmail.com';

-- NULL handling
SELECT * FROM loans WHERE credit_score IS NOT NULL;
SELECT * FROM loans WHERE co_borrower IS NULL;
```

---

## 📊 Aggregation & GROUP BY

```sql
-- Count loans per state
SELECT state, COUNT(*) AS total_loans
FROM loans
GROUP BY state
ORDER BY total_loans DESC;

-- Average loan amount by loan type
SELECT loan_type,
       COUNT(*) AS num_loans,
       AVG(loan_amount) AS avg_amount,
       MIN(loan_amount) AS min_amount,
       MAX(loan_amount) AS max_amount,
       SUM(loan_amount) AS total_volume
FROM loans
GROUP BY loan_type;

-- HAVING (filter AFTER grouping — like WHERE but for aggregates)
SELECT state, AVG(credit_score) AS avg_score
FROM loans
GROUP BY state
HAVING AVG(credit_score) < 700  -- only states with low avg credit
ORDER BY avg_score ASC;
```

---

## 🔗 JOINs — Combining Tables

```sql
-- INNER JOIN: only matching rows from both tables
SELECT l.loan_id, l.loan_amount, c.name, c.state
FROM loans l
INNER JOIN customers c ON l.customer_id = c.customer_id;

-- LEFT JOIN: all rows from left table, matching from right
SELECT c.name, l.loan_amount
FROM customers c
LEFT JOIN loans l ON c.customer_id = l.customer_id;
-- customers with no loans will have NULL loan_amount

-- Multiple joins
SELECT
    l.loan_id,
    c.name AS customer_name,
    p.property_address,
    s.servicer_name
FROM loans l
JOIN customers c ON l.customer_id = c.customer_id
JOIN properties p ON l.property_id = p.property_id
LEFT JOIN servicers s ON l.servicer_id = s.servicer_id;
```

---

## 🪟 Window Functions — Advanced (Very Commonly Asked)

```sql
-- ROW_NUMBER: rank rows within a group
SELECT
    loan_id,
    customer_id,
    loan_amount,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY loan_amount DESC) AS loan_rank
FROM loans;
-- Gives each customer's loans a rank by amount (1 = largest)

-- RANK vs DENSE_RANK
-- RANK: 1, 2, 2, 4 (skips 3 when there's a tie)
-- DENSE_RANK: 1, 2, 2, 3 (no skipping)

-- Running total (cumulative sum)
SELECT
    origination_date,
    loan_amount,
    SUM(loan_amount) OVER (ORDER BY origination_date) AS cumulative_volume
FROM loans;

-- Moving average (3-month rolling)
SELECT
    month,
    avg_rate,
    AVG(avg_rate) OVER (ORDER BY month ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS rolling_3m_avg
FROM monthly_rates;

-- LAG / LEAD (compare to previous/next row)
SELECT
    month,
    originations,
    LAG(originations, 1) OVER (ORDER BY month) AS prev_month,
    originations - LAG(originations, 1) OVER (ORDER BY month) AS mom_change
FROM monthly_originations;
```

---

## 🔁 Subqueries & CTEs

```sql
-- Subquery in WHERE
SELECT * FROM loans
WHERE customer_id IN (
    SELECT customer_id FROM customers WHERE state = 'TX'
);

-- CTE (Common Table Expression) — Much cleaner
WITH texas_customers AS (
    SELECT customer_id FROM customers WHERE state = 'TX'
),
high_risk_loans AS (
    SELECT * FROM loans WHERE ltv > 90 OR credit_score < 620
)
SELECT l.loan_id, l.loan_amount, c.name
FROM high_risk_loans l
JOIN customers c ON l.customer_id = c.customer_id
WHERE c.customer_id IN (SELECT customer_id FROM texas_customers);

-- Multi-step CTE for reporting
WITH monthly_stats AS (
    SELECT
        DATE_TRUNC('month', origination_date) AS month,
        COUNT(*) AS new_loans,
        SUM(loan_amount) AS total_volume,
        AVG(credit_score) AS avg_fico
    FROM loans
    GROUP BY 1
),
with_growth AS (
    SELECT *,
        LAG(total_volume) OVER (ORDER BY month) AS prev_volume,
        ROUND((total_volume - LAG(total_volume) OVER (ORDER BY month)) /
              LAG(total_volume) OVER (ORDER BY month) * 100, 2) AS mom_growth_pct
    FROM monthly_stats
)
SELECT * FROM with_growth ORDER BY month;
```

---

## 💼 Finance-Specific SQL Patterns

### Default Rate Calculation
```sql
SELECT
    EXTRACT(YEAR FROM origination_date) AS year,
    loan_type,
    COUNT(*) AS total_loans,
    SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) AS defaulted_loans,
    ROUND(
        100.0 * SUM(CASE WHEN default_flag = 1 THEN 1 ELSE 0 END) / COUNT(*),
        2
    ) AS default_rate_pct
FROM loans
GROUP BY 1, 2
ORDER BY 1, 4 DESC;
```

### Risk Bucket Segmentation
```sql
SELECT
    CASE
        WHEN credit_score >= 750 THEN '1. Excellent (750+)'
        WHEN credit_score >= 700 THEN '2. Good (700-749)'
        WHEN credit_score >= 650 THEN '3. Fair (650-699)'
        WHEN credit_score >= 600 THEN '4. Poor (600-649)'
        ELSE '5. Very Poor (<600)'
    END AS credit_tier,
    COUNT(*) AS loan_count,
    AVG(loan_amount) AS avg_loan,
    AVG(interest_rate) AS avg_rate,
    AVG(CASE WHEN default_flag = 1 THEN 1.0 ELSE 0 END) AS default_rate
FROM loans
GROUP BY 1
ORDER BY 1;
```

### Delinquency Pipeline Report
```sql
WITH loan_status AS (
    SELECT
        loan_id,
        MAX(days_delinquent) AS max_delinquency,
        CASE
            WHEN MAX(days_delinquent) = 0 THEN 'Current'
            WHEN MAX(days_delinquent) <= 30 THEN '30 Days Late'
            WHEN MAX(days_delinquent) <= 60 THEN '60 Days Late'
            WHEN MAX(days_delinquent) <= 90 THEN '90 Days Late'
            ELSE 'Severely Delinquent (90+)'
        END AS status_bucket
    FROM loan_payments
    GROUP BY loan_id
)
SELECT
    status_bucket,
    COUNT(*) AS loan_count,
    ROUND(100.0 * COUNT(*) / SUM(COUNT(*)) OVER (), 2) AS pct_of_portfolio
FROM loan_status
GROUP BY status_bucket
ORDER BY 2 DESC;
```

---

## 🧠 Top 10 SQL Interview Questions at Finance Companies

1. Find the top 3 customers by loan amount in each state
2. Calculate month-over-month growth in origination volume
3. Find all customers who have more than one loan
4. What is the running default rate over time?
5. Find loans where the payment amount has increased since origination
6. List customers who took a loan but never made a payment
7. Calculate 90-day delinquency rate by loan type
8. Find the median credit score (hint: use PERCENTILE_CONT)
9. Write a query to detect duplicate records
10. Calculate cohort retention: of loans originated in month X, what % are still active 12 months later?

---
**Back to Home →** [[Dashboard]]
**Next Study Note →** [[ML Notes]]
