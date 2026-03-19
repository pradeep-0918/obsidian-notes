# 30 — ACID Properties

#intermediate #interview #dataengineering

---

## 🧠 Introduction

ACID is a set of **four guarantees that ensure database transactions are reliable**. This is one of the most fundamental concepts in database theory and a guaranteed interview question.

> ACID is the reason you can trust your bank statement — even if the server crashed mid-transfer.

---

## 🔤 ACID Explained

### A — Atomicity
**"All or nothing."**

A transaction is treated as a single unit. Either ALL operations succeed, or NONE of them take effect.

```sql
-- Bank transfer: must be atomic
START TRANSACTION;
  UPDATE accounts SET balance = balance - 5000 WHERE acc_id = 101;
  UPDATE accounts SET balance = balance + 5000 WHERE acc_id = 202;
COMMIT;

-- If the second UPDATE fails → ROLLBACK undoes the first UPDATE too
-- Money never disappears!
```

### C — Consistency
**"Database moves from one valid state to another valid state."**

All data integrity rules (constraints, foreign keys, triggers) are enforced. A transaction that would violate constraints is rejected.

```sql
-- Consistency enforced by constraints
ALTER TABLE accounts ADD CONSTRAINT chk_balance CHECK (balance >= 0);

-- This transaction will FAIL (consistency maintained)
START TRANSACTION;
  UPDATE accounts SET balance = balance - 10000 WHERE acc_id = 101;
  -- If balance was 5000, it would go to -5000 → CHECK constraint rejects!
ROLLBACK;  -- or auto-rollback on constraint violation
```

### I — Isolation
**"Concurrent transactions don't interfere with each other."**

Multiple users running queries simultaneously see consistent data. MySQL's InnoDB uses locking and MVCC (Multi-Version Concurrency Control).

**Isolation Levels (weakest → strongest):**

| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|-------|------------|---------------------|--------------|
| `READ UNCOMMITTED` | ✅ Possible | ✅ Possible | ✅ Possible |
| `READ COMMITTED` | ❌ Prevented | ✅ Possible | ✅ Possible |
| `REPEATABLE READ` | ❌ Prevented | ❌ Prevented | ✅ Possible |
| `SERIALIZABLE` | ❌ Prevented | ❌ Prevented | ❌ Prevented |

> MySQL InnoDB default: **REPEATABLE READ**

```sql
-- Check/set isolation level
SHOW VARIABLES LIKE 'transaction_isolation';

SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET GLOBAL  TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

**Read Phenomena Explained:**

```
Dirty Read:           T2 reads T1's uncommitted (possibly rolled-back) data
Non-Repeatable Read:  T2 reads a row twice and gets different values (T1 modified it)
Phantom Read:         T2 reads a range twice and gets different rows (T1 inserted)
```

### D — Durability
**"Committed data survives crashes."**

Once COMMIT is executed, the data is permanent — even if the server crashes immediately after.

```sql
-- After COMMIT, data is written to disk (redo log in InnoDB)
START TRANSACTION;
  INSERT INTO orders VALUES (...);
COMMIT;  -- data is NOW durable — crash won't lose this!
```

InnoDB implements durability via:
- **Redo log (write-ahead log)**: changes written to log before data files
- **Doublewrite buffer**: prevents partial page writes on crash
- **Crash recovery**: on restart, replays redo log to restore committed state

---

## 🆚 ACID vs BASE (NoSQL)

| Property | ACID (RDBMS) | BASE (NoSQL) |
|----------|-------------|--------------|
| Consistency | Strong | Eventually consistent |
| Availability | Sometimes sacrificed | Prioritized |
| Complexity | Higher | Lower |
| Use case | Banking, orders | Social feeds, caches |

> BASE = **B**asically **A**vailable, **S**oft state, **E**ventually consistent

---

## 🌍 Real-World Use Case — Why ACID Matters

```sql
-- Scenario: Two bank tellers serving the same account simultaneously

-- Teller 1 reads balance: ₹10,000
-- Teller 2 reads balance: ₹10,000
-- Teller 1 withdraws ₹7,000 → writes ₹3,000
-- Teller 2 withdraws ₹8,000 → writes ₹2,000 (WRONG! Should fail!)

-- Without ACID: both withdrawals succeed → bank loses ₹5,000
-- With ACID (Isolation): Teller 2's read gets the locked row,
-- waits for Teller 1 to commit, then sees ₹3,000 and correctly fails!
```

---

## 💼 Interview Questions

1. **What does ACID stand for?**
   > Atomicity, Consistency, Isolation, Durability.

2. **What is Atomicity? Give an example.**
   > All operations in a transaction succeed or none do. Example: bank transfer — debit and credit must both succeed or both be rolled back.

3. **What are the four MySQL isolation levels?**
   > READ UNCOMMITTED, READ COMMITTED, REPEATABLE READ (default), SERIALIZABLE.

4. **What is a dirty read?**
   > Reading another transaction's uncommitted changes — if that transaction rolls back, you read data that never "officially" existed.

5. **Scenario:** *"MySQL default isolation level is REPEATABLE READ. A performance team wants to change it to READ COMMITTED. What are the risks?"*
   > Non-repeatable reads become possible — a transaction may see different values for the same row if another transaction commits between reads. Acceptable for analytics; risky for financial transactions.

6. **What is MVCC?**
   > Multi-Version Concurrency Control — InnoDB keeps multiple versions of rows so readers don't block writers and vice versa. Readers see a consistent snapshot of data from when their transaction started.

---

## 🔗 Related Topics

- [[28 - COMMIT ROLLBACK]] — Implementing transactions
- [[30 - ACID Properties]] ↔ [[05 - DDL DML TCL DCL]] — TCL implements atomicity
- [[01 - Introduction]] — ACID is why databases exist over spreadsheets
- [[32 - Slowly Changing Dimensions]] — ACID in data warehouse loads
