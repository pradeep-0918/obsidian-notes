# 32 — Slowly Changing Dimensions (SCD)

#advanced #dataengineering #interview

---

## 🧠 Introduction

In Data Warehousing, **Slowly Changing Dimensions (SCD)** handle how to track changes to dimension data (like customer address, employee department) over time. This is a core ETL/Data Engineering concept.

> The question: "If a customer moved from Chennai to Mumbai, should their past orders show Chennai or Mumbai?" — SCD types answer this.

---

## 🏗️ Context: Star Schema

```
Dimension Tables (DIM):     Fact Tables (FACT):
dim_customer                fact_orders
dim_product                 fact_inventory
dim_date                    fact_transactions
dim_location
```

Dimension tables contain descriptive attributes. These attributes change over time — that's the "slowly changing" part.

---

## 📊 SCD Types Overview

| Type | Strategy | History? | Storage |
|------|----------|----------|---------|
| Type 0 | Never change | No | Minimal |
| Type 1 | Overwrite | No | Minimal |
| Type 2 | Add new row | Yes (full) | Grows over time |
| Type 3 | Add column | Partial (prev only) | Moderate |
| Type 4 | History table | Yes (separate) | Separate table |
| Type 6 | Hybrid (1+2+3) | Yes (full) | Most complex |

---

## 🔷 SCD Type 1 — Overwrite (No History)

Simply update the existing record. **No history preserved.**

```sql
-- Customer moved from Chennai to Mumbai
UPDATE dim_customer
SET city = 'Mumbai', updated_at = NOW()
WHERE customer_id = 1001;

-- All historical orders now show Mumbai (past is "rewritten")
-- Use when: historical accuracy for this field doesn't matter (e.g., name correction)
```

---

## 🔶 SCD Type 2 — New Row (Full History)

Add a new row for each change. **Full history preserved.**

```sql
-- SCD Type 2 customer dimension table
CREATE TABLE dim_customer (
    surrogate_key   INT PRIMARY KEY AUTO_INCREMENT,  -- warehouse PK
    customer_id     INT,                             -- business/natural key
    name            VARCHAR(100),
    city            VARCHAR(50),
    loyalty_tier    VARCHAR(20),
    -- SCD2 control columns
    effective_date  DATE NOT NULL,
    expiry_date     DATE,        -- NULL means current record
    is_current      BOOLEAN DEFAULT TRUE,
    -- audit
    created_at      DATETIME DEFAULT NOW()
);

-- Initial load
INSERT INTO dim_customer (customer_id, name, city, loyalty_tier, effective_date, expiry_date, is_current)
VALUES (1001, 'Priya Sharma', 'Chennai', 'Gold', '2022-01-01', NULL, TRUE);

-- Customer moves to Mumbai (SCD2 update process):
-- Step 1: Expire the old record
UPDATE dim_customer
SET expiry_date = '2024-07-14',
    is_current  = FALSE
WHERE customer_id = 1001 AND is_current = TRUE;

-- Step 2: Insert new current record
INSERT INTO dim_customer (customer_id, name, city, loyalty_tier, effective_date, expiry_date, is_current)
VALUES (1001, 'Priya Sharma', 'Mumbai', 'Gold', '2024-07-15', NULL, TRUE);

-- Now query: orders before July 15 show Chennai, after show Mumbai
SELECT
    o.order_id,
    o.order_date,
    c.city AS customer_city_at_order_time
FROM fact_orders o
JOIN dim_customer c
  ON o.customer_id = c.customer_id
  AND o.order_date BETWEEN c.effective_date AND COALESCE(c.expiry_date, '9999-12-31');
```

---

## 🔵 SCD Type 3 — Add Column (Limited History)

Add a column for the previous value. Only one version of history.

