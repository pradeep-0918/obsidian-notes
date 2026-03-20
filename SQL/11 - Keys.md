# 11 — Keys (Primary, Natural, Surrogate, Super, Candidate)

#beginner #intermediate #interview

---

## 🧠 Introduction

Keys are the foundation of **relational database design**. They identify rows uniquely, establish relationships, and enable efficient querying. This topic is heavily tested in interviews.

---

## 🗺️ Key Hierarchy Overview

```
All Attributes in a Table
    └── Super Keys (any combo that uniquely identifies a row)
            └── Candidate Keys (minimal super keys)
                    ├── Primary Key (chosen candidate key)
                    └── Alternate Keys (non-chosen candidates)
```

---

## 🔑 Primary Key

**The chosen unique identifier for each row.** There can be only ONE per table.

Rules:
- Must be UNIQUE
- Cannot be NULL
- Should never change (stable identity)

```sql
-- Single column PK
CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    name   VARCHAR(100)
);

-- Composite PK
CREATE TABLE order_items (
    order_id   INT,
    product_id INT,
    quantity   INT,
    PRIMARY KEY (order_id, product_id)
);
```

---

## 🌿 Natural Key

A key that comes from the **real-world meaning** of the data.

```sql
-- Passport number is a natural key for a person (globally unique IRL)
CREATE TABLE travelers (
    passport_no VARCHAR(20) PRIMARY KEY,  -- natural key
    name        VARCHAR(100),
    nationality VARCHAR(50)
);

-- PAN card number as natural key
CREATE TABLE taxpayers (
    pan_number CHAR(10) PRIMARY KEY,  -- natural, real-world identifier
    name       VARCHAR(100)
);
```

**Pros:** Meaningful, no extra column needed
**Cons:** May change (passport renewal), may expose sensitive data, cross-system inconsistency

---

## 🔢 Surrogate Key

An **artificially generated** key with no business meaning.

```sql
-- AUTO_INCREMENT surrogate key (most common in MySQL)
CREATE TABLE customers (
    customer_id INT PRIMARY KEY AUTO_INCREMENT,  -- surrogate
    email       VARCHAR(255) UNIQUE,              -- natural (business identifier)
    name        VARCHAR(100)
);

-- UUID surrogate key (distributed systems)
CREATE TABLE events (
    event_id CHAR(36) PRIMARY KEY DEFAULT (UUID()),  -- surrogate
    event_type VARCHAR(50),
    created_at DATETIME
);
```

**Pros:** Stable, short, no business meaning leakage, works across systems
**Cons:** Extra column, needs JOIN to get human-readable identity

> **Best practice:** Use surrogate keys as PK in most cases. Keep natural key as UNIQUE constraint.

---

## 🎓 Candidate Key

**Any column (or combination) that COULD be the primary key** — minimal super key.

```sql
-- In a users table:
-- Candidate keys:
--   1. user_id (if auto-incremented)
--   2. email (globally unique per business rule)
--   3. phone_number (if enforced unique)

-- Choose one as PK → others become Alternate Keys
CREATE TABLE users (
    user_id      INT PRIMARY KEY AUTO_INCREMENT,  -- chosen PK
    email        VARCHAR(255) UNIQUE,              -- alternate key
    phone_number VARCHAR(15) UNIQUE               -- alternate key
);
```

---

## 🧩 Super Key

**Any combination of columns that uniquely identifies a row** — not necessarily minimal.

```sql
-- In employees table with columns: emp_id, email, name, dept
-- Super keys include:
--   (emp_id)                    → candidate key (minimal)
--   (email)                     → candidate key (minimal)
--   (emp_id, email)             → super key (not minimal — emp_id alone works)
--   (emp_id, name)              → super key
--   (emp_id, email, name, dept) → super key (but very redundant)
```

> Super key ⊃ Candidate key: every candidate key is a super key, but not vice versa.

---

## 🔗 Foreign Key

A column in one table that **references the primary key of another table**.

```sql
CREATE TABLE orders (
    order_id INT PRIMARY KEY AUTO_INCREMENT,
    user_id  INT NOT NULL,
    amount   DECIMAL(10,2),
    
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
-- user_id here is a foreign key — it references users.user_id
```

---

## 🔄 Composite Key

A primary key made of **two or more columns**.

```sql
-- Student enrollment: same student can't enroll in same course twice
CREATE TABLE enrollments (
    student_id INT,
    course_id  INT,
    grade      CHAR(2),
    PRIMARY KEY (student_id, course_id)  -- composite PK
);
```

---

## 📊 Keys Summary Table

| Key Type | Unique? | Null? | Count per Table | Source |
|----------|---------|-------|-----------------|--------|
| Super Key | Yes | No | Many | Any unique combination |
| Candidate Key | Yes | No | Many | Minimal super key |
| Primary Key | Yes | No | ONE | Chosen candidate key |
| Alternate Key | Yes | No | Many | Unchosen candidate keys |
| Natural Key | Yes | No | Depends | Real-world attribute |
| Surrogate Key | Yes | No | ONE | System-generated |
| Foreign Key | No | Yes | Many | References another PK |

---

## 🌍 Real-World Use Case — E-Commerce

```sql
CREATE TABLE products (
    product_id  INT PRIMARY KEY AUTO_INCREMENT,  -- surrogate PK
    sku         VARCHAR(50) UNIQUE NOT NULL,      -- natural alternate key
    barcode     VARCHAR(50) UNIQUE,               -- natural alternate key
    name        VARCHAR(200) NOT NULL,
    price       DECIMAL(10,2),
    category_id INT,
    FOREIGN KEY (category_id) REFERENCES categories(category_id)
);
-- product_id = surrogate PK
-- sku, barcode = candidate keys (alternate)
-- category_id = foreign key
```

---

## 💼 Interview Questions

1. **What is the difference between a Primary Key and a Candidate Key?**
   > A candidate key is any minimal unique identifier. The primary key is the ONE candidate key chosen to identify rows. Other candidate keys become alternate keys.

2. **Why prefer surrogate keys over natural keys?**
   > Natural keys may change (e.g., email change), may be long (less efficient as FK), may expose sensitive data. Surrogate keys are stable, short, and system-controlled.

3. **What is a composite key? When would you use one?**
   > A PK made of multiple columns. Used when no single column is unique but the combination is, e.g., (student_id, course_id) in an enrollments table.

4. **What is the difference between a Super Key and a Candidate Key?**
   > A super key is any combination of columns that uniquely identifies rows. A candidate key is a MINIMAL super key (no redundant columns).

5. **Scenario:** *"Your users table has both user_id (auto-increment) and email (unique). Which is the PK and why?"*
   > user_id is the PK (surrogate) — it's stable, short, integer (efficient), and system-generated. Email is an alternate key — it's a candidate key but we use it for business logic, not as PK.

---

## 🔗 Related Topics

- [[10 - SQL Constraints]] — PRIMARY KEY, UNIQUE, FOREIGN KEY constraints
- [[31 - Normalization]] — Keys determine normal forms
- [[25 - Indexes]] — PKs automatically create indexes
- [[22 - Joins]] — Joins happen via FK → PK relationships
