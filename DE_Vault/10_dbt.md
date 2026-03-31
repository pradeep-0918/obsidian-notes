# 10 — dbt (Data Build Tool)
#dbt #sql #data-transformation #incremental #snapshots #testing #cicd

**Roadmap Day:** 18 | [[DE_Vault/00_Master_Index]] | Prev → [[09_Snowflake]] | Next → [[11_Delta_Lake_Databricks]]

---

## 🗺️ Topic Map

```
dbt
├── Project Structure (models, sources, tests, macros, snapshots)
├── Materializations (table, view, incremental, ephemeral)
├── ref() and source()
├── Incremental Models (is_incremental, unique_key)
├── Sources & Freshness
├── Tests (built-in + custom)
├── Snapshots (SCD Type 2 automated)
├── Macros (Jinja2)
├── dbt docs & Lineage
└── CI/CD with GitHub Actions
```

---

## 1. Project Structure

```
dbt_project/
├── dbt_project.yml          # project config
├── profiles.yml             # connection config (local only, not in repo)
├── packages.yml             # external packages (dbt-utils, etc.)
├── models/
│   ├── staging/             # 1:1 with source, light cleaning only
│   │   ├── schema.yml       # sources + staging model tests
│   │   └── stg_orders.sql
│   ├── intermediate/        # business logic building blocks
│   │   └── int_order_revenue.sql
│   └── marts/               # final business-facing models
│       ├── core/
│       │   ├── fct_orders.sql
│       │   └── dim_customers.sql
│       └── finance/
│           └── rpt_monthly_revenue.sql
├── macros/
│   └── generate_surrogate_key.sql
├── snapshots/
│   └── customers_snapshot.sql
├── seeds/                   # static CSVs loaded as tables
│   └── country_codes.csv
└── tests/
    └── assert_positive_revenue.sql
```

---

## 2. Materializations

| Type | Creates | Rebuilt on `dbt run` | Best For |
|------|---------|---------------------|---------|
| `view` | SQL view | Yes (always) | Lightweight, rarely queried |
| `table` | Physical table | Yes (drop + recreate) | Frequently queried, complex |
| `incremental` | Physical table | Only new/changed rows | Large tables, append-only |
| `ephemeral` | CTE (no DB object) | N/A | Reusable logic, not queried directly |

```sql
-- Set in model header
{{ config(materialized='incremental') }}

-- Or in dbt_project.yml
models:
  my_project:
    staging:
      +materialized: view
    marts:
      +materialized: table
```

---

## 3. ref() and source()

```sql
-- ref() — reference another model (builds DAG automatically)
SELECT o.*, c.customer_name
FROM {{ ref('stg_orders') }} o
JOIN {{ ref('dim_customers') }} c ON o.customer_id = c.customer_id

-- source() — reference raw/external tables (with freshness tracking)
SELECT * FROM {{ source('raw', 'orders') }}
```

```yaml
# models/staging/schema.yml
sources:
  - name: raw
    database: raw_db
    schema: public
    tables:
      - name: orders
        loaded_at_field: _loaded_at     # for freshness check
        freshness:
          warn_after: {count: 12, period: hour}
          error_after: {count: 24, period: hour}
```

---

## 4. Incremental Models

```sql
-- models/marts/fct_orders.sql
{{ config(
    materialized='incremental',
    unique_key='order_id',
    incremental_strategy='merge',   -- or 'delete+insert', 'append'
    on_schema_change='append_new_columns'
) }}

SELECT
    order_id,
    user_id,
    amount,
    order_date,
    CURRENT_TIMESTAMP AS dbt_updated_at
FROM {{ source('raw', 'orders') }}

{% if is_incremental() %}
  -- Only process rows newer than the latest in the existing table
  WHERE order_date > (SELECT MAX(order_date) FROM {{ this }})
{% endif %}
```

> 💡 **`is_incremental()` block:** Only runs when table already exists AND you're doing an incremental run. On first run (or `--full-refresh`), entire query runs.

> 💡 **`unique_key`:** Required for `merge` strategy. dbt uses this to upsert (update if exists, insert if not). Without it, dbt just appends.

> ⚠️ **Late-arriving data:** If old data arrives, the `MAX(order_date)` filter misses it. Solution: use a lookback window: `WHERE order_date >= DATEADD(days, -3, (SELECT MAX(order_date) FROM {{ this }}))`.

---

## 5. Tests

```yaml
# Built-in generic tests
models:
  - name: fct_orders
    columns:
      - name: order_id
        tests:
          - not_null
          - unique
      - name: user_id
        tests:
          - not_null
          - relationships:
              to: ref('dim_customers')
              field: customer_id
      - name: status
        tests:
          - accepted_values:
              values: ['pending', 'completed', 'cancelled']
```

```sql
-- Custom singular test (tests/assert_no_negative_revenue.sql)
SELECT order_id, revenue
FROM {{ ref('fct_orders') }}
WHERE revenue < 0
-- Test passes if this query returns 0 rows
```

---

## 6. Snapshots (SCD Type 2 automated)

```sql
-- snapshots/customers_snapshot.sql
{% snapshot customers_snapshot %}

{{ config(
    target_schema='snapshots',
    unique_key='customer_id',
    strategy='timestamp',            -- or 'check'
    updated_at='updated_at',
    invalidate_hard_deletes=True
) }}

SELECT * FROM {{ source('raw', 'customers') }}

{% endsnapshot %}
```

> 💡 **`strategy='check'`:** Use when source has no `updated_at` — compares hash of specified columns. `strategy='timestamp'`: more performant, requires reliable updated_at.

---

## 7. Macros (Jinja2)

```sql
-- macros/cents_to_dollars.sql
{% macro cents_to_dollars(column_name) %}
    ({{ column_name }} / 100.0)::NUMERIC(10, 2)
{% endmacro %}

-- Usage in model
SELECT {{ cents_to_dollars('amount_cents') }} AS amount_usd FROM orders

-- Calling dbt packages
SELECT {{ dbt_utils.surrogate_key(['order_id', 'line_item_id']) }} AS sk
FROM order_items
```

---

## 8. CI/CD with GitHub Actions

```yaml
# .github/workflows/dbt_ci.yml
name: dbt CI
on:
  pull_request:
    paths: ['dbt/**']

jobs:
  dbt-ci:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with: {python-version: '3.11'}
      - run: pip install dbt-snowflake
      - run: dbt deps
        working-directory: dbt/
      - run: dbt compile --target ci
      - run: dbt test --select state:modified+ --target ci
        # Only test modified models and their downstream
      - run: dbt run --select state:modified+ --target ci
```

---

## 🃏 Interview Cheatsheet
- **`ref()` vs `source()`?** ref() = reference a dbt model (auto DAG). source() = reference raw external table with freshness tracking.
- **`is_incremental()`?** True when table exists and running incrementally. Use to filter to new rows only.
- **Snapshot strategy 'check'?** Compares hash of specified columns; triggers new row if any column changes. Use when no `updated_at`.
- **Ephemeral materialization?** Becomes a CTE in downstream models. No DB object created. Can't query directly.
- **`dbt build`?** Runs models + tests + snapshots + seeds in DAG order. Use instead of `dbt run` + `dbt test` separately.
