# 39 — SQL Practice Problems (30 Graded Problems)

#practice #interview #mustdo

> **Setup:** All problems use the sample schema below. Solve each one before checking the solution.

---

## 🏗️ Sample Schema

```sql
-- Users/Customers
CREATE TABLE users (
    user_id    INT PRIMARY KEY AUTO_INCREMENT,
    name       VARCHAR(100),
    email      VARCHAR(255) UNIQUE,
    city       VARCHAR(50),
    age        INT,
    joined_at  DATE
);

-- Products
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name       VARCHAR(200),
    category   VARCHAR(50),
    price      DECIMAL(10,2),
    stock      INT
);

-- Orders
CREATE TABLE orders (
    order_id   INT PRIMARY KEY AUTO_INCREMENT,
    user_id    INT,
    order_date DATE,
    status     ENUM('pending','shipped','delivered','cancelled'),
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);

-- Order Items (line items)
CREATE TABLE order_items (
    item_id    INT PRIMARY KEY AUTO_INCREMENT,
    order_id   INT,
    product_id INT,
    quantity   INT,
    unit_price DECIMAL(10,2),
    FOREIGN KEY (order_id)   REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);

-- Employees (for hierarchy problems)
CREATE TABLE employees (
    emp_id     INT PRIMARY KEY,
    name       VARCHAR(100),
    dept       VARCHAR(50),
    salary     DECIMAL(10,2),
    manager_id INT,
    hire_date  DATE,
    FOREIGN KEY (manager_id) REFERENCES employees(emp_id)
);
```

---

## 🟢 Easy (Problems 1–10)

### P01. List all users from Chennai, sorted by name

<details>
<summary>💡 Solution</summary>

```sql
SELECT user_id, name, email
FROM users
WHERE city = 'Chennai'
ORDER BY name;
```
</details>

---

### P02. Count total orders per status

<details>
<summary>💡 Solution</summary>

```sql
SELECT status, COUNT(*) AS order_count
FROM orders
GROUP BY status
ORDER BY order_count DESC;
```
</details>

---

### P03. Find all products with price between ₹500 and ₹5000

<details>
<summary>💡 Solution</summary>

```sql
SELECT product_id, name, category, price
FROM products
WHERE price BETWEEN 500 AND 5000
ORDER BY price;
```
</details>

---

### P04. Find users who joined in 2024

<details>
<summary>💡 Solution</summary>

```sql
SELECT name, email, joined_at
FROM users
WHERE YEAR(joined_at) = 2024
-- OR (index-friendly):
-- WHERE joined_at >= '2024-01-01' AND joined_at < '2025-01-01'
ORDER BY joined_at;
```
</details>

---

### P05. Get the total revenue from delivered orders

<details>
<summary>💡 Solution</summary>

```sql
SELECT SUM(oi.quantity * oi.unit_price) AS total_revenue
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
WHERE o.status = 'delivered';
```
</details>

---

### P06. List products with zero stock

<details>
<summary>💡 Solution</summary>

```sql
SELECT product_id, name, category
FROM products
WHERE stock = 0
ORDER BY category, name;
```
</details>

---

### P07. Find users whose email is from Gmail

<details>
<summary>💡 Solution</summary>

```sql
SELECT name, email
FROM users
WHERE email LIKE '%@gmail.com';
```
</details>

---

### P08. Get the average age of users per city

<details>
<summary>💡 Solution</summary>

```sql
SELECT city,
       ROUND(AVG(age), 1) AS avg_age,
       COUNT(*)           AS user_count
FROM users
WHERE age IS NOT NULL
GROUP BY city
ORDER BY avg_age DESC;
```
</details>

---

### P09. Find orders placed in the last 30 days

<details>
<summary>💡 Solution</summary>

```sql
SELECT o.order_id, u.name, o.order_date, o.status
FROM orders o
JOIN users u ON o.user_id = u.user_id
WHERE o.order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
ORDER BY o.order_date DESC;
```
</details>

