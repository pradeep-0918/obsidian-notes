

> [!info] What is DML? DML deals with the **manipulation of data** stored inside database objects. Unlike DDL, DML statements are **not auto-committed** — you can roll back changes using TCL commands.

---

## Tags

#sql #dml #database-basics

---

## Related Notes

- [[DDL - Data Definition Language]]
- [[TCL - Transaction Control Language]]
- [[DCL - Data Control Language]]

---

## Core Commands

|Command|Purpose|
|---|---|
|`SELECT`|Retrieve/query data|
|`INSERT`|Add new rows|
|`UPDATE`|Modify existing rows|
|`DELETE`|Remove rows|
|`MERGE`|Upsert (insert or update)|

---

## SELECT

```sql
-- Basic select
SELECT * FROM employees;

-- Specific columns
SELECT name, department, salary FROM employees;

-- With conditions
SELECT name, salary
FROM employees
WHERE department = 'Engineering'
  AND salary > 60000;

-- Sorting
SELECT name, salary FROM employees ORDER BY salary DESC;

-- Aggregates
SELECT department, COUNT(*) AS headcount, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING COUNT(*) > 5;

-- Joins
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;
```

---

## INSERT

```sql
-- Insert a single row
INSERT INTO employees (emp_id, name, department, salary)
VALUES (101, 'Priya Sharma', 'Engineering', 85000);

-- Insert multiple rows
INSERT INTO employees (emp_id, name, department, salary)
VALUES
    (102, 'Arjun Mehta',   'Marketing',   70000),
    (103, 'Sneha Iyer',    'Engineering', 90000);

-- Insert from another table
INSERT INTO archived_employees
SELECT * FROM employees WHERE joined_on < '2020-01-01';
```

---

## UPDATE

```sql
-- Update a single column
UPDATE employees
SET salary = 95000
WHERE emp_id = 101;

-- Update multiple columns
UPDATE employees
SET salary = salary * 1.10,
    department = 'Senior Engineering'
WHERE department = 'Engineering' AND salary > 80000;
```

> [!warning] Always use WHERE with UPDATE Without `WHERE`, **all rows** in the table will be updated.

---

## DELETE

```sql
-- Delete specific rows
DELETE FROM employees WHERE emp_id = 103;

-- Delete with a condition
DELETE FROM employees WHERE department = 'Marketing';
```

> [!warning] Always use WHERE with DELETE Without `WHERE`, **all rows** are deleted (but the table structure remains — unlike `DROP`).

---

## MERGE (Upsert)

```sql
-- MERGE: update if exists, insert if not
MERGE INTO employees AS target
USING new_hires AS source
ON target.emp_id = source.emp_id
WHEN MATCHED THEN
    UPDATE SET target.salary = source.salary
WHEN NOT MATCHED THEN
    INSERT (emp_id, name, department, salary)
    VALUES (source.emp_id, source.name, source.department, source.salary);
```

---

## Key Concepts

- **Transactional**: DML statements can be wrapped in a transaction and rolled back.
- **WHERE clause**: Always think carefully before omitting it in UPDATE or DELETE.
- **Subqueries**: DML supports subqueries inside SELECT, WHERE, and SET.

---

## SELECT Clause Order (must follow this sequence)

```sql
SELECT   -- columns / expressions
FROM     -- table(s)
JOIN     -- optional joins
WHERE    -- row-level filter
GROUP BY -- grouping
HAVING   -- group-level filter
ORDER BY -- sort
LIMIT    -- row limit
```

---

## Quick Reference: DML vs DDL

|Feature|DML|DDL|
|---|---|---|
|Operates on|Data (rows)|Structure (schema)|
|Auto-commit|❌ No|✅ Yes|
|Rollback|✅ Yes|❌ Usually no|
|Examples|INSERT, UPDATE, DELETE|CREATE, ALTER, DROP|