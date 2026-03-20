# 19 — Regex in MySQL

#intermediate #advanced

---

## 🧠 Introduction

Regular expressions (Regex) allow **powerful pattern matching** beyond what LIKE can do. MySQL 8.0+ supports full PCRE-compatible regex via `REGEXP` / `RLIKE` and `REGEXP_REPLACE`, `REGEXP_SUBSTR`.

---

## 📐 Basic Syntax

```sql
-- REGEXP / RLIKE (case-insensitive by default)
SELECT * FROM table WHERE column REGEXP 'pattern';
SELECT * FROM table WHERE column RLIKE 'pattern';  -- same thing

-- NOT REGEXP
SELECT * FROM table WHERE column NOT REGEXP 'pattern';
```

---

## 🔣 Common Regex Patterns

| Pattern | Meaning | Example |
|---------|---------|---------|
| `.` | Any single character | `a.c` matches abc, aXc |
| `*` | Zero or more of preceding | `ab*c` matches ac, abc, abbc |
| `+` | One or more of preceding | `ab+c` matches abc, abbc |
| `?` | Zero or one of preceding | `colou?r` matches color, colour |
| `^` | Start of string | `^A` starts with A |
| `$` | End of string | `com$` ends with com |
| `[abc]` | Any of a, b, c | `[aeiou]` any vowel |
| `[^abc]` | Not a, b, or c | `[^0-9]` non-digit |
| `[a-z]` | Range | `[0-9]` any digit |
| `{n}` | Exactly n times | `[0-9]{10}` 10 digits |
| `{n,m}` | Between n and m times | `[a-z]{2,5}` |
| `\d` | Digit (MySQL 8.0+) | `\d{3}` three digits |
| `\w` | Word character | `\w+` one or more |
| `\s` | Whitespace | |
| `|` | OR | `cat|dog` |

---

## 🔍 REGEXP in WHERE

```sql
-- Valid Indian mobile numbers (10 digits, starts with 6-9)
SELECT * FROM users
WHERE phone REGEXP '^[6-9][0-9]{9}$';

-- Valid email format (basic)
SELECT * FROM users
WHERE email REGEXP '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$';

-- Names starting with vowel
SELECT * FROM employees
WHERE name REGEXP '^[AEIOUaeiou]';

-- Product codes: 2 letters + 4 digits
SELECT * FROM products
WHERE sku REGEXP '^[A-Z]{2}[0-9]{4}$';

-- Find records with special characters
SELECT * FROM products
WHERE name REGEXP '[^A-Za-z0-9 ]';

-- Strings containing only digits
SELECT * FROM contacts
WHERE phone REGEXP '^[0-9]+$';

-- Find records with multiple spaces
SELECT * FROM customers
WHERE address REGEXP '  +';
```

---

## 🔄 REGEXP_REPLACE (MySQL 8.0+)

```sql
-- Remove all non-digit characters from phone
SELECT REGEXP_REPLACE(phone, '[^0-9]', '') AS clean_phone
FROM users;

-- Remove extra spaces
SELECT REGEXP_REPLACE(name, ' +', ' ') AS clean_name
FROM customers;

-- Mask last 4 digits of credit card
SELECT REGEXP_REPLACE(card_number, '[0-9](?=[0-9]{4})', '*') AS masked
FROM payments;

-- Standardize date format
SELECT REGEXP_REPLACE('15/07/2024', '/', '-') AS std_date;

-- UPDATE with REGEXP_REPLACE
UPDATE customers
SET phone = REGEXP_REPLACE(phone, '[^0-9]', '')
WHERE phone REGEXP '[^0-9]';
```

---

## 🔍 REGEXP_SUBSTR (MySQL 8.0+)

```sql
-- Extract digits from string
SELECT REGEXP_SUBSTR('Order-12345-ABC', '[0-9]+');  -- '12345'

-- Extract email domain
SELECT REGEXP_SUBSTR(email, '@(.+)$') AS domain FROM users;

-- Extract first word
SELECT REGEXP_SUBSTR(full_name, '^\\w+') AS first_name FROM users;
```

---

## 🌍 Real-World Use Case — Data Validation Pipeline

```sql
-- Data quality check: find invalid records before loading to warehouse
SELECT
    COUNT(*) AS total,
    SUM(CASE WHEN email NOT REGEXP '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$'
             THEN 1 ELSE 0 END) AS invalid_emails,
    SUM(CASE WHEN phone NOT REGEXP '^[6-9][0-9]{9}$'
             THEN 1 ELSE 0 END) AS invalid_phones,
    SUM(CASE WHEN pan_number NOT REGEXP '^[A-Z]{5}[0-9]{4}[A-Z]$'
             THEN 1 ELSE 0 END) AS invalid_pans
FROM customers_staging;

-- Clean phone numbers in bulk
UPDATE customers_staging
SET phone = REGEXP_REPLACE(phone, '[^0-9]', '')
WHERE phone NOT REGEXP '^[0-9]{10}$';
```

---

## 💼 Interview Questions

1. **What is the difference between LIKE and REGEXP?**
   > LIKE uses simple wildcards (`%` for any chars, `_` for one char). REGEXP uses full pattern matching with character classes, quantifiers, anchors — much more powerful.

2. **How do you validate an email format in MySQL?**
   ```sql
   WHERE email REGEXP '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$'
   ```

3. **Does REGEXP use indexes?**
   > No — REGEXP cannot use B-tree indexes (similar to `LIKE '%pattern%'`). It performs a full table scan.

4. **Scenario:** *"Find all product codes not matching the format 'XX-0000' (2 uppercase letters, dash, 4 digits)."*
   ```sql
   SELECT * FROM products
   WHERE code NOT REGEXP '^[A-Z]{2}-[0-9]{4}$';
   ```

---

## ⚠️ Common Mistakes

- Using REGEXP on indexed columns for filtering — full table scan, slow on large tables
- Forgetting to double-escape backslashes: `\\d` not `\d` in MySQL string literals
- Using REGEXP when LIKE would suffice — LIKE with prefix matching CAN use indexes

---

## 🔗 Related Topics

- [[17 - String Functions]] — REPLACE, LOCATE, SUBSTRING
- [[09 - WHERE Clause]] — Pattern matching in WHERE
- [[25 - Indexes]] — Why REGEXP skips indexes
- [[16 - NULL Handling]] — REGEXP with NULL values