---

### P10. List all categories with their product count and average price

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    category,
    COUNT(*)            AS product_count,
    ROUND(AVG(price),2) AS avg_price,
    MIN(price)          AS min_price,
    MAX(price)          AS max_price
FROM products
GROUP BY category
ORDER BY product_count DESC;
```
</details>

---

## 🟡 Medium (Problems 11–20)

### P11. Find customers who have never placed an order

<details>
<summary>💡 Solution</summary>

```sql
-- Method 1: LEFT JOIN anti-join
SELECT u.user_id, u.name, u.email
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
WHERE o.order_id IS NULL;

-- Method 2: NOT EXISTS
SELECT user_id, name, email FROM users u
WHERE NOT EXISTS (SELECT 1 FROM orders WHERE user_id = u.user_id);
```
</details>

---

### P12. Find the top 5 customers by total spend

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    u.user_id,
    u.name,
    u.city,
    COUNT(DISTINCT o.order_id)          AS total_orders,
    SUM(oi.quantity * oi.unit_price)    AS lifetime_value
FROM users u
JOIN orders o     ON u.user_id     = o.user_id
JOIN order_items oi ON o.order_id  = oi.order_id
WHERE o.status = 'delivered'
GROUP BY u.user_id, u.name, u.city
ORDER BY lifetime_value DESC
LIMIT 5;
```
</details>

---

### P13. Find products ordered by more than 100 unique customers

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    p.product_id,
    p.name,
    p.category,
    COUNT(DISTINCT o.user_id) AS unique_buyers
FROM products p
JOIN order_items oi ON p.product_id = oi.product_id
JOIN orders o       ON oi.order_id  = o.order_id
WHERE o.status != 'cancelled'
GROUP BY p.product_id, p.name, p.category
HAVING unique_buyers > 100
ORDER BY unique_buyers DESC;
```
</details>

---

### P14. Monthly revenue for the current year

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    DATE_FORMAT(o.order_date, '%Y-%m')      AS month,
    COUNT(DISTINCT o.order_id)              AS orders,
    SUM(oi.quantity * oi.unit_price)        AS revenue,
    LAG(SUM(oi.quantity * oi.unit_price))
        OVER (ORDER BY DATE_FORMAT(o.order_date, '%Y-%m')) AS prev_month_rev
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
WHERE o.status = 'delivered'
  AND YEAR(o.order_date) = YEAR(CURDATE())
GROUP BY DATE_FORMAT(o.order_date, '%Y-%m')
ORDER BY month;
```
</details>

---

### P15. Find employees earning more than their department average

<details>
<summary>💡 Solution</summary>

```sql
SELECT e.emp_id, e.name, e.dept, e.salary,
       ROUND(dept_avg.avg_sal, 2) AS dept_avg_salary
FROM employees e
JOIN (
    SELECT dept, AVG(salary) AS avg_sal
    FROM employees
    GROUP BY dept
) dept_avg ON e.dept = dept_avg.dept
WHERE e.salary > dept_avg.avg_sal
ORDER BY e.dept, e.salary DESC;
```
</details>

---

### P16. Rank customers within each city by their total spend

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    u.name,
    u.city,
    SUM(oi.quantity * oi.unit_price) AS total_spend,
    RANK() OVER (PARTITION BY u.city ORDER BY SUM(oi.quantity * oi.unit_price) DESC) AS city_rank
