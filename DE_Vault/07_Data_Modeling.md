# 07 — Data Modeling
#data-modeling #star-schema #scd #normalization #data-vault #cdc

**Roadmap Day:** 12 | [[00_Master_Index]] | Prev → [[06_Apache_Airflow]] | Next → [[08_AWS_Data_Services]]

---

## 🗺️ Topic Map

```
Data Modeling
├── Normalization (1NF → 3NF → BCNF)
├── Star Schema (fact, dimension, grain)
├── Snowflake Schema
├── SCD Types (1, 2, 6)
├── Data Vault 2.0 (Hub, Link, Satellite)
├── CDC (Change Data Capture)
├── ETL vs ELT
└── Idempotency Patterns
```

---

## 1. Normalization

| Form | Rule | Violation Example |
|------|------|-----------------|
| **1NF** | Atomic values, no repeating groups | `tags = "python,sql,spark"` |
| **2NF** | 1NF + no partial dependencies (on composite PK) | Non-key col depends on only part of composite PK |
| **3NF** | 2NF + no transitive dependencies | `zip_code → city → state` (city depends on zip, not PK) |
| **BCNF** | Every determinant is a candidate key | Stricter than 3NF |

> 💡 **When to denormalize?** For analytical/OLAP workloads — fewer joins = faster queries. Star schema is intentionally denormalized.

---

## 2. Star Schema Design

```
                    ┌──────────────┐
                    │  dim_date    │
                    │  date_key PK │
                    └──────┬───────┘
                           │
┌──────────────┐    ┌──────▼───────────┐    ┌──────────────┐
│  dim_product │    │   fact_orders    │    │  dim_customer │
│  product_key ├────┤  order_id  PK   ├────┤  customer_key │
│  product_name│    │  product_key FK │    │  customer_name│
│  category    │    │  customer_key FK│    │  region       │
│  price       │    │  date_key FK    │    │  segment      │
└──────────────┘    │  quantity       │    └──────────────┘
                    │  revenue        │
                    │  discount       │
                    └─────────────────┘
```

### Design Rules
- **Grain:** Define FIRST. "One row per order line item" = grain. Every column must be at that grain.
- **Surrogate keys:** Always use integer surrogate keys in fact tables (not natural/business keys).
- **Degenerate dimensions:** Order number in fact table — dimension with no dim table (e.g., invoice #).
- **Junk dimensions:** Combine low-cardinality flags into one dimension table (e.g., is_gift, is_return, payment_type).

---

## 3. Snowflake Schema vs Star Schema

| | Star | Snowflake |
|--|------|----------|
| Dimension storage | Denormalized (wide) | Normalized (multiple tables) |
| Query joins | Fewer | More |
| Query speed | Faster | Slower |
| Storage | More | Less |
| Maintenance | Simpler | Complex |
| BI tool compatibility | Better | OK |

> 💡 **Interview answer:** "Star schema is preferred for analytics — query speed matters more than storage savings. Use snowflake only when dimension tables are huge and storage is a real concern."

---

## 4. SCD Types

### SCD Type 1 — Overwrite
```sql
UPDATE dim_customer SET email = 'new@email.com' WHERE customer_key = 1;
-- No history kept — use for non-critical, self-correcting attributes
```

### SCD Type 2 — Full History (most common)
```sql
-- dim_customer structure
customer_key    BIGINT (surrogate, PK)
customer_id     VARCHAR (natural/business key)
email           VARCHAR
address         VARCHAR
valid_from      DATE
valid_to        DATE        -- 9999-12-31 for current
is_current      BOOLEAN
```

```sql
-- SCD Type 2 MERGE (PostgreSQL)
-- Step 1: Expire changed rows
UPDATE dim_customer
SET valid_to = CURRENT_DATE - 1,
    is_current = FALSE
WHERE customer_id IN (SELECT customer_id FROM staging WHERE changed)
  AND is_current = TRUE;

-- Step 2: Insert new versions
INSERT INTO dim_customer (customer_id, email, address, valid_from, valid_to, is_current)
SELECT customer_id, email, address, CURRENT_DATE, '9999-12-31', TRUE
FROM staging;
```

### SCD Type 6 — Hybrid (Type 1 + 2 + 3)
Adds a `current_value` column that always shows the latest value, alongside the historical row.

> 💡 **When to use each:** Type 1 → corrections/fixes. Type 2 → must track history (billing, compliance). Type 6 → want history + easy current-value access.

---

## 5. CDC — Change Data Capture

| Method | How | Latency | DB Impact |
|--------|-----|---------|----------|
| **Log-based (Debezium)** | Reads DB transaction log (WAL) | Near-real-time | Low |
| **Trigger-based** | DB triggers write to audit table | Low | High (every write) |
| **Timestamp-based** | Poll `updated_at > last_run` | Minutes | Medium |
| **Snapshot diff** | Compare full snapshots | Hours | High |

> 💡 **Log-based CDC is gold:** Debezium tails PostgreSQL WAL or MySQL binlog → publishes to Kafka topic. No load on source DB, real-time, captures deletes (others miss deletes!).

---

## 6. Data Vault 2.0 (Conceptual)

| Object | Purpose | Key Columns |
|--------|---------|-------------|
| **Hub** | Business entity (unique business key) | `hub_key`, `business_key`, `load_date`, `source` |
| **Link** | Relationship between hubs | `link_key`, `hub_key_1`, `hub_key_2`, `load_date` |
| **Satellite** | Descriptive attributes (with history) | `hub_key`, `load_date`, `end_date`, attributes |

> 💡 **When Data Vault?** Enterprise DWH with many sources, heavy audit requirements. Scales well for additions (just add satellites). Harder to query directly → needs a mart layer on top.

---

## 🃏 Interview Cheatsheet
- **Grain?** The level of detail in a fact table row. Must define before designing fact table.
- **SCD Type 2 cols?** `valid_from`, `valid_to` (9999-12-31 for current), `is_current`.
- **ETL vs ELT?** ETL: transform before loading (traditional DWH). ELT: load raw then transform in-warehouse (cloud DWH like Snowflake/BigQuery — cheap storage + compute on demand).
- **Idempotency?** Running pipeline twice = same result. Use upsert/merge or partition overwrite.
- **Log-based CDC advantage?** Captures deletes, low source DB impact, near-real-time.
