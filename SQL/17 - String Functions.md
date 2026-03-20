# 17 — String Functions

#intermediate #interview

---

## 🧠 Introduction

String functions let you **manipulate text data** in queries — essential for data cleaning, ETL pipelines, and report formatting. MySQL has a rich set of built-in string functions.

---

## 📏 Length & Case

```sql
-- String length (bytes)
SELECT LENGTH('Hello World');  -- 11
SELECT LENGTH(name) FROM users;

-- Character length (multi-byte safe)
SELECT CHAR_LENGTH('café');   -- 4 (correct for Unicode)

-- Case conversion
SELECT UPPER('hello');       -- HELLO
SELECT LOWER('HELLO');       -- hello
SELECT UPPER(name) FROM users WHERE city = 'Chennai';
```

---

## ✂️ Trimming & Padding

```sql
-- Remove whitespace
SELECT TRIM('  hello  ');        -- 'hello'
SELECT LTRIM('  hello  ');       -- 'hello  '
SELECT RTRIM('  hello  ');       -- '  hello'

-- Remove specific characters
SELECT TRIM(BOTH '.' FROM '...hello...');  -- 'hello'

-- Padding
SELECT LPAD('42', 6, '0');   -- '000042' (left-pad with zeros)
SELECT RPAD('SQL', 10, '-'); -- 'SQL-------'
```

---

## 🔗 Concatenation & Substring

```sql
-- CONCAT: join strings
SELECT CONCAT(first_name, ' ', last_name) AS full_name FROM employees;
SELECT CONCAT('Hello', ', ', 'World!');  -- 'Hello, World!'

-- CONCAT_WS: with separator (skips NULLs!)
SELECT CONCAT_WS(', ', city, state, country) AS address FROM users;
-- If state is NULL, result: 'Chennai, India' (not 'Chennai, , India')

-- SUBSTRING (substr)
SELECT SUBSTRING('Hello World', 1, 5);  -- 'Hello'
SELECT SUBSTRING('Hello World', 7);     -- 'World'
SELECT SUBSTR(email, 1, LOCATE('@', email)-1) AS username FROM users;

-- LEFT / RIGHT
SELECT LEFT('Hello World', 5);   -- 'Hello'
SELECT RIGHT('Hello World', 5);  -- 'World'
```

---

## 🔍 Search & Position

```sql
-- LOCATE / INSTR: find position of substring
SELECT LOCATE('@', 'user@email.com');   -- 5
SELECT INSTR('user@email.com', '@');    -- 5 (same result)

-- Check if string contains another
SELECT * FROM products WHERE LOCATE('iPhone', name) > 0;

-- FIND_IN_SET: search in comma-separated list
SELECT FIND_IN_SET('banana', 'apple,banana,cherry');  -- 2

-- POSITION (alias for LOCATE)
SELECT POSITION('@' IN 'user@email.com');  -- 5
```

---

## 🔄 Replace & Reverse

```sql
-- REPLACE: replace all occurrences
SELECT REPLACE('Hello World', 'World', 'SQL');  -- 'Hello SQL'

-- Clean phone numbers
UPDATE users SET phone = REPLACE(REPLACE(phone, '-', ''), ' ', '');

-- REVERSE
SELECT REVERSE('Hello');  -- 'olleH'

-- REPEAT
SELECT REPEAT('*', 5);  -- '*****'
```

---

## 🔢 Number Formatting

```sql
-- FORMAT: add commas and decimals
SELECT FORMAT(1234567.89, 2);   -- '1,234,567.89' (as string)
SELECT FORMAT(salary, 0) FROM employees;  -- '75,000'
```

---

## 🌍 Real-World Use Case — Data Cleaning Pipeline

```sql
-- Clean and standardize customer data from raw import
SELECT
    TRIM(UPPER(name))                                    AS clean_name,
    LOWER(TRIM(email))                                   AS clean_email,
    REPLACE(REPLACE(REPLACE(phone, '-', ''), ' ', ''), '(', '') AS clean_phone,
    CONCAT_WS(', ', TRIM(address), TRIM(city), TRIM(state)) AS full_address,
    -- Extract username from email
    SUBSTRING(email, 1, LOCATE('@', email) - 1)          AS username,
    -- Extract domain from email
    SUBSTRING(email, LOCATE('@', email) + 1)             AS email_domain
FROM customers_raw
WHERE LENGTH(TRIM(name)) > 0
  AND LOCATE('@', email) > 0;

-- Find duplicate emails (case-insensitive)
SELECT LOWER(TRIM(email)) AS email, COUNT(*) AS cnt
FROM users
GROUP BY LOWER(TRIM(email))
HAVING cnt > 1;
```

---

## 💼 Interview Questions

1. **What is the difference between LENGTH and CHAR_LENGTH?**
   > LENGTH returns byte count (multi-byte chars count as more). CHAR_LENGTH returns character count — safer for Unicode/UTF-8 data.

2. **What does CONCAT_WS do differently from CONCAT?**
   > CONCAT_WS uses a separator and automatically skips NULL values. CONCAT with NULL returns NULL.

3. **How do you extract the domain from an email address?**
   ```sql
   SELECT SUBSTRING(email, LOCATE('@', email) + 1) AS domain FROM users;
   ```

4. **Scenario:** *"Your CSV import has phone numbers in formats like '(044) 123-4567', '+91-044-123456', '044 123 4567'. How do you standardize?"*
   ```sql
   UPDATE contacts SET phone = 
       REGEXP_REPLACE(phone, '[^0-9]', '');  -- keep only digits
   ```

---

## ⚠️ Common Mistakes

- Using `CONCAT` with NULL — result is NULL! Use `CONCAT_WS` or `COALESCE`
- Wrapping indexed columns in functions like `UPPER(email)` — breaks index usage
- Using `LENGTH` for multi-byte character counts — use `CHAR_LENGTH` for text

---

## 🔗 Related Topics

- [[16 - NULL Handling]] — CONCAT with NULLs
- [[19 - Regex]] — Pattern-based string filtering
- [[09 - WHERE Clause]] — String comparisons in WHERE
- [[25 - Indexes]] — Function-based index workarounds