```sql
CREATE TABLE dim_customer (
    customer_id      INT PRIMARY KEY,
    name             VARCHAR(100),
    current_city     VARCHAR(50),    -- current value
    previous_city    VARCHAR(50),    -- one historical value
    city_changed_at  DATE
);

-- When customer moves:
UPDATE dim_customer
SET previous_city   = current_city,
    current_city    = 'Mumbai',
    city_changed_at = CURDATE()
WHERE customer_id = 1001;

-- Can only see current + ONE previous — limited history
```

---

## 🟡 SCD Type 4 — History Table

Keep main table current; separate history table for all changes.

```sql
-- Current table (always latest values)
CREATE TABLE dim_customer (
    customer_id INT PRIMARY KEY,
    name        VARCHAR(100),
    city        VARCHAR(50),
    tier        VARCHAR(20),
    updated_at  DATETIME
);

-- History table (all versions)
CREATE TABLE dim_customer_history (
    history_id   INT PRIMARY KEY AUTO_INCREMENT,
    customer_id  INT,
    name         VARCHAR(100),
    city         VARCHAR(50),
    tier         VARCHAR(20),
    valid_from   DATETIME,
    valid_to     DATETIME,
    changed_by   VARCHAR(50)
);

-- Trigger or ETL to auto-populate history on changes
```

---

## 🔴 SCD Type 6 — Hybrid (1+2+3)

Combines Type 1 (overwrite non-historical fields), Type 2 (new row for changes), and Type 3 (current + previous columns).

```sql
CREATE TABLE dim_customer (
    surrogate_key    INT PRIMARY KEY AUTO_INCREMENT,
    customer_id      INT,
    name             VARCHAR(100),
    current_city     VARCHAR(50),     -- Type 1: always current city
    historical_city  VARCHAR(50),     -- Type 3: city at time of this record
    previous_city    VARCHAR(50),     -- Type 3: previous city
    effective_date   DATE,            -- Type 2: when this row became valid
    expiry_date      DATE,            -- Type 2: when this row expired
    is_current       BOOLEAN
);
```

---

## 🌍 Real-World Use Case — SCD2 ETL Pipeline

```python
# Python ETL pseudo-code for SCD2 load
def process_scd2(staging_df, dim_table):
    for row in staging_df:
        existing = query(f"""
            SELECT * FROM {dim_table}
            WHERE customer_id = {row.customer_id} AND is_current = TRUE
        """)
        
        if not existing:
            # New customer - insert
            insert_new_record(row, effective_date=TODAY)
        
        elif has_changes(existing, row):
            # Changed data - expire old, insert new
            expire_record(existing.surrogate_key, expiry_date=YESTERDAY)
            insert_new_record(row, effective_date=TODAY)
        
        else:
            # No change - skip
            pass
```

---

## 💼 Interview Questions

1. **What is a Slowly Changing Dimension?**
   > A dimension table whose attributes change slowly over time (not every transaction). SCD strategies define how to handle these changes in the data warehouse.

2. **What is the difference between SCD Type 1 and Type 2?**
   > Type 1 overwrites old values — no history. Type 2 adds a new row for each change — full history preserved via effective/expiry dates and is_current flag.

3. **When would you use SCD Type 2 over Type 1?**
   > Use Type 2 when you need historical accuracy — "What was the customer's city when they placed this order?" vs Type 1 when history doesn't matter (correcting a typo).

4. **What are the control columns in a Type 2 dimension?**
   > Surrogate key (warehouse PK), effective_date (when this version started), expiry_date (when it ended, NULL = current), is_current flag.

5. **Scenario:** *"Design SCD2 for an employee who moved departments."*
   > Add surrogate_key, effective_date, expiry_date, is_current. On department change: expire old row, insert new row with new dept. Fact tables join on employee_id and event_date BETWEEN effective_date AND COALESCE(expiry_date, '9999-12-31').

---

## 🔗 Related Topics

- [[31 - Normalization]] — Schema design principles
- [[22 - Joins]] — Joining facts to SCD2 dimensions
- [[18 - Date and Time]] — Date range logic in SCD2
- [[33 - Python JDBC Connectivity]] — ETL pipeline implementation
- [[34 - Data Engineering Projects]] — Full SCD2 project
