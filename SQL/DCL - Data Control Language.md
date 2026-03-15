
> [!info] What is DCL? DCL controls **access and permissions** on database objects. It determines **who** can do **what** on which database resources, making it essential for security and multi-user environments.

---

## Tags

#sql #dcl #database-basics #security

---

## Related Notes

- [[DDL - Data Definition Language]]
- [[DML - Data Manipulation Language]]
- [[TCL - Transaction Control Language]]

---

## Core Commands

|Command|Purpose|
|---|---|
|`GRANT`|Give a user/role specific privileges|
|`REVOKE`|Remove previously granted privileges|

---

## GRANT

```sql
-- Grant SELECT on a table to a user
GRANT SELECT ON employees TO 'analyst_user'@'localhost';

-- Grant multiple privileges
GRANT SELECT, INSERT, UPDATE ON employees TO 'hr_user'@'localhost';

-- Grant all privileges on a table
GRANT ALL PRIVILEGES ON employees TO 'admin_user'@'localhost';

-- Grant on an entire database
GRANT ALL PRIVILEGES ON company_db.* TO 'dba_user'@'localhost';

-- Grant with the ability to pass permissions to others
GRANT SELECT ON employees TO 'manager_user'@'localhost' WITH GRANT OPTION;
```

---

## REVOKE

```sql
-- Revoke a specific privilege
REVOKE SELECT ON employees FROM 'analyst_user'@'localhost';

-- Revoke multiple privileges
REVOKE INSERT, UPDATE ON employees FROM 'hr_user'@'localhost';

-- Revoke all privileges
REVOKE ALL PRIVILEGES ON employees FROM 'admin_user'@'localhost';

-- Revoke grant option only (keep other permissions)
REVOKE GRANT OPTION FOR SELECT ON employees FROM 'manager_user'@'localhost';
```

---

## Common Privilege Types

|Privilege|Description|
|---|---|
|`SELECT`|Read data from table/view|
|`INSERT`|Add new rows|
|`UPDATE`|Modify existing rows|
|`DELETE`|Remove rows|
|`CREATE`|Create new DB objects|
|`DROP`|Delete DB objects|
|`ALTER`|Modify DB object structure|
|`INDEX`|Create/drop indexes|
|`EXECUTE`|Run stored procedures/functions|
|`ALL`|All of the above|

---

## Roles (Role-Based Access Control)

```sql
-- Create a role
CREATE ROLE read_only_role;

-- Grant privileges to a role
GRANT SELECT ON company_db.* TO read_only_role;

-- Assign role to a user
GRANT read_only_role TO 'new_analyst'@'localhost';

-- Revoke role from user
REVOKE read_only_role FROM 'new_analyst'@'localhost';
```

---

## Checking Permissions

```sql
-- MySQL: show grants for a user
SHOW GRANTS FOR 'analyst_user'@'localhost';

-- PostgreSQL: list privileges
\dp employees

-- SQL Server: view effective permissions
SELECT * FROM fn_my_permissions('employees', 'OBJECT');
```

---

## Key Concepts

- **Principle of Least Privilege**: Grant users only the minimum permissions they need.
- **WITH GRANT OPTION**: Allows a user to pass their privileges on to others — use cautiously.
- **Roles**: Group permissions into roles and assign roles to users for easier management.
- **Object-level vs DB-level**: Permissions can be scoped to a column, table, schema, or entire database.

---

## DCL vs DDL

|Aspect|DCL|DDL|
|---|---|---|
|Manages|User permissions & access|Database structure/schema|
|Commands|GRANT, REVOKE|CREATE, ALTER, DROP|
|Auto-commit|✅ Yes (in most RDBMS)|✅ Yes|

---

## Security Best Practices

> [!tip] Best Practices
> 
> - Never grant `ALL PRIVILEGES` to application-level users.
> - Use **roles** instead of assigning permissions directly to users.
> - Regularly **audit** user privileges with `SHOW GRANTS`.
> - Revoke `WITH GRANT OPTION` unless absolutely necessary.
> - Use **schema-level** permissions where possible to limit blast radius.