FROM users u
JOIN orders o     ON u.user_id    = o.user_id
JOIN order_items oi ON o.order_id = oi.order_id
WHERE o.status = 'delivered'
GROUP BY u.user_id, u.name, u.city
ORDER BY u.city, city_rank;
```
</details>

---

### P17. Find the most popular product category per city

<details>
<summary>💡 Solution</summary>

```sql
WITH city_category AS (
    SELECT
        u.city,
        p.category,
        SUM(oi.quantity) AS units_sold,
        ROW_NUMBER() OVER (PARTITION BY u.city ORDER BY SUM(oi.quantity) DESC) AS rn
    FROM users u
    JOIN orders o     ON u.user_id    = o.user_id
    JOIN order_items oi ON o.order_id = oi.order_id
    JOIN products p   ON oi.product_id = p.product_id
    WHERE o.status = 'delivered'
    GROUP BY u.city, p.category
)
SELECT city, category, units_sold AS top_category_units
FROM city_category
WHERE rn = 1
ORDER BY city;
```
</details>

---

### P18. Calculate a 3-day moving average of daily orders

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    order_date,
    COUNT(*) AS daily_orders,
    ROUND(
        AVG(COUNT(*)) OVER (
            ORDER BY order_date
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
        ), 2
    ) AS moving_avg_3d
FROM orders
WHERE status != 'cancelled'
GROUP BY order_date
ORDER BY order_date;
```
</details>

---

### P19. Find customers who ordered in consecutive months (at least 3 in a row)

<details>
<summary>💡 Solution</summary>

```sql
WITH monthly_orders AS (
    SELECT DISTINCT
        user_id,
        DATE_FORMAT(order_date, '%Y-%m') AS order_month
    FROM orders WHERE status != 'cancelled'
),
with_gaps AS (
    SELECT
        user_id,
        order_month,
        DATE_FORMAT(
            DATE_SUB(
                STR_TO_DATE(CONCAT(order_month, '-01'), '%Y-%m-%d'),
                INTERVAL ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY order_month) MONTH
            ), '%Y-%m'
        ) AS grp
    FROM monthly_orders
)
SELECT DISTINCT user_id
FROM with_gaps
GROUP BY user_id, grp
HAVING COUNT(*) >= 3;
```
</details>

---

### P20. Show each order with a running total per customer

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    o.user_id,
    u.name,
    o.order_id,
    o.order_date,
    oi_totals.order_total,
    SUM(oi_totals.order_total) OVER (
        PARTITION BY o.user_id
        ORDER BY o.order_date, o.order_id
    ) AS cumulative_spend
FROM orders o
JOIN users u ON o.user_id = u.user_id
JOIN (
    SELECT order_id, SUM(quantity * unit_price) AS order_total
    FROM order_items GROUP BY order_id
) oi_totals ON o.order_id = oi_totals.order_id
WHERE o.status = 'delivered'
ORDER BY o.user_id, o.order_date;
```
</details>

---

## 🔴 Hard (Problems 21–30)

### P21. Find products frequently bought together (co-occurrence)

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    p1.name AS product_1,
    p2.name AS product_2,
    COUNT(*) AS times_bought_together
FROM order_items oi1
JOIN order_items oi2 ON oi1.order_id = oi2.order_id
                     AND oi1.product_id < oi2.product_id  -- avoid duplicates
JOIN products p1 ON oi1.product_id = p1.product_id
JOIN products p2 ON oi2.product_id = p2.product_id
GROUP BY p1.product_id, p2.product_id, p1.name, p2.name
HAVING times_bought_together >= 5
ORDER BY times_bought_together DESC
LIMIT 10;
```
</details>

---

### P22. Detect users who placed orders from more than 2 different cities (fraud detection)

<details>
<summary>💡 Solution</summary>

```sql
-- Assume orders have a shipping_city column
SELECT
    o.user_id,
    u.name,
    COUNT(DISTINCT o.shipping_city) AS distinct_cities,
    GROUP_CONCAT(DISTINCT o.shipping_city ORDER BY o.shipping_city) AS cities
FROM orders o
JOIN users u ON o.user_id = u.user_id
WHERE o.order_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
GROUP BY o.user_id, u.name
HAVING distinct_cities > 2
ORDER BY distinct_cities DESC;
```
</details>

---

