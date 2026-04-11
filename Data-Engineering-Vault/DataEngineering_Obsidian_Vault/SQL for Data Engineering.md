# 🗄️ SQL for Data Engineering

> **Level:** Beginner | **Stage:** 1 of 5
> SQL is the *lingua franca* of data. Every Data Engineer writes SQL every single day.

---

## 🤔 What is SQL and Why Does It Matter?

SQL (Structured Query Language) lets you **talk to databases**.
- Retrieve data from tables
- Transform and aggregate data
- Join multiple tables together
- Create views, indexes, and optimized queries

> **Real-world analogy:** SQL is like asking a librarian to find specific books based on your criteria — the librarian knows exactly where everything is stored.

---

## 🧠 Key Concepts

### 1. Basic Querying
```sql
SELECT name, age, city
FROM users
WHERE age > 25
ORDER BY name ASC
LIMIT 10;
```

### 2. Aggregations
```sql
SELECT city, COUNT(*) AS total_users, AVG(age) AS avg_age
FROM users
GROUP BY city
HAVING COUNT(*) > 100;
```

### 3. Joins (Critical for DE!)
```sql
-- INNER JOIN: only matching rows
SELECT o.order_id, u.name, o.amount
FROM orders o
INNER JOIN users u ON o.user_id = u.id;

-- LEFT JOIN: all rows from left table
SELECT u.name, o.order_id
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;
```

### 4. Window Functions ⭐ (Advanced & Frequently Tested)
```sql
-- Rank users by sales within each region
SELECT name, region, sales,
  RANK() OVER (PARTITION BY region ORDER BY sales DESC) AS rank
FROM sales_data;

-- Running total
SELECT date, amount,
  SUM(amount) OVER (ORDER BY date) AS running_total
FROM transactions;
```

### 5. CTEs (Common Table Expressions)
```sql
WITH high_value_customers AS (
  SELECT user_id, SUM(amount) AS total_spent
  FROM orders
  GROUP BY user_id
  HAVING SUM(amount) > 1000
)
SELECT u.name, h.total_spent
FROM users u
JOIN high_value_customers h ON u.id = h.user_id;
```

### 6. Subqueries
```sql
SELECT name FROM users
WHERE id IN (
  SELECT user_id FROM orders WHERE amount > 500
);
```

---

## 🏭 Real-World Data Engineering Use Cases

| Use Case | SQL Technique |
|----------|---------------|
| Daily sales report | Aggregation + GROUP BY |
| Find duplicate records | GROUP BY + HAVING COUNT > 1 |
| Combine data from 2 tables | JOINs |
| Calculate 7-day rolling average | Window Functions |
| Stage → transform → load | CTEs chained together |
| Partitioned table queries | WHERE on partition column |

---

## 🛠️ Tools & Technologies

- **Databases:** PostgreSQL, MySQL, SQLite
- **Cloud Warehouses:** Snowflake, BigQuery, Redshift, Databricks SQL
- **Query Tools:** DBeaver, pgAdmin, DataGrip, VS Code + SQLTools
- **dbt:** Runs SQL transformations in a version-controlled, modular way

---

## ✅ Mini Practice Tasks

- [ ] Write a query to find the top 5 customers by total order value
- [ ] Use a window function to calculate a 7-day rolling average of daily sales
- [ ] Write a CTE that finds users who ordered more than 3 times
- [ ] Find duplicate emails in a `users` table
- [ ] Join 3 tables: `users`, `orders`, and `products` to get a full order report

---

## 🔗 Related Notes

- [[Python for Data Engineering]] — Use Python + SQL together (SQLAlchemy, pandas)
- [[Data Modeling]] — Design the tables you'll query
- [[Data Warehousing]] — SQL is the primary interface for warehouses
- [[ETL vs ELT]] — SQL is used heavily in the Transform step
- [[Interview Preparation]] — Window functions are a top interview topic

---

## ➡️ Next Step

[[Python for Data Engineering]] — Learn to automate SQL queries, process files, and build pipelines using Python.
