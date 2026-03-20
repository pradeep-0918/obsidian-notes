

> [!info] What is TCL? TCL manages **transactions** in a database. A transaction is a unit of work — a sequence of DML operations that either **all succeed** or **all fail** together, ensuring data integrity.

---

## Tags

#sql #tcl #database-basics #transactions

---

## Related Notes

- [[DDL - Data Definition Language]]
- [[DML - Data Manipulation Language]]
- [[DCL - Data Control Language]]

---

## Core Commands

|Command|Purpose|
|---|---|
|`COMMIT`|Permanently save all changes in transaction|
|`ROLLBACK`|Undo all changes since last commit|
|`SAVEPOINT`|Set a named checkpoint within a transaction|
|`ROLLBACK TO`|Undo back to a specific savepoint|
|`RELEASE SAVEPOINT`|Remove a savepoint|
|`SET TRANSACTION`|Configure transaction properties|

---

## COMMIT

```sql
-- Start making changes
UPDATE accounts SET balance = balance - 5000 WHERE acc_id = 1;
UPDATE accounts SET balance = balance + 5000 WHERE acc_id = 2;

-- Permanently save both changes
COMMIT;
```

---

## ROLLBACK

```sql
UPDATE employees SET salary = 0;  -- Oops!

-- Undo all uncommitted changes
ROLLBACK;
```

> [!tip] `ROLLBACK` only works on **DML** statements (INSERT, UPDATE, DELETE). DDL statements are auto-committed and **cannot** be rolled back in most RDBMS.

---

## SAVEPOINT

```sql
INSERT INTO orders VALUES (1, 'Laptop', 75000);
SAVEPOINT after_laptop;

INSERT INTO orders VALUES (2, 'Phone', 30000);
SAVEPOINT after_phone;

INSERT INTO orders VALUES (3, 'Tablet', 25000);

-- Undo only the tablet insert
ROLLBACK TO after_phone;

-- Commit laptop and phone inserts
COMMIT;
```

---

## RELEASE SAVEPOINT

```sql
SAVEPOINT sp1;

-- Once done, release to free memory
RELEASE SAVEPOINT sp1;
```

---

## SET TRANSACTION

```sql
-- Set isolation level for a transaction
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Read-only transaction
SET TRANSACTION READ ONLY;
```

---

## ACID Properties

> Every transaction in SQL must satisfy ACID:

|Property|Description|
|---|---|
|**Atomicity**|All operations succeed or all fail — no partial updates|
|**Consistency**|DB moves from one valid state to another valid state|
|**Isolation**|Concurrent transactions don't interfere with each other|
|**Durability**|Committed data persists even after system failure|

---

## Transaction Isolation Levels

|Level|Dirty Read|Non-repeatable Read|Phantom Read|
|---|---|---|---|
|READ UNCOMMITTED|✅ Possible|✅ Possible|✅ Possible|
|READ COMMITTED|❌ No|✅ Possible|✅ Possible|
|REPEATABLE READ|❌ No|❌ No|✅ Possible|
|SERIALIZABLE|❌ No|❌ No|❌ No|

---

## Typical Transaction Pattern

```sql
-- Begin (implicit in most DB; explicit in some)
BEGIN TRANSACTION;

  UPDATE accounts SET balance = balance - 1000 WHERE acc_id = 'A';
  UPDATE accounts SET balance = balance + 1000 WHERE acc_id = 'B';

  -- If all good:
  COMMIT;

  -- If something went wrong:
  -- ROLLBACK;
```

---

## Key Concepts

- **Auto-commit mode**: In many clients (e.g., MySQL Workbench), each statement is auto-committed. Disable it to use transactions manually.
- **Implicit vs Explicit transactions**: Some RDBMS start a transaction implicitly on first DML; others require `BEGIN`.
- **Nested transactions**: Not all RDBMS support them; use SAVEPOINTs instead.