### P23. Find the first and last order date for each customer, plus the gap

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    u.user_id,
    u.name,
    MIN(o.order_date)                                     AS first_order,
    MAX(o.order_date)                                     AS last_order,
    COUNT(DISTINCT o.order_id)                            AS total_orders,
    DATEDIFF(MAX(o.order_date), MIN(o.order_date))        AS days_active,
    DATEDIFF(CURDATE(), MAX(o.order_date))                AS days_since_last_order,
    CASE
        WHEN DATEDIFF(CURDATE(), MAX(o.order_date)) > 180 THEN 'Churned'
        WHEN DATEDIFF(CURDATE(), MAX(o.order_date)) > 60  THEN 'At Risk'
        ELSE 'Active'
    END AS customer_status
FROM users u
JOIN orders o ON u.user_id = o.user_id
WHERE o.status = 'delivered'
GROUP BY u.user_id, u.name
ORDER BY days_since_last_order DESC;
```
</details>

---

### P24. Pivot: Show order counts by status for each month in 2024

<details>
<summary>💡 Solution</summary>

```sql
SELECT
    DATE_FORMAT(order_date, '%Y-%m') AS month,
    COUNT(CASE WHEN status = 'pending'   THEN 1 END) AS pending,
    COUNT(CASE WHEN status = 'shipped'   THEN 1 END) AS shipped,
    COUNT(CASE WHEN status = 'delivered' THEN 1 END) AS delivered,
    COUNT(CASE WHEN status = 'cancelled' THEN 1 END) AS cancelled,
    COUNT(*) AS total
FROM orders
WHERE YEAR(order_date) = 2024
GROUP BY DATE_FORMAT(order_date, '%Y-%m')
ORDER BY month;
```
</details>

---

### P25. Identify the second purchase date for each customer

<details>
<summary>💡 Solution</summary>

```sql
SELECT user_id, order_date AS second_purchase_date
FROM (
    SELECT
        user_id,
        order_date,
        ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY order_date) AS rn
    FROM orders
    WHERE status != 'cancelled'
) t
WHERE rn = 2;
```
</details>

---

### P26. Find employees with no direct reports (leaf nodes in hierarchy)

<details>
<summary>💡 Solution</summary>

```sql
SELECT e.emp_id, e.name, e.dept, e.salary
FROM employees e
WHERE e.emp_id NOT IN (
    SELECT DISTINCT manager_id
    FROM employees
    WHERE manager_id IS NOT NULL
)
ORDER BY e.dept, e.name;
```
</details>

---

### P27. Calculate customer retention: % who ordered again within 90 days of first order

<details>
<summary>💡 Solution</summary>

```sql
WITH first_orders AS (
    SELECT user_id, MIN(order_date) AS first_order_date
    FROM orders WHERE status = 'delivered'
    GROUP BY user_id
),
repeat_buyers AS (
    SELECT f.user_id
    FROM first_orders f
    JOIN orders o ON f.user_id = o.user_id
    WHERE o.order_date > f.first_order_date
      AND o.order_date <= DATE_ADD(f.first_order_date, INTERVAL 90 DAY)
      AND o.status = 'delivered'
)
SELECT
    COUNT(DISTINCT f.user_id)           AS total_customers,
    COUNT(DISTINCT r.user_id)           AS retained_customers,
    ROUND(COUNT(DISTINCT r.user_id) /
          COUNT(DISTINCT f.user_id) * 100, 2) AS retention_rate_pct
