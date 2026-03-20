# 01 — Introduction to Databases & SQL

#beginner #foundation

---

## 🧠 Introduction

Before writing a single SQL query, you need to understand **why databases exist**.

> Think of a database as a highly organized Excel spreadsheet — but one that can handle millions of rows, multiple users simultaneously, and complex relationships between data.

A **Database** is an organized collection of structured data stored electronically.
A **DBMS** (Database Management System) is the software that manages that data.
**SQL** (Structured Query Language) is the language you use to talk to a RDBMS.

---

## 🏗️ Types of Databases

| Type | Description | Examples |
|------|-------------|---------|
| **Relational (RDBMS)** | Data in tables with relationships | MySQL, PostgreSQL, Oracle |
| **NoSQL** | Flexible schema, key-value/document | MongoDB, Redis, Cassandra |
| **Data Warehouse** | Optimized for analytics | Snowflake, BigQuery, Redshift |
| **In-Memory** | Extremely fast, temporary | Redis, Memcached |

> For Data Engineering roles, you'll primarily work with **RDBMS + Data Warehouses**.

---

## 🔑 Core Concepts

### Table
The fundamental unit — rows (records) and columns (fields).

```
+----+----------+--------+------------+
| id | name     | salary | department |
+----+----------+--------+------------+
|  1 | Priya    | 75000  | Engineering|
|  2 | Ravi     | 55000  | Marketing  |
+----+----------+--------+------------+
```

### Schema
The **blueprint** of a database — defines tables, columns, data types, and relationships.

### Row / Record
A single entry in a table. (Priya's data above is one row.)

### Column / Field
A specific attribute across all rows. (`salary` column holds all salary values.)

### Primary Key
A unique identifier for each row. No two rows can have the same primary key. ([[11 - Keys]])

### Foreign Key
A column in one table that references the primary key of another table — creating a **relationship**.

---

## 🔗 Relational Model Example

**E-commerce System:**

```
users (user_id, name, email)
         ↕ (user_id is foreign key in orders)
orders (order_id, user_id, total_amount, order_date)
         ↕ (order_id is foreign key in order_items)
order_items (item_id, order_id, product_name, quantity)
```

---

## 🌍 Real-World Use Case

**Scenario:** You work at a food delivery company (like Swiggy/Zomato).

- **users** table → customer profiles
- **restaurants** table → restaurant details
- **orders** table → every order placed
- **deliveries** table → driver + delivery status

SQL lets you answer business questions like:
- *"Which customers ordered more than 5 times this month?"*
- *"What's the average delivery time per city?"*

---

## 💼 Interview Questions

1. **What is the difference between a database and a DBMS?**
   > A database is the data itself; a DBMS is the software (like MySQL) that stores, retrieves, and manages that data.

2. **What is the difference between SQL and MySQL?**
   > SQL is a language (standard). MySQL is a specific RDBMS that uses SQL as its query language.

3. **What is a schema?**
   > A schema is the logical structure/blueprint of a database — defining tables, columns, data types, and constraints.

4. **Why use a relational database over a spreadsheet?**
   > RDBMS handles concurrent users, enforces data integrity via constraints, supports complex relationships, and scales to millions of rows.

5. **Scenario:** *"Your company stores all data in Excel files. A new VP asks why they should switch to a database. What do you say?"*
   > Mention: data integrity, concurrent access, scalability, relationships between datasets, security (GRANT/REVOKE), and SQL querying power.

---

## 💡 Tips & Best Practices

- Always design your schema before writing queries
- Understand the **business domain** — know what each table represents
- RDBMS vs NoSQL choice depends on data structure and query patterns
- In Data Engineering: you'll often work with **both** RDBMS (source) and Data Warehouses (destination)

---

## ⚠️ Common Mistakes

- Confusing SQL (language) with MySQL (software)
- Treating NULL as zero or empty string — NULL means **unknown/missing** → [[16 - NULL Handling]]
- Ignoring relationships between tables → leads to poor schema design

---

## 🚀 Advanced Insights

- **OLTP vs OLAP:**
  - OLTP (Online Transaction Processing): Fast reads/writes, normalized, e.g., your app's MySQL database
  - OLAP (Online Analytical Processing): Complex queries, denormalized, e.g., Snowflake data warehouse
- In Data Engineering pipelines: data flows from OLTP → ETL → OLAP
- Understanding this distinction is critical for [[32 - Slowly Changing Dimensions]] and [[34 - Data Engineering Projects]]

---

## 🔗 Related Topics

- [[02 - MySQL Installation Windows]] — Set up your environment next
- [[05 - DDL DML TCL DCL]] — Learn SQL command categories
- [[11 - Keys]] — Primary, Foreign, and other key types
- [[31 - Normalization]] — Design databases properly
- [[30 - ACID Properties]] — Understand database reliability guarantees
