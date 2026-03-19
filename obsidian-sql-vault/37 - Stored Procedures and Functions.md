# 37 — Stored Procedures & Functions

#advanced #interview #performance

---

## 🧠 Introduction

**Stored Procedures** are named SQL code blocks stored in the database and executed by name. **Functions** are similar but always return a value and can be used inside SQL expressions.

> Think of them as "reusable SQL programs" — reduce network round-trips, enforce logic at the database layer, and improve performance.

---

## 🔷 Stored Procedure vs Function

| Feature | Stored Procedure | Function |
|---------|-----------------|---------|
| Returns | Optional (OUT params) | Always one value |
| Use in SELECT | ❌ No | ✅ Yes |
| Can modify DB | ✅ Yes | ⚠️ Deterministic only |
| CALL syntax | `CALL proc_name()` | `SELECT func_name()` |
| Transactions | ✅ Full support | Limited |

---

## 🔧 Creating Stored Procedures

```sql
-- Always change delimiter first!
DELIMITER //

CREATE PROCEDURE GetEmployeesByDept(
    IN dept_name VARCHAR(50)    -- IN = input param
)
BEGIN
    SELECT emp_id, name, salary
    FROM employees
    WHERE dept = dept_name
    ORDER BY salary DESC;
END //

DELIMITER ;

-- Call it
CALL GetEmployeesByDept('Engineering');
```

### IN / OUT / INOUT Parameters

```sql
DELIMITER //

CREATE PROCEDURE GetDeptStats(
    IN  dept_name  VARCHAR(50),   -- input
    OUT emp_count  INT,           -- output
    OUT avg_salary DECIMAL(10,2)  -- output
)
BEGIN
    SELECT COUNT(*), AVG(salary)
    INTO emp_count, avg_salary
    FROM employees
    WHERE dept = dept_name;
END //

DELIMITER ;

-- Call with output variables
CALL GetDeptStats('Engineering', @count, @avg);
SELECT @count AS headcount, @avg AS avg_salary;
```

---

## 🔁 Control Flow in Procedures

### IF / ELSEIF / ELSE

```sql
DELIMITER //

CREATE PROCEDURE ClassifyEmployee(IN emp_id INT)
BEGIN
    DECLARE emp_salary DECIMAL(10,2);
    
    SELECT salary INTO emp_salary FROM employees WHERE emp_id = emp_id;
    
    IF emp_salary >= 100000 THEN
        SELECT 'Senior' AS level;
    ELSEIF emp_salary >= 60000 THEN
        SELECT 'Mid-Level' AS level;
    ELSE
        SELECT 'Junior' AS level;
    END IF;
END //

DELIMITER ;
```

### WHILE Loop

```sql
DELIMITER //

CREATE PROCEDURE PopulateDates(IN start_date DATE, IN end_date DATE)
BEGIN
    DECLARE current_date DATE;
    SET current_date = start_date;
    
    WHILE current_date <= end_date DO
        INSERT INTO dim_date (full_date, year, month_num, day_of_week)
        VALUES (current_date, YEAR(current_date), MONTH(current_date), WEEKDAY(current_date));
        
        SET current_date = DATE_ADD(current_date, INTERVAL 1 DAY);
    END WHILE;
END //

DELIMITER ;

CALL PopulateDates('2024-01-01', '2024-12-31');
```

### CURSOR — Row-by-Row Processing

```sql
DELIMITER //

CREATE PROCEDURE UpdateBonuses()
BEGIN
    DECLARE done     INT DEFAULT FALSE;
    DECLARE v_emp_id INT;
    DECLARE v_salary DECIMAL(10,2);
    
    -- Declare cursor
    DECLARE emp_cursor CURSOR FOR
        SELECT emp_id, salary FROM employees WHERE dept = 'Sales';
    
    -- Handler for end of cursor
    DECLARE CONTINUE HANDLER FOR NOT FOUND SET done = TRUE;
    
    OPEN emp_cursor;
    
    read_loop: LOOP
        FETCH emp_cursor INTO v_emp_id, v_salary;
        IF done THEN LEAVE read_loop; END IF;
        
        -- Update bonus based on salary tier
        UPDATE employees
        SET bonus = CASE
            WHEN v_salary > 80000 THEN v_salary * 0.15
            ELSE v_salary * 0.10
        END
        WHERE emp_id = v_emp_id;
    END LOOP;
    
    CLOSE emp_cursor;
END //

DELIMITER ;
```

> ⚠️ Cursors are slow for large datasets. Prefer set-based SQL (UPDATE with CASE) over cursors whenever possible.

---

## 🔵 Stored Functions