FROM first_orders f
LEFT JOIN repeat_buyers r ON f.user_id = r.user_id;
```
</details>

---

### P28. ABC Analysis: Classify products into A (top 20%), B (next 30%), C (bottom 50%) by revenue

<details>
<summary>💡 Solution</summary>

```sql
WITH product_revenue AS (
    SELECT
        p.product_id, p.name,
        SUM(oi.quantity * oi.unit_price) AS revenue
    FROM products p
    JOIN order_items oi ON p.product_id = oi.product_id
    JOIN orders o ON oi.order_id = o.order_id
    WHERE o.status = 'delivered'
    GROUP BY p.product_id, p.name
),
ranked AS (
    SELECT *,
        CUME_DIST() OVER (ORDER BY revenue DESC) AS cum_pct
    FROM product_revenue
)
SELECT
    product_id, name,
    ROUND(revenue, 2) AS revenue,
    ROUND(cum_pct * 100, 1) AS cumulative_pct,
    CASE
        WHEN cum_pct <= 0.20 THEN 'A'
        WHEN cum_pct <= 0.50 THEN 'B'
        ELSE 'C'
    END AS abc_class
FROM ranked
ORDER BY revenue DESC;
```
</details>

---

### P29. Find all manager chains for a given employee (recursive CTE)

<details>
<summary>💡 Solution</summary>

```sql
-- MySQL 8.0+ supports recursive CTEs
WITH RECURSIVE manager_chain AS (
    -- Base: start from the employee
    SELECT emp_id, name, manager_id, 0 AS level
    FROM employees
    WHERE emp_id = 5  -- target employee

    UNION ALL

    -- Recursive: get each manager's manager
    SELECT e.emp_id, e.name, e.manager_id, mc.level + 1
    FROM employees e
    JOIN manager_chain mc ON e.emp_id = mc.manager_id
)
SELECT level, emp_id, name
FROM manager_chain
ORDER BY level;
```
</details>

---

### P30. Design and query an SCD2 dimension for customer tier changes

<details>
<summary>💡 Solution</summary>

```sql
-- SCD2 dim_customer table (assume already populated)
-- Question: What tier was each customer when they made their highest-value order?

SELECT
    o.order_id,
    o.user_id,
    o.order_date,
    c.tier AS tier_at_time_of_order,
    oi_total.order_value
FROM orders o
JOIN order_items oi_total_sub ON o.order_id = oi_total_sub.order_id
JOIN (
    SELECT order_id, SUM(quantity * unit_price) AS order_value
    FROM order_items GROUP BY order_id
) oi_total ON o.order_id = oi_total.order_id
-- SCD2 join: find the customer record valid at order time
JOIN dim_customer c
  ON o.user_id = c.customer_id
  AND o.order_date BETWEEN c.effective_date
                       AND COALESCE(c.expiry_date, '9999-12-31')
WHERE o.status = 'delivered'
ORDER BY oi_total.order_value DESC
LIMIT 10;
```
</details>

---

## 📊 Progress Tracker

| # | Problem | Difficulty | Solved | Concepts |
|---|---------|-----------|--------|---------|
| P01 | Chennai users | 🟢 | ☐ | WHERE, ORDER BY |
| P05 | Total revenue | 🟢 | ☐ | JOIN, SUM |
| P11 | No orders | 🟡 | ☐ | LEFT JOIN, NOT EXISTS |
| P14 | Monthly revenue | 🟡 | ☐ | DATE_FORMAT, GROUP BY, LAG |
| P16 | Rank by city | 🟡 | ☐ | RANK, PARTITION BY |
| P18 | Moving average | 🟡 | ☐ | AVG OVER ROWS |
| P21 | Co-occurrence | 🔴 | ☐ | SELF JOIN, GROUP BY |
| P27 | Retention | 🔴 | ☐ | CTE, DATE math |
| P29 | Recursive CTE | 🔴 | ☐ | Recursive CTE |
| P30 | SCD2 query | 🔴 | ☐ | SCD2, date range JOIN |

---

## 🔗 Related Topics

- [[35 - Interview Questions Master]] — Conceptual questions
- [[23 - Window Functions]] — P16, P17, P18, P25
- [[22 - Joins]] — Most problems
- [[20 - Subqueries]] — P13, P14, P15
- [[32 - Slowly Changing Dimensions]] — P30
- [[18 - Date and Time]] — P09, P14, P23, P27
