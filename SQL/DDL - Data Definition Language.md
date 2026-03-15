

> [!info] What is DDL? DDL defines and manages the **structure** of database objects (schemas, tables, indexes, etc.). DDL statements are **auto-committed** — changes are permanent immediately and cannot be rolled back.

---

## Tags

#sql #ddl #database-basics

---

## Related Notes

- [[DML - Data Manipulation Language]]
- [[TCL - Transaction Control Language]]
- [[DCL - Data Control Language]]

---

## Core Commands

|Command|Purpose|
|---|---|
|`CREATE`|Create a new database object|
|`ALTER`|Modify an existing object|
|`DROP`|Permanently delete an object|
|`TRUNCATE`|Remove all rows, keep table structure|
|`RENAME`|Rename a database object|

---

## CREATE

```sql
-- Create a database
CREATE DATABASE company_db;

-- Create a table
CREATE TABLE employees (
    emp_id     INT PRIMARY KEY,
    name       VARCHAR(100) NOT NULL,
    department VARCHAR(50),
    salary     DECIMAL(10, 2),
    joined_on  DATE DEFAULT CURRENT_DATE
);

-- Create an index
CREATE INDEX idx_dept ON employees(department);
```

---

## ALTER

```sql
-- Add a column
ALTER TABLE employees ADD COLUMN email VARCHAR(100);

-- Modify column data type
ALTER TABLE employees MODIFY COLUMN salary DECIMAL(12, 2);

-- Rename a column (MySQL syntax)
ALTER TABLE employees RENAME COLUMN name TO full_name;

-- Drop a column
ALTER TABLE employees DROP COLUMN email;

-- Add a constraint
ALTER TABLE employees ADD CONSTRAINT chk_salary CHECK (salary > 0);
```

---

## DROP

```sql
-- Drop a table (permanent!)
DROP TABLE employees;

-- Drop with safety check
DROP TABLE IF EXISTS employees;

-- Drop a database
DROP DATABASE company_db;

-- Drop an index
DROP INDEX idx_dept ON employees;
```

> [!warning] Irreversible `DROP` permanently removes the object **and all its data**. Always double-check before running.

---

## TRUNCATE

```sql
-- Remove all rows but keep the table structure
TRUNCATE TABLE employees;
```

> [!tip] TRUNCATE vs DELETE
> 
> |Feature|TRUNCATE|DELETE|
> |---|---|---|
> |Speed|Fast|Slower|
> |Rollback|❌ Usually not|✅ Yes (with TCL)|
> |WHERE clause|❌ Not allowed|✅ Allowed|
> |Triggers|❌ Not fired|✅ Fired|

---

## RENAME

```sql
-- Rename a table
RENAME TABLE employees TO staff;

-- Using ALTER (PostgreSQL / SQL Server)
ALTER TABLE employees RENAME TO staff;
```

---

## Key Concepts

- **Auto-commit**: DDL changes are committed automatically — no `ROLLBACK` possible in most RDBMS.
- **Schema**: The logical structure/blueprint of a database object.
- **Constraints**: Rules enforced at the DDL level — `PRIMARY KEY`, `FOREIGN KEY`, `UNIQUE`, `NOT NULL`, `CHECK`, `DEFAULT`.

---

## Common Constraints Example

```sql
CREATE TABLE orders (
    order_id   INT PRIMARY KEY,
    customer   VARCHAR(100) NOT NULL,
    amount     DECIMAL(10,2) CHECK (amount > 0),
    status     VARCHAR(20) DEFAULT 'pending',
    emp_id     INT,
    FOREIGN KEY (emp_id) REFERENCES employees(emp_id)
);
```