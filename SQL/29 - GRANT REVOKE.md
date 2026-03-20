# 29 — GRANT / REVOKE

#intermediate #security #interview

---

## 🧠 Introduction

`GRANT` and `REVOKE` are DCL commands that **manage user permissions** in MySQL. In real-world systems, different users need different levels of access — security through least privilege is essential.

---

## 👤 Create a User First

```sql
-- Create user
CREATE USER 'analyst'@'localhost' IDENTIFIED BY 'StrongPassword123!';
CREATE USER 'etl_job'@'%' IDENTIFIED BY 'ETLpass456!';   -- % = any host
CREATE USER 'reporter'@'10.0.0.%' IDENTIFIED BY 'Rpt789!'; -- specific subnet
```

---

## 🔷 GRANT — Give Permissions

```sql
-- All privileges on everything (admin — use sparingly!)
GRANT ALL PRIVILEGES ON *.* TO 'admin_user'@'localhost' WITH GRANT OPTION;

-- All privileges on specific database
GRANT ALL PRIVILEGES ON ecommerce.* TO 'app_user'@'localhost';

-- Read-only on a database
GRANT SELECT ON ecommerce.* TO 'analyst'@'%';

-- Specific table, specific operations
GRANT SELECT, INSERT ON ecommerce.orders TO 'etl_job'@'%';

-- Multiple specific permissions
GRANT SELECT, INSERT, UPDATE ON ecommerce.orders TO 'app_readonly'@'localhost';

-- Column-level grants (very granular)
GRANT SELECT (order_id, amount, status) ON ecommerce.orders TO 'limited_user'@'%';

-- Apply changes immediately
FLUSH PRIVILEGES;
```

---

## 📋 Common Privilege Types

| Privilege | Description |
|-----------|-------------|
| `SELECT` | Read data |
| `INSERT` | Add rows |
| `UPDATE` | Modify rows |
| `DELETE` | Remove rows |
| `CREATE` | Create tables/databases |
| `DROP` | Drop tables/databases |
| `INDEX` | Create/drop indexes |
| `ALTER` | Modify table structure |
| `EXECUTE` | Run stored procedures |
| `SHOW VIEW` | View CREATE VIEW |
| `TRIGGER` | Create/drop triggers |
| `ALL PRIVILEGES` | Everything |

---

## 🔶 REVOKE — Remove Permissions

```sql
-- Revoke specific privilege
REVOKE INSERT ON ecommerce.orders FROM 'analyst'@'%';

-- Revoke all privileges on a database
REVOKE ALL PRIVILEGES ON ecommerce.* FROM 'old_user'@'localhost';

-- Revoke all privileges and grant option
REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'old_user'@'localhost';
```

---

## 🔍 Check Permissions

```sql
-- Show grants for a user
SHOW GRANTS FOR 'analyst'@'%';

-- Show your own grants
SHOW GRANTS;

-- View all users
SELECT user, host FROM mysql.user;
```

---

## 🗑️ Drop User

```sql
DROP USER 'old_user'@'localhost';
```

---

## 🌍 Real-World Use Case — Role-Based Access

```sql
-- Application user (read/write on specific tables)
CREATE USER 'app_backend'@'10.0.1.%' IDENTIFIED BY 'AppPass!';
GRANT SELECT, INSERT, UPDATE ON ecommerce.users TO 'app_backend'@'10.0.1.%';
GRANT SELECT, INSERT, UPDATE, DELETE ON ecommerce.orders TO 'app_backend'@'10.0.1.%';
GRANT SELECT ON ecommerce.products TO 'app_backend'@'10.0.1.%';

-- BI analyst (read only on reporting view)
CREATE USER 'bi_analyst'@'%' IDENTIFIED BY 'BiAnalyst!';
GRANT SELECT ON ecommerce.vw_sales_dashboard TO 'bi_analyst'@'%';
-- No access to base tables!

-- ETL pipeline (read from source, write to staging)
CREATE USER 'etl_pipeline'@'10.0.2.%' IDENTIFIED BY 'ETLpipe!';
GRANT SELECT ON source_db.* TO 'etl_pipeline'@'10.0.2.%';
GRANT INSERT, TRUNCATE ON staging.* TO 'etl_pipeline'@'10.0.2.%';

FLUSH PRIVILEGES;
```

---

## 💼 Interview Questions

1. **What is the principle of least privilege?**
   > Give users only the minimum permissions needed for their job — reduces risk of accidental or malicious damage.

2. **What does `WITH GRANT OPTION` mean?**
   > The user can grant their own privileges to other users.

3. **How do you audit what permissions a user has?**
   > `SHOW GRANTS FOR 'username'@'host';`

4. **Scenario:** *"An analyst accidentally deleted production data. How do you prevent this?"*
   > REVOKE DELETE privilege from the analyst user. Grant SELECT only. Consider giving access to a view instead of the base table.

---

## 🔗 Related Topics

- [[05 - DDL DML TCL DCL]] — DCL overview
- [[21 - Views]] — Grant on views for data security
- [[30 - ACID Properties]] — Transactions + access control
- [[03 - MySQL Installation Linux]] — Secure installation
