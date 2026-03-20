# 05 — DDL, DML, TCL, DCL — SQL Command Categories

#beginner #foundation #interview

---

## 🧠 Introduction

SQL commands are organized into **4 categories** based on what they do. This is one of the **most common interview questions** at every level — know this cold.

> Think of it as: DDL = structure, DML = data, TCL = control, DCL = security.

---

## 📊 The Four Categories

| Category | Full Form | Purpose | Commands |
|----------|-----------|---------|----------|
| **DDL** | Data Definition Language | Define/modify structure | CREATE, ALTER, DROP, TRUNCATE, RENAME |
| **DML** | Data Manipulation Language | Work with data | SELECT, INSERT, UPDATE, DELETE |
| **TCL** | Transaction Control Language | Manage transactions | COMMIT, ROLLBACK, SAVEPOINT |
| **DCL** | Data Control Language | Manage permissions | GRANT, REVOKE |

---

## 🔷 DDL — Data Definition Language

DDL commands define the **structure** of your database. They operate on schemas/tables, not data.

**Key property:** DDL commands in MySQL are **auto-committed** — you cannot roll them back!

```sql
-- CREATE: Define a new table
CREATE TABLE employees (
    emp_id INT PRIMARY KEY AUTO_INCREMENT,
    name   VARCHAR(100) NOT NULL,
    salary DECIMAL(10,2),
    dept   VARCHAR(50)
);

-- ALTER: Modify existing table structure
ALTER TABLE employees ADD COLUMN email VARCHAR(255);
ALTER TABLE employees DROP COLUMN dept;

-- TRUNCATE: Remove ALL data (keeps table structure)
TRUNCATE TABLE employees;

-- DROP: Delete the entire table (structure + data)
DROP TABLE employees;

-- RENAME: Rename a table
RENAME TABLE employees TO staff;
```

> ⚠️ **TRUNCATE vs DELETE:** TRUNCATE is DDL (auto-commit, can't rollback). DELETE is DML (can rollback).

---

## 🔶 DML — Data Manipulation Language

DML commands **work with the data** inside tables.

**Key property:** DML is part of transactions — you CAN rollback DML operations.

```sql
-- SELECT: Read data
SELECT name, salary FROM employees WHERE salary > 50000;

-- INSERT: Add new data
INSERT INTO employees (name, salary, dept)
VALUES ('Priya Sharma', 75000, 'Engineering');

-- UPDATE: Modify existing data
UPDATE employees
SET salary = 80000
WHERE emp_id = 1;

-- DELETE: Remove specific rows
DELETE FROM employees WHERE emp_id = 5;
```

---

## 🔵 TCL — Transaction Control Language

TCL commands **control transactions** — grouping DML operations into atomic units.

```sql
-- Start a transaction
START TRANSACTION;

-- Make changes
UPDATE accounts SET balance = balance - 5000 WHERE acc_id = 101;
UPDATE accounts SET balance = balance + 5000 WHERE acc_id = 202;

-- If all good: save permanently
COMMIT;

-- If something went wrong: undo everything
ROLLBACK;

-- Create a checkpoint within a transaction
SAVEPOINT before_update;
-- ... do work ...
ROLLBACK TO SAVEPOINT before_update;
```

> 💡 See [[28 - COMMIT ROLLBACK]] for deep dive.

---

## 🔴 DCL — Data Control Language

DCL commands **manage user permissions** on database objects.

```sql
-- GRANT: Give privileges
GRANT SELECT, INSERT ON mydb.employees TO 'analyst'@'localhost';

-- REVOKE: Remove privileges
REVOKE INSERT ON mydb.employees FROM 'analyst'@'localhost';
```

> 💡 See [[29 - GRANT REVOKE]] for deep dive.

---

## 🌍 Real-World Use Case

**Banking System:**

```sql
-- DDL: Create the accounts table (done once during setup)
CREATE TABLE accounts (
    acc_id   INT PRIMARY KEY,
    holder   VARCHAR(100),
    balance  DECIMAL(15,2)
);

-- DML: Process a fund transfer
START TRANSACTION;                           -- TCL
  UPDATE accounts SET balance = balance - 1000 WHERE acc_id = 101;  -- DML
  UPDATE accounts SET balance = balance + 1000 WHERE acc_id = 202;  -- DML
COMMIT;                                      -- TCL

-- DCL: Restrict a teller to read-only
GRANT SELECT ON bank.accounts TO 'teller'@'%';  -- DCL
```

---

## 💼 Interview Questions

1. **What are the four categories of SQL commands?**
   > DDL (Data Definition), DML (Data Manipulation), TCL (Transaction Control), DCL (Data Control)

2. **Can you rollback a DDL command?**
   > In MySQL, NO — DDL commands like DROP and TRUNCATE are auto-committed. In some databases like PostgreSQL, DDL can be wrapped in transactions.

3. **What is the difference between TRUNCATE and DELETE?**
   > TRUNCATE is DDL — removes all rows, auto-commits, resets AUTO_INCREMENT, faster. DELETE is DML — can filter rows with WHERE, can be rolled back.

4. **Which SQL category does SELECT belong to?**
   > DML (Data Manipulation Language)

5. **Scenario:** *"A junior developer ran `DROP TABLE orders` on production. Can you recover?"*
   > If there's a recent backup — restore from it. If binary logging is enabled in MySQL, use `mysqlbinlog` to replay transactions up to that point. This is why daily backups and controlled GRANT permissions matter.

---

## 💡 Tips & Best Practices

- Always wrap DML operations that affect multiple rows/tables in a **transaction**
- Never run `DROP` or `TRUNCATE` on production without a backup
- Use `GRANT` minimally — give users only what they need (principle of least privilege)
- Know that in MySQL, `TRUNCATE` resets `AUTO_INCREMENT` counter — `DELETE` does not

---

## ⚠️ Common Mistakes

- Thinking `SELECT` is DDL — it's DML
- Assuming TRUNCATE can be rolled back in MySQL — it cannot
- Using DELETE to clear a full table when TRUNCATE is much faster

---

## 🔗 Related Topics

- [[06 - SQL Basic Commands CRUD]] — Hands-on DML commands
- [[07 - CREATE TABLE Variants]] — Deep dive into DDL
- [[08 - ALTER TABLE]] — Modifying table structure
- [[28 - COMMIT ROLLBACK]] — TCL deep dive
- [[29 - GRANT REVOKE]] — DCL deep dive
- [[30 - ACID Properties]] — Why transactions matter
