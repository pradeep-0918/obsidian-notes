# 10 — SQL Constraints

#beginner #ddl #interview

---

## 🧠 Introduction

Constraints are **rules enforced at the database level** to maintain data integrity. They prevent bad data from ever entering your database — something application code alone can't guarantee.

> Database constraints are your last line of defense against dirty data.

---

## 📋 Types of Constraints

| Constraint | Purpose |
|-----------|---------|
| `NOT NULL` | Column must have a value |
| `UNIQUE` | No duplicate values in the column |
| `PRIMARY KEY` | Uniquely identifies each row (NOT NULL + UNIQUE) |
| `FOREIGN KEY` | Links to another table's primary key |
| `CHECK` | Value must meet a condition |
| `DEFAULT` | Provides a value when none is given |
| `AUTO_INCREMENT` | Auto-generate sequential numbers |

---

## 🔷 NOT NULL

```sql
CREATE TABLE employees (
    emp_id   INT PRIMARY KEY AUTO_INCREMENT,
    name     VARCHAR(100) NOT NULL,   -- must always have a name
    salary   DECIMAL(10,2) NOT NULL,
    email    VARCHAR(255)             -- nullable — email optional
);

-- Add NOT NULL to existing column
ALTER TABLE employees MODIFY COLUMN name VARCHAR(100) NOT NULL;
```

---

## 🔶 UNIQUE

```sql
CREATE TABLE users (
    user_id  INT PRIMARY KEY AUTO_INCREMENT,
    email    VARCHAR(255) UNIQUE,     -- no two users can share an email
    username VARCHAR(50) UNIQUE
);

-- Composite UNIQUE (combination must be unique)
CREATE TABLE enrollments (
    student_id INT,
    course_id  INT,
    UNIQUE KEY uq_student_course (student_id, course_id)
);
-- Same student can enroll in different courses,
-- same course can have different students,
-- but same student+course combo must be unique.
```

---

## 🔑 PRIMARY KEY

```sql
-- Single column PK
CREATE TABLE products (
    product_id INT PRIMARY KEY AUTO_INCREMENT,
    name       VARCHAR(100)
);

-- Composite PK (combination is the key)
CREATE TABLE order_items (
    order_id   INT,
    product_id INT,
    quantity   INT,
    PRIMARY KEY (order_id, product_id)
);
```

---

## 🔗 FOREIGN KEY

```sql
CREATE TABLE orders (
    order_id  INT PRIMARY KEY AUTO_INCREMENT,
    user_id   INT NOT NULL,
    amount    DECIMAL(10,2),
    
    CONSTRAINT fk_orders_user
        FOREIGN KEY (user_id) 
        REFERENCES users(user_id)
        ON DELETE CASCADE       -- delete orders if user deleted
        ON UPDATE CASCADE       -- update orders if user_id changes
);
```

### ON DELETE / ON UPDATE Options

| Option | Behavior |
|--------|---------|
| `CASCADE` | Propagate delete/update to child rows |
| `SET NULL` | Set FK column to NULL in child rows |
| `RESTRICT` | Prevent parent delete/update if children exist |
| `NO ACTION` | Same as RESTRICT in MySQL |
| `SET DEFAULT` | Set to default value (rare) |

```sql
-- Example: if user deleted, set their orders' user_id to NULL
FOREIGN KEY (user_id) REFERENCES users(user_id) ON DELETE SET NULL

-- Example: prevent deleting a department that has employees
FOREIGN KEY (dept_id) REFERENCES departments(dept_id) ON DELETE RESTRICT
```

---

## ✅ CHECK Constraint

```sql
CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    name   VARCHAR(100),
    salary DECIMAL(10,2) CHECK (salary > 0),
    age    INT CHECK (age BETWEEN 18 AND 65),
    gender CHAR(1) CHECK (gender IN ('M', 'F', 'O'))
);

-- Named CHECK constraint
CREATE TABLE products (
    product_id INT PRIMARY KEY,
    price      DECIMAL(10,2),
    discount   DECIMAL(5,2),
    CONSTRAINT chk_discount CHECK (discount < price)
);
```

---

## 🔧 DEFAULT

```sql
CREATE TABLE orders (
    order_id   INT PRIMARY KEY AUTO_INCREMENT,
    status     VARCHAR(20) DEFAULT 'pending',
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    is_active  BOOLEAN DEFAULT TRUE
);

-- Row inserted without status → gets 'pending' automatically
INSERT INTO orders (amount) VALUES (500.00);
```

---

## 🌍 Real-World Use Case — Banking System

```sql
CREATE TABLE accounts (
    acc_id      INT PRIMARY KEY AUTO_INCREMENT,
    user_id     INT NOT NULL,
    acc_type    ENUM('savings','current','loan') NOT NULL,
    balance     DECIMAL(15,2) NOT NULL DEFAULT 0.00,
    currency    CHAR(3) NOT NULL DEFAULT 'INR',
    opened_date DATE NOT NULL DEFAULT (CURDATE()),
    is_active   BOOLEAN DEFAULT TRUE,
    
    CONSTRAINT chk_balance CHECK (balance >= 0),
    CONSTRAINT chk_currency CHECK (currency IN ('INR', 'USD', 'EUR', 'GBP')),
    CONSTRAINT fk_acc_user FOREIGN KEY (user_id) 
        REFERENCES users(user_id) ON DELETE RESTRICT
);
```

---

## 💼 Interview Questions

1. **What is the difference between PRIMARY KEY and UNIQUE?**
   > Both enforce uniqueness, but: PK cannot be NULL (implicit NOT NULL), only one PK per table. UNIQUE allows one NULL, multiple UNIQUE constraints per table are allowed.

2. **What are ON DELETE CASCADE and ON DELETE RESTRICT?**
   > CASCADE: automatically deletes child records when parent is deleted. RESTRICT: prevents parent deletion if child records exist.

3. **Can you have multiple NOT NULL constraints in a table?**
   > Yes — you can apply NOT NULL to as many columns as needed.

4. **What happens if you insert a row that violates a CHECK constraint?**
   > MySQL raises an error and rejects the INSERT.

5. **Scenario:** *"A user complains that two accounts were created with the same email. How do you fix this going forward?"*
   > Add a UNIQUE constraint on the email column: `ALTER TABLE users ADD UNIQUE (email);`

---

## ⚠️ Common Mistakes

- Forgetting that PRIMARY KEY is implicitly NOT NULL
- Not using FOREIGN KEY constraints — leads to orphaned records
- Using `NULL` as default for NOT NULL columns — contradicts itself
- Forgetting that UNIQUE allows exactly one NULL value

---

## 🔗 Related Topics

- [[11 - Keys]] — Primary, Natural, Surrogate, Candidate keys
- [[07 - CREATE TABLE Variants]] — Creating tables with constraints
- [[08 - ALTER TABLE]] — Adding constraints after creation
- [[31 - Normalization]] — Designing tables to avoid redundancy
- [[30 - ACID Properties]] — How constraints support consistency
