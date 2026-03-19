# 31 — Normalization

#intermediate #advanced #interview #dataengineering

---

## 🧠 Introduction

Normalization is the process of **organizing a database to reduce data redundancy and improve data integrity**. It's done by dividing tables and defining relationships between them.

> Normalization = eliminating "bad" data repetition. Every piece of data lives in exactly one place.

---

## 🔴 The Problem: Un-normalized Data (0NF)

```
ORDERS TABLE (Un-normalized):
+----------+----------+----------+---------+----------+----------+----------+
| order_id | cust_name| cust_email| cust_city| prod1    | prod2    | prod3    |
+----------+----------+----------+---------+----------+----------+----------+
| 101      | Priya    | p@g.com  | Chennai | iPhone   | AirPods  |          |
| 102      | Priya    | p@g.com  | Chennai | Samsung  |          |          |
+----------+----------+----------+---------+----------+----------+----------+
```

Problems:
- **Redundancy**: Priya's info repeated in every order
- **Update anomaly**: Change Priya's city → update every row
- **Insert anomaly**: Can't add customer without an order
- **Delete anomaly**: Delete last order → lose customer info

---

## 1️⃣ First Normal Form (1NF)

**Rules:**
- All values are **atomic** (no multi-valued columns)
- No **repeating groups** (no prod1, prod2, prod3)
- Each row is **uniquely identifiable**

```sql
-- 1NF: Each order-product combination is a separate row
CREATE TABLE orders_1nf (
    order_id    INT,
    customer_id INT,
    cust_name   VARCHAR(100),
    cust_email  VARCHAR(255),
    cust_city   VARCHAR(50),
    product_name VARCHAR(100),
    quantity     INT,
    price        DECIMAL(10,2),
    PRIMARY KEY (order_id, product_name)
);
```

Still has redundancy (cust_name, cust_email in every order row).

---

## 2️⃣ Second Normal Form (2NF)

**Rules (1NF + this):**
- All non-key attributes must depend on the **ENTIRE** primary key (no partial dependency)

```sql
-- Problem: In orders_1nf with PK(order_id, product_name):
-- cust_name depends only on order_id (partial dependency → violation!)
-- product_name's attributes depend only on product_name (another partial dependency!)

-- Fix: Separate tables for customers, products, and order items

CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,
    name        VARCHAR(100),
    email       VARCHAR(255) UNIQUE,
    city        VARCHAR(50)
);

CREATE TABLE orders (
    order_id    INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT,  -- FK to customers
    order_date  DATE
);

CREATE TABLE order_items (
    order_id    INT,
    product_id  INT,
    quantity    INT,
    unit_price  DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id)
);
```

---

## 3️⃣ Third Normal Form (3NF)

**Rules (2NF + this):**
- No **transitive dependencies**: non-key column should not depend on another non-key column

```sql
-- Violation: employees table with (emp_id, dept_id, dept_name, dept_location)
-- dept_name depends on dept_id (not on emp_id) → transitive dependency!

-- Before (violates 3NF):
CREATE TABLE employees_bad (
    emp_id       INT PRIMARY KEY,
    name         VARCHAR(100),
    dept_id      INT,
    dept_name    VARCHAR(50),    -- depends on dept_id, not emp_id
    dept_location VARCHAR(100)   -- depends on dept_id, not emp_id
);

-- After (3NF compliant):
CREATE TABLE departments (
    dept_id      INT PRIMARY KEY,
    dept_name    VARCHAR(50),
    dept_location VARCHAR(100)
);

CREATE TABLE employees (
    emp_id   INT PRIMARY KEY,
    name     VARCHAR(100),
    dept_id  INT,
    FOREIGN KEY (dept_id) REFERENCES departments(dept_id)
);
```

---

## ⭐ BCNF (Boyce-Codd Normal Form)

Stricter form of 3NF: **every determinant must be a candidate key**.

```sql
-- Scenario: course_enrollments (student_id, course_id, instructor_id)
-- Each course has ONE instructor, each instructor teaches ONE course
-- Dependency: instructor_id → course_id (but instructor_id is not a key)
-- This violates BCNF!

-- Fix: separate teacher-course assignment
CREATE TABLE course_instructor (
    course_id     INT PRIMARY KEY,
    instructor_id INT UNIQUE
);

CREATE TABLE enrollments (
    student_id INT,
    course_id  INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (course_id) REFERENCES course_instructor(course_id)
);
```

---

## 🔄 Denormalization — When to Break the Rules

In **Data Warehousing and analytics**, denormalization is intentional for performance:

```sql
-- Denormalized fact table for analytics (common in OLAP):
CREATE TABLE fact_orders (
    order_id     INT,
    order_date   DATE,
    customer_id  INT,
    customer_name VARCHAR(100),  -- denormalized!
    customer_city VARCHAR(50),   -- denormalized!
    product_id   INT,
    product_name VARCHAR(100),   -- denormalized!
    category     VARCHAR(50),    -- denormalized!
    quantity     INT,
    revenue      DECIMAL(10,2)
);
-- Fewer JOINs = faster analytics queries
```

> OLTP systems: normalize for write performance and integrity
> OLAP/DW systems: denormalize for read/query performance

---

## 💼 Interview Questions

1. **What is normalization? Why do we do it?**
   > Organizing a database to eliminate data redundancy and dependency anomalies. Reduces storage, prevents update/insert/delete anomalies, ensures data integrity.

2. **What is the difference between 2NF and 3NF?**
   > 2NF removes partial dependencies (non-key depends on PART of composite PK). 3NF removes transitive dependencies (non-key column depends on another non-key column).

3. **What is a transitive dependency? Give an example.**
   > A → B and B → C, so A → C indirectly. Example: emp_id → dept_id → dept_name. dept_name depends on dept_id, not directly on emp_id.

4. **When would you deliberately denormalize?**
   > For analytics/OLAP workloads where query speed matters more than write efficiency. Pre-joining data avoids expensive JOINs at query time.

5. **Scenario:** *"Design a schema for a library management system."*
   ```
   books (book_id, title, isbn, publisher_id)
   publishers (publisher_id, name, country)
   authors (author_id, name)
   book_authors (book_id, author_id)  -- many-to-many
   members (member_id, name, email)
   loans (loan_id, book_id, member_id, loan_date, return_date)
   ```

---

## 🔗 Related Topics

- [[11 - Keys]] — Normal forms depend on key analysis
- [[10 - SQL Constraints]] — Constraints enforce normalization rules
- [[22 - Joins]] — Normalized tables require JOINs to reunite data
- [[32 - Slowly Changing Dimensions]] — Normalization concepts in data warehouses
- [[34 - Data Engineering Projects]] — Normalization vs star schema design
