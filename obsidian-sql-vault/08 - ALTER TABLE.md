# 08 — ALTER TABLE / ALTER COLUMN

#beginner #ddl

---

## 🧠 Introduction

`ALTER TABLE` lets you **modify an existing table's structure** without dropping and recreating it. In real-world projects, schemas evolve — ALTER is how you adapt.

> Think of ALTER TABLE as "schema migration" — a critical skill in production environments.

---

## 📐 Syntax

```sql
ALTER TABLE table_name action;
```

---

## 🔧 All ALTER Operations

### Add a Column

```sql
-- Add a single column
ALTER TABLE employees ADD COLUMN phone VARCHAR(15);

-- Add with position control
ALTER TABLE employees ADD COLUMN phone VARCHAR(15) AFTER email;
ALTER TABLE employees ADD COLUMN emp_code VARCHAR(10) FIRST;

-- Add multiple columns at once
ALTER TABLE employees
    ADD COLUMN phone VARCHAR(15),
    ADD COLUMN address TEXT,
    ADD COLUMN is_active BOOLEAN DEFAULT TRUE;
```

### Drop a Column

```sql
ALTER TABLE employees DROP COLUMN phone;

-- Drop multiple columns
ALTER TABLE employees 
    DROP COLUMN address,
    DROP COLUMN temp_field;
```

### Modify a Column (Change Data Type or Constraint)

```sql
-- Change data type
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12,2);

-- Add NOT NULL constraint
ALTER TABLE employees MODIFY COLUMN email VARCHAR(255) NOT NULL;

-- Change default value
ALTER TABLE employees MODIFY COLUMN status VARCHAR(20) DEFAULT 'active';
```

### Rename a Column

```sql
-- MySQL 8.0+
ALTER TABLE employees RENAME COLUMN emp_name TO full_name;

-- Older MySQL (use CHANGE)
ALTER TABLE employees CHANGE emp_name full_name VARCHAR(100);
```

### Rename a Table

```sql
ALTER TABLE employees RENAME TO staff;
-- or
RENAME TABLE employees TO staff;
```

### Add / Drop Constraints

```sql
-- Add PRIMARY KEY
ALTER TABLE employees ADD PRIMARY KEY (emp_id);

-- Add FOREIGN KEY
ALTER TABLE orders ADD CONSTRAINT fk_user
    FOREIGN KEY (user_id) REFERENCES users(user_id);

-- Drop FOREIGN KEY (need the constraint name)
ALTER TABLE orders DROP FOREIGN KEY fk_user;

-- Add UNIQUE constraint
ALTER TABLE employees ADD UNIQUE (email);

-- Add INDEX
ALTER TABLE employees ADD INDEX idx_dept (dept_id);

-- Drop INDEX
ALTER TABLE employees DROP INDEX idx_dept;
```

### Change AUTO_INCREMENT starting value

```sql
-- Useful after bulk deletes to reset sequence
ALTER TABLE orders AUTO_INCREMENT = 1000;
```

---

## 🌍 Real-World Use Case

**Scenario:** Your e-commerce app needs to add a loyalty tier system:

```sql
-- Week 1: Add loyalty_points column
ALTER TABLE users ADD COLUMN loyalty_points INT DEFAULT 0;

-- Week 2: Add tier classification
ALTER TABLE users ADD COLUMN tier ENUM('bronze','silver','gold','platinum') DEFAULT 'bronze';

-- Week 3: Discovered email wasn't enforcing uniqueness
ALTER TABLE users ADD UNIQUE INDEX idx_email_unique (email);

-- Week 4: Renaming for clarity
ALTER TABLE users RENAME COLUMN joined_at TO registration_date;

-- Week 6: Add soft-delete support
ALTER TABLE users ADD COLUMN deleted_at DATETIME NULL DEFAULT NULL;

-- Performance: Add index for frequent filter
ALTER TABLE orders ADD INDEX idx_status_date (status, order_date);
```

---

## ⚙️ ALTER TABLE in Production — Important Notes

```sql
-- Check how many rows will be affected first
SELECT COUNT(*) FROM employees;
-- On millions of rows, ALTER TABLE locks the table!

-- MySQL 8.0 Online DDL — use ALGORITHM and LOCK hints
ALTER TABLE orders 
    ADD COLUMN notes TEXT,
    ALGORITHM=INPLACE, LOCK=NONE;  -- Non-blocking!
```

> For large tables (millions of rows), use tools like **pt-online-schema-change** or **gh-ost** to perform zero-downtime ALTER.

---

## 💼 Interview Questions

1. **What is the difference between MODIFY and CHANGE in ALTER TABLE?**
   > `MODIFY` changes data type/constraints of a column (same name). `CHANGE` can also rename the column.

2. **Can you rollback an ALTER TABLE statement in MySQL?**
   > No — ALTER TABLE is DDL and auto-commits in MySQL.

3. **What happens when you ALTER a large production table?**
   > MySQL may lock the table during ALTER, blocking reads/writes. Use `ALGORITHM=INPLACE, LOCK=NONE` for online DDL or use pt-osc/gh-ost tools.

4. **How do you add a NOT NULL column to a table that already has data?**
   > Either provide a DEFAULT value, or: add as nullable → populate data → add NOT NULL constraint.

   ```sql
   -- Safe approach:
   ALTER TABLE employees ADD COLUMN department VARCHAR(50) NULL;
   UPDATE employees SET department = 'Unknown' WHERE department IS NULL;
   ALTER TABLE employees MODIFY COLUMN department VARCHAR(50) NOT NULL;
   ```

---

## ⚠️ Common Mistakes

- Running ALTER on large tables without checking row count first — can cause downtime
- Dropping a column that a foreign key depends on — causes error
- Forgetting that column changes affect dependent views and stored procedures

---

## 🔗 Related Topics

- [[07 - CREATE TABLE Variants]] — Creating tables
- [[10 - SQL Constraints]] — Adding constraints
- [[25 - Indexes]] — Adding indexes via ALTER
- [[05 - DDL DML TCL DCL]] — DDL category
