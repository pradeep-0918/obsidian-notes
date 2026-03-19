# 28 — COMMIT / ROLLBACK / SAVEPOINT

#intermediate #transactions #interview

---

## 🧠 Introduction

Transactions group SQL operations into an **atomic unit** — either all succeed or all fail. COMMIT saves permanently, ROLLBACK undoes everything.

> Classic example: bank transfer. Debit from A and credit to B must BOTH succeed, or neither should.

---

## 📐 Transaction Syntax

```sql
-- Start a transaction explicitly
START TRANSACTION;
-- or
BEGIN;

-- Make changes
INSERT / UPDATE / DELETE ...

-- Commit (save permanently)
COMMIT;

-- Undo (revert all changes since START TRANSACTION)
ROLLBACK;
```

---

## 🔷 Basic Transaction Example

```sql
-- Bank Transfer: move ₹5000 from account 101 to account 202
START TRANSACTION;

UPDATE accounts SET balance = balance - 5000 WHERE acc_id = 101;
UPDATE accounts SET balance = balance + 5000 WHERE acc_id = 202;

-- Check for errors (application logic)
-- If OK:
COMMIT;
-- If error:
-- ROLLBACK;
```

---

## 🔖 SAVEPOINT — Partial Rollback

```sql
START TRANSACTION;

INSERT INTO orders (user_id, amount) VALUES (1, 500);
SAVEPOINT after_order;   -- bookmark

INSERT INTO order_items (order_id, product_id) VALUES (LAST_INSERT_ID(), 101);
SAVEPOINT after_items;

-- Oops, problem with payment
INSERT INTO payments (order_id, amount) VALUES (LAST_INSERT_ID(), 500);

-- Roll back only to the payment step
ROLLBACK TO SAVEPOINT after_items;

-- Try payment again differently
INSERT INTO payments (order_id, amount, method) VALUES (LAST_INSERT_ID(), 500, 'UPI');

COMMIT;

-- Release savepoint (optional)
RELEASE SAVEPOINT after_order;
```

---

## ⚙️ AUTOCOMMIT Mode

```sql
-- Check current mode
SHOW VARIABLES LIKE 'autocommit';  -- ON by default!

-- With autocommit ON: every DML is auto-committed
UPDATE employees SET salary = 90000 WHERE emp_id = 1;  -- immediately committed!

-- Turn off autocommit for session
SET autocommit = 0;
UPDATE employees SET salary = 90000 WHERE emp_id = 1;
-- Now this is pending until COMMIT or ROLLBACK

-- DDL (CREATE, DROP, TRUNCATE) always auto-commits even if autocommit=0
```

---

## 🌍 Real-World Use Case — E-Commerce Order Processing

```sql
START TRANSACTION;

-- 1. Create the order
INSERT INTO orders (user_id, total_amount, status)
VALUES (42, 15999, 'pending');

SET @order_id = LAST_INSERT_ID();

-- 2. Add order items
INSERT INTO order_items (order_id, product_id, quantity, price)
VALUES (@order_id, 201, 1, 15999);

-- 3. Deduct inventory
UPDATE inventory SET stock = stock - 1 WHERE product_id = 201;

-- 4. Verify stock didn't go negative
IF (SELECT stock FROM inventory WHERE product_id = 201) < 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Insufficient stock';
END IF;

-- 5. Update order status
UPDATE orders SET status = 'confirmed' WHERE order_id = @order_id;

COMMIT;
```

---

## 💼 Interview Questions

1. **What is a transaction?**
   > A logical unit of work comprising one or more SQL operations that must all succeed or all fail — ensuring data consistency.

2. **What is SAVEPOINT?**
   > A named checkpoint within a transaction. You can `ROLLBACK TO SAVEPOINT` to undo work only back to that point, while keeping earlier changes.

3. **What happens to uncommitted transactions if MySQL crashes?**
   > InnoDB's redo/undo logs ensure uncommitted transactions are rolled back on restart (crash recovery via [[30 - ACID Properties]]).

---

## 🔗 Related Topics

- [[30 - ACID Properties]] — Why transactions matter
- [[05 - DDL DML TCL DCL]] — TCL commands
- [[29 - GRANT REVOKE]] — Access control
