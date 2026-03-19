# 🏗️ Data Modeling

> **Level:** Beginner-Intermediate | **Stage:** 1 of 5
> Data modeling is the art of designing *how data is structured* before storing it — good models make queries fast and pipelines simple.

---

## 🤔 What is Data Modeling?

Data modeling is designing the **structure of your database** — which tables exist, what columns they have, and how they relate to each other.

> **Real-world analogy:** Before building a house, you draw blueprints. Data modeling is the blueprint for your database.

---

## 🧠 Key Concepts

### 1. Normalization (OLTP — Transactional Systems)
Organizing data to **reduce redundancy**.

| Normal Form | Rule |
|-------------|------|
| 1NF | Each column has atomic (single) values |
| 2NF | No partial dependencies on composite keys |
| 3NF | No transitive dependencies |

```
✅ Good (normalized):
  users(user_id, name, city_id)
  cities(city_id, city_name, country)

❌ Bad (denormalized):
  users(user_id, name, city_name, country)  ← redundant!
```

### 2. Star Schema (OLAP — Analytics Systems) ⭐
Used in **data warehouses**. One central **fact table** surrounded by **dimension tables**.

```
        [dim_date]
            |
[dim_product] — [fact_sales] — [dim_customer]
            |
        [dim_store]
```

- **Fact table:** Measurable events (sales, clicks, orders) — contains metrics + foreign keys
- **Dimension table:** Descriptive context (who, what, when, where)

```sql
-- Fact table example
CREATE TABLE fact_sales (
  sale_id INT, date_id INT, product_id INT,
  customer_id INT, quantity INT, revenue FLOAT
);

-- Dimension table example
CREATE TABLE dim_product (
  product_id INT, product_name TEXT, category TEXT, brand TEXT
);
```

### 3. Snowflake Schema
Like Star Schema, but dimension tables are **further normalized** (broken into sub-tables).
- Saves storage
- More complex queries
- Use when dimensions are large

### 4. Slowly Changing Dimensions (SCD)
How do you handle changes in dimension data over time?

| Type | Strategy | Example |
|------|----------|---------|
| SCD Type 1 | Overwrite old value | Update customer city |
| SCD Type 2 | Add new row with date range | Track historical addresses |
| SCD Type 3 | Add a new column | Keep "previous_city" column |

### 5. Data Vault (Advanced)
For enterprise-scale warehouses. More flexible but more complex. Uses: **Hubs**, **Links**, **Satellites**.

---

## 🏭 Real-World Example

**E-commerce Analytics Model:**

```
fact_orders
  ├── order_id (PK)
  ├── customer_id (FK → dim_customer)
  ├── product_id (FK → dim_product)
  ├── date_id (FK → dim_date)
  ├── quantity
  └── total_amount

dim_customer: customer_id, name, email, country
dim_product: product_id, name, category, price
dim_date: date_id, date, month, quarter, year
```

---

## 🛠️ Tools & Technologies

- **Design:** dbdiagram.io, Lucidchart, draw.io
- **dbt:** Implements models as SQL files in a version-controlled project
- **Databases:** PostgreSQL, Snowflake, BigQuery (all use star schema)
- **ERD tools:** pgAdmin, DBeaver (auto-generate diagrams)

---

## ✅ Mini Practice Tasks

- [ ] Draw an ERD for a simple blog (users, posts, comments, tags)
- [ ] Design a star schema for an online store with 3 dimension tables
- [ ] Identify which SCD type you'd use for a customer's email address change
- [ ] Write CREATE TABLE statements for a star schema of your choice

---

## 🔗 Related Notes

- [[SQL for Data Engineering]] — You query these models using SQL
- [[Data Warehousing]] — Warehouses implement star/snowflake schemas
- [[ETL vs ELT]] — Transformation step often produces the final modeled tables
- [[Data Lakes vs Data Warehouses]] — Lakes are schema-on-read; warehouses are schema-on-write

---

## ➡️ Next Step

[[ETL vs ELT]] — Now that you understand data structure, learn how data gets *moved and transformed* into these models.