```sql
DELIMITER //

-- Function: calculate age in years
CREATE FUNCTION GetAge(birth_date DATE)
RETURNS INT
DETERMINISTIC
BEGIN
    RETURN TIMESTAMPDIFF(YEAR, birth_date, CURDATE());
END //

-- Function: format Indian currency
CREATE FUNCTION FormatINR(amount DECIMAL(15,2))
RETURNS VARCHAR(30)
DETERMINISTIC
BEGIN
    RETURN CONCAT('₹', FORMAT(amount, 2));
END //

DELIMITER ;

-- Use in queries (just like built-in functions!)
SELECT name, GetAge(birth_date) AS age FROM employees WHERE GetAge(birth_date) > 30;
SELECT name, FormatINR(salary) AS salary_inr FROM employees;
```

---

## ⚡ Triggers

Triggers automatically execute SQL when INSERT/UPDATE/DELETE occurs.

```sql
DELIMITER //

-- Audit log trigger: log every salary change
CREATE TRIGGER trg_salary_audit
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
    IF OLD.salary != NEW.salary THEN
        INSERT INTO salary_audit_log (emp_id, old_salary, new_salary, changed_at, changed_by)
        VALUES (NEW.emp_id, OLD.salary, NEW.salary, NOW(), USER());
    END IF;
END //

-- Prevent negative balance (BEFORE trigger)
CREATE TRIGGER trg_prevent_negative_balance
BEFORE UPDATE ON accounts
FOR EACH ROW
BEGIN
    IF NEW.balance < 0 THEN
        SIGNAL SQLSTATE '45000'
            SET MESSAGE_TEXT = 'Balance cannot be negative';
    END IF;
END //

DELIMITER ;
```

---

## 🔧 Manage Procedures

```sql
-- List all stored procedures
SHOW PROCEDURE STATUS WHERE Db = 'mydb';

-- View procedure definition
SHOW CREATE PROCEDURE GetEmployeesByDept;

-- Drop
DROP PROCEDURE IF EXISTS GetEmployeesByDept;
DROP FUNCTION  IF EXISTS GetAge;
DROP TRIGGER   IF EXISTS trg_salary_audit;
```

---

## 🌍 Real-World Use Case — ETL Procedure

```sql
DELIMITER //

CREATE PROCEDURE RunDailyETL(IN run_date DATE)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        INSERT INTO etl_logs (run_date, status, message)
        VALUES (run_date, 'FAILED', 'SQL Exception occurred');
    END;
    
    START TRANSACTION;
    
    -- Step 1: Load staging
    INSERT INTO orders_staging
    SELECT * FROM orders_raw WHERE DATE(created_at) = run_date;
    
    -- Step 2: Clean data
    DELETE FROM orders_staging WHERE amount <= 0 OR user_id IS NULL;
    
    -- Step 3: Load to warehouse
    INSERT INTO fact_orders
    SELECT * FROM orders_staging;
    
    -- Step 4: Log success
    INSERT INTO etl_logs (run_date, status, rows_loaded)
    SELECT run_date, 'SUCCESS', COUNT(*) FROM orders_staging;
    
    COMMIT;
    
    -- Cleanup
    TRUNCATE TABLE orders_staging;
END //

DELIMITER ;

-- Run daily via scheduled event or cron
CALL RunDailyETL(CURDATE() - INTERVAL 1 DAY);
```

---

## 💼 Interview Questions

1. **What is the difference between a stored procedure and a function?**
   > Procedures can return multiple values via OUT params and don't need to return a value. Functions must return exactly one value and can be used inside SQL expressions. Procedures can use transactions freely; functions are more restricted.

2. **When would you use a stored procedure over application-side SQL?**
   > When logic must be centralized (multiple apps use the same DB), for performance (reduce round-trips), for security (grant EXECUTE instead of table access), or for complex multi-step transactional logic.

3. **What is a trigger? Name two use cases.**
   > A trigger is code that auto-executes on INSERT/UPDATE/DELETE. Use cases: audit logging, enforcing business rules (prevent negative balance), auto-updating derived fields.

4. **Why should you avoid cursors for large datasets?**
   > Cursors process row-by-row (procedural) — extremely slow for large tables. Set-based SQL (UPDATE/INSERT with expressions) is orders of magnitude faster.

5. **Scenario:** *"You need to apply different bonus rates to 50,000 employees based on complex rules. Procedure or query?"*
   > Single UPDATE with CASE WHEN — set-based, fastest. Only use a procedure if there's complex conditional logic that can't be expressed in a single SQL statement.

---

## 🔗 Related Topics

- [[05 - DDL DML TCL DCL]] — Procedures are part of the DDL category
- [[28 - COMMIT ROLLBACK]] — Transactions inside procedures
- [[15 - CASE WHEN]] — Used heavily inside procedures
- [[26 - EXPLAIN ANALYZE]] — Profile procedure performance
- [[29 - GRANT REVOKE]] — GRANT EXECUTE ON PROCEDURE
