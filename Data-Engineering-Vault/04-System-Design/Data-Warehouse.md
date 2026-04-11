# 🏛️ Data Warehouse — System Design Guide
#design #warehouse #dimensional-modeling #star-schema #dwh

> The organized, curated store of business data. Where analysts live. Where decisions are made.

---

## 🧠 The Mental Model

A data warehouse is like a **well-organized library**:
- **Raw operational databases** = Piles of books scattered across departments
- **Data warehouse** = Library with catalog system, consistent organization, searchable
- **Data marts** = Specialized reading rooms for specific departments (Finance, Marketing)

**Why not just query production databases?**
1. Production queries slow down your live application
2. Operational schema is normalized (many JOINs needed for analytics)
3. Historical data often not retained in production
4. No audit trail for how numbers were calculated

---

## 📐 Dimensional Modeling

Created by Ralph Kimball. The most important skill for data warehouse design.

### The Star Schema

```
                     ┌──────────────────────┐
                     │      dim_date         │
                     │──────────────────────│
                     │ date_key        (PK)  │
                     │ date                  │
                     │ day_of_week           │
                     │ week_of_year          │
                     │ month                 │
                     │ month_name            │
                     │ quarter               │
                     │ year                  │
                     │ is_weekend            │
                     │ is_holiday            │
                     │ fiscal_quarter        │
                     └──────────┬───────────┘
                                │
┌──────────────────┐  ┌─────────┴──────────────┐  ┌──────────────────┐
│   dim_product     │  │      fact_orders         │  │  dim_customer     │
│──────────────────│  │────────────────────────-│  │──────────────────│
│ product_key (PK) ├──┤ product_key      (FK)   │  │ customer_key (PK)│
│ product_id       │  │ customer_key     (FK)   ├──┤ customer_id      │
│ product_name     │  │ date_key         (FK)   │  │ customer_name    │
│ category         │  │ store_key        (FK)   │  │ email            │
│ subcategory      │  │ promo_key        (FK)   │  │ segment          │
│ brand            │  │ ── MEASURES ──          │  │ region           │
│ supplier         │  │ quantity                │  │ country          │
│ unit_cost        │  │ unit_price              │  │ city             │
│ price_tier       │  │ discount_amount         │  │ postal_code      │
│ is_active        │  │ gross_revenue           │  │ acquired_date    │
│ eff_from         │  │ net_revenue             │  │ lifetime_value   │
│ eff_to           │  │ cost_of_goods           │  │ is_b2b           │
└──────────────────┘  │ gross_profit            │  └──────────────────┘
                       │ tax_amount              │
                       └─────────────────────────┘
```

### Grain

The grain is the **most atomic level of detail** in a fact table. 
**Define grain before anything else.**

```
Too coarse (monthly sales): Loses daily patterns
Just right (one row per order line item): Maximum flexibility
Too fine (every click in checkout): Too much noise, expensive

Example grains:
- One row per order line item (retail)
- One row per transaction (banking)
- One row per page view (web analytics)
- One row per sensor reading per minute (IoT)
```

---

## 🏗️ Building the Warehouse — SQL Implementation

### Dimension Tables

```sql
-- ── DATE DIMENSION ────────────────────────────────────────────────
-- Generated once, used forever
CREATE TABLE dim_date AS
WITH RECURSIVE date_series AS (
    SELECT DATE '2015-01-01' AS d
    UNION ALL
    SELECT d + INTERVAL '1 day'
    FROM date_series
    WHERE d < DATE '2030-12-31'
)
SELECT
    TO_CHAR(d, 'YYYYMMDD')::INT AS date_key,
    d AS date,
    EXTRACT(DOW FROM d)::INT AS day_of_week,
    TO_CHAR(d, 'Day') AS day_name,
    EXTRACT(WEEK FROM d)::INT AS week_of_year,
    EXTRACT(MONTH FROM d)::INT AS month,
    TO_CHAR(d, 'Month') AS month_name,
    EXTRACT(QUARTER FROM d)::INT AS quarter,
    EXTRACT(YEAR FROM d)::INT AS year,
    CASE WHEN EXTRACT(DOW FROM d) IN (0, 6) THEN TRUE ELSE FALSE END AS is_weekend,
    -- Fiscal year (starting April 1)
    CASE
        WHEN EXTRACT(MONTH FROM d) >= 4 THEN EXTRACT(YEAR FROM d)
        ELSE EXTRACT(YEAR FROM d) - 1
    END AS fiscal_year,
    CASE
        WHEN EXTRACT(MONTH FROM d) IN (4,5,6) THEN 1
        WHEN EXTRACT(MONTH FROM d) IN (7,8,9) THEN 2
        WHEN EXTRACT(MONTH FROM d) IN (10,11,12) THEN 3
        ELSE 4
    END AS fiscal_quarter
FROM date_series;

CREATE UNIQUE INDEX ON dim_date (date_key);
CREATE INDEX ON dim_date (date);
CREATE INDEX ON dim_date (year, month);

-- ── CUSTOMER DIMENSION (SCD Type 2) ──────────────────────────────
CREATE TABLE dim_customer (
    customer_key    BIGSERIAL PRIMARY KEY,
    -- Natural key
    customer_id     BIGINT NOT NULL,
    -- Descriptive attributes
    customer_name   VARCHAR(255),
    email           VARCHAR(255),
    phone           VARCHAR(50),
    segment         VARCHAR(50),    -- e.g., 'Enterprise', 'SMB', 'Consumer'
    region          VARCHAR(50),
    country         VARCHAR(50),
    city            VARCHAR(100),
    -- Derived metrics (snapshot at time of record)
    lifetime_orders INT DEFAULT 0,
    lifetime_value  NUMERIC(14, 2) DEFAULT 0,
    -- SCD Type 2 tracking
    effective_from  DATE NOT NULL,
    effective_to    DATE,           -- NULL = current record
    is_current      BOOLEAN DEFAULT TRUE,
    -- Audit
    row_hash        CHAR(64),       -- SHA256 of key attributes
    created_at      TIMESTAMP DEFAULT NOW(),
    updated_at      TIMESTAMP DEFAULT NOW()
);

CREATE INDEX ON dim_customer (customer_id, is_current);
CREATE INDEX ON dim_customer (customer_id, effective_from, effective_to);

-- ── PRODUCT DIMENSION ─────────────────────────────────────────────
CREATE TABLE dim_product (
    product_key     BIGSERIAL PRIMARY KEY,
    product_id      VARCHAR(50) NOT NULL,
    sku             VARCHAR(100),
    product_name    VARCHAR(500),
    description     TEXT,
    category_l1     VARCHAR(100),   -- Electronics
    category_l2     VARCHAR(100),   -- Phones & Tablets
    category_l3     VARCHAR(100),   -- Smartphones
    brand           VARCHAR(100),
    supplier_name   VARCHAR(200),
    -- Pricing (point-in-time)
    list_price      NUMERIC(12, 2),
    cost            NUMERIC(12, 2),
    margin_pct      NUMERIC(6, 4),
    -- Status
    is_active       BOOLEAN DEFAULT TRUE,
    launched_date   DATE,
    discontinued_date DATE,
    -- SCD
    effective_from  DATE NOT NULL,
    effective_to    DATE,
    is_current      BOOLEAN DEFAULT TRUE,
    row_hash        CHAR(64)
);
```

### Fact Tables

```sql
-- ── FACT_ORDERS (Transaction fact) ───────────────────────────────
CREATE TABLE fact_orders (
    -- Surrogate key
    order_line_key  BIGSERIAL PRIMARY KEY,
    -- Dimension foreign keys
    date_key        INT REFERENCES dim_date(date_key),
    customer_key    BIGINT REFERENCES dim_customer(customer_key),
    product_key     BIGINT REFERENCES dim_product(product_key),
    -- Degenerate dimensions (no separate table needed)
    order_id        BIGINT NOT NULL,
    order_line_id   BIGINT NOT NULL,
    -- Measures (additive)
    quantity        INT NOT NULL DEFAULT 0,
    unit_price      NUMERIC(12, 2),
    discount_pct    NUMERIC(6, 4) DEFAULT 0,
    discount_amount NUMERIC(12, 2) DEFAULT 0,
    gross_revenue   NUMERIC(14, 2),
    net_revenue     NUMERIC(14, 2),
    cost_of_goods   NUMERIC(12, 2),
    gross_profit    NUMERIC(14, 2),
    tax_amount      NUMERIC(12, 2) DEFAULT 0,
    -- Semi-additive measures (can't SUM across dates)
    is_first_order  BOOLEAN,        -- For new customer analysis
    -- Audit
    loaded_at       TIMESTAMP DEFAULT NOW(),
    source_system   VARCHAR(50)
);

-- Indexes for common query patterns
CREATE INDEX ON fact_orders (date_key, customer_key);
CREATE INDEX ON fact_orders (customer_key, date_key);
CREATE INDEX ON fact_orders (product_key, date_key);
CREATE INDEX ON fact_orders (order_id);

-- ── PERIODIC SNAPSHOT FACT (inventory) ────────────────────────────
-- One row per product per day (even if no activity)
CREATE TABLE fact_inventory_daily (
    snapshot_date_key   INT REFERENCES dim_date(date_key),
    product_key         BIGINT REFERENCES dim_product(product_key),
    -- Semi-additive measures (SUM by product, AVG by date)
    units_on_hand       INT,
    units_on_order      INT,
    units_sold          INT,
    inventory_value     NUMERIC(14, 2),
    days_of_supply      NUMERIC(6, 2),
    -- Classification
    abc_class           CHAR(1),  -- A=high velocity, B=medium, C=low
    stock_status        VARCHAR(20),  -- 'in_stock', 'low_stock', 'out_of_stock'
    PRIMARY KEY (snapshot_date_key, product_key)
);

-- ── ACCUMULATING SNAPSHOT FACT (order fulfillment) ────────────────
-- One row per order, updated as order progresses
CREATE TABLE fact_order_fulfillment (
    order_id            BIGINT PRIMARY KEY,
    customer_key        BIGINT,
    -- Multiple date keys (milestones)
    order_date_key      INT,
    paid_date_key       INT,
    picked_date_key     INT,
    shipped_date_key    INT,
    delivered_date_key  INT,
    returned_date_key   INT,
    -- Lag measures (days between milestones)
    days_to_payment     INT,
    days_to_fulfillment INT,
    days_to_delivery    INT,
    -- Measures
    order_total         NUMERIC(14, 2),
    -- Status
    current_status      VARCHAR(30),
    is_complete         BOOLEAN DEFAULT FALSE
);
```

---

## 🔄 ETL to Populate the Warehouse

```sql
-- ── SCD TYPE 2 UPSERT PROCEDURE ───────────────────────────────────
CREATE OR REPLACE PROCEDURE load_dim_customer(run_date DATE)
LANGUAGE plpgsql AS $$
DECLARE
    v_new_hash CHAR(64);
BEGIN
    -- Step 1: Identify changed records
    CREATE TEMP TABLE changed_customers AS
    SELECT
        s.customer_id,
        s.customer_name,
        s.email,
        s.phone,
        s.segment,
        s.region,
        s.country,
        s.city,
        MD5(CONCAT_WS('|',
            s.customer_name, s.email, s.phone,
            s.segment, s.region, s.country, s.city
        ))::CHAR(64) AS new_hash,
        d.row_hash AS old_hash,
        d.customer_key
    FROM staging_customers s
    LEFT JOIN dim_customer d 
        ON s.customer_id = d.customer_id 
        AND d.is_current = TRUE;
    
    -- Step 2: Expire changed records
    UPDATE dim_customer dc
    SET
        effective_to = run_date - 1,
        is_current = FALSE,
        updated_at = NOW()
    FROM changed_customers cc
    WHERE dc.customer_key = cc.customer_key
    AND cc.new_hash != cc.old_hash;
    
    -- Step 3: Insert new/changed records
    INSERT INTO dim_customer (
        customer_id, customer_name, email, phone,
        segment, region, country, city,
        effective_from, row_hash
    )
    SELECT
        customer_id, customer_name, email, phone,
        segment, region, country, city,
        run_date, new_hash
    FROM changed_customers
    WHERE old_hash IS NULL      -- New customer
       OR new_hash != old_hash; -- Changed customer
    
    DROP TABLE changed_customers;
END;
$$;

-- ── FACT TABLE LOAD (incremental) ────────────────────────────────
INSERT INTO fact_orders (
    date_key, customer_key, product_key,
    order_id, order_line_id,
    quantity, unit_price, discount_pct, discount_amount,
    gross_revenue, net_revenue, cost_of_goods, gross_profit,
    is_first_order, source_system
)
SELECT
    TO_CHAR(o.order_date, 'YYYYMMDD')::INT AS date_key,
    dc.customer_key,
    dp.product_key,
    o.order_id,
    ol.order_line_id,
    ol.quantity,
    ol.unit_price,
    ol.discount_pct,
    ol.quantity * ol.unit_price * ol.discount_pct AS discount_amount,
    ol.quantity * ol.unit_price AS gross_revenue,
    ol.quantity * ol.unit_price * (1 - ol.discount_pct) AS net_revenue,
    dp.cost * ol.quantity AS cost_of_goods,
    (ol.quantity * ol.unit_price * (1 - ol.discount_pct)) - (dp.cost * ol.quantity) AS gross_profit,
    -- Is this the customer's first order?
    NOT EXISTS (
        SELECT 1 FROM fact_orders fo2
        WHERE fo2.customer_key = dc.customer_key
        AND fo2.date_key < TO_CHAR(o.order_date, 'YYYYMMDD')::INT
    ) AS is_first_order,
    'orders_service'
FROM staging_orders o
JOIN staging_order_lines ol ON o.order_id = ol.order_id
JOIN dim_customer dc ON o.customer_id = dc.customer_id AND dc.is_current = TRUE
JOIN dim_product dp ON ol.product_id = dp.product_id AND dp.is_current = TRUE
WHERE o.order_date = CURRENT_DATE - 1  -- Yesterday's orders
ON CONFLICT (order_line_key) DO NOTHING;  -- Idempotent
```

---

## 📊 Analytics Queries on the Warehouse

```sql
-- ── COHORT ANALYSIS ────────────────────────────────────────────────
WITH cohorts AS (
    SELECT
        customer_key,
        DATE_TRUNC('month', MIN(d.date)) AS cohort_month
    FROM fact_orders fo
    JOIN dim_date d ON fo.date_key = d.date_key
    WHERE fo.is_first_order = TRUE
    GROUP BY customer_key
),
customer_activity AS (
    SELECT
        c.cohort_month,
        DATE_TRUNC('month', d.date) AS activity_month,
        COUNT(DISTINCT fo.customer_key) AS active_customers
    FROM fact_orders fo
    JOIN cohorts c ON fo.customer_key = c.customer_key
    JOIN dim_date d ON fo.date_key = d.date_key
    GROUP BY c.cohort_month, DATE_TRUNC('month', d.date)
),
cohort_sizes AS (
    SELECT cohort_month, COUNT(*) AS cohort_size
    FROM cohorts
    GROUP BY cohort_month
)
SELECT
    ca.cohort_month,
    EXTRACT(MONTH FROM AGE(ca.activity_month, ca.cohort_month)) AS months_since_start,
    ca.active_customers,
    cs.cohort_size,
    ROUND(100.0 * ca.active_customers / cs.cohort_size, 1) AS retention_pct
FROM customer_activity ca
JOIN cohort_sizes cs ON ca.cohort_month = cs.cohort_month
ORDER BY ca.cohort_month, months_since_start;

-- ── RFM ANALYSIS ──────────────────────────────────────────────────
WITH customer_rfm AS (
    SELECT
        fo.customer_key,
        dc.customer_name,
        MAX(d.date) AS last_order_date,
        CURRENT_DATE - MAX(d.date) AS recency_days,
        COUNT(DISTINCT fo.order_id) AS frequency,
        SUM(fo.net_revenue) AS monetary
    FROM fact_orders fo
    JOIN dim_date d ON fo.date_key = d.date_key
    JOIN dim_customer dc ON fo.customer_key = dc.customer_key AND dc.is_current = TRUE
    GROUP BY fo.customer_key, dc.customer_name
),
rfm_scores AS (
    SELECT
        *,
        NTILE(5) OVER (ORDER BY recency_days ASC) AS r_score,  -- 5=most recent
        NTILE(5) OVER (ORDER BY frequency DESC) AS f_score,
        NTILE(5) OVER (ORDER BY monetary DESC) AS m_score
    FROM customer_rfm
)
SELECT
    customer_key,
    customer_name,
    recency_days,
    frequency,
    ROUND(monetary, 2) AS monetary,
    r_score, f_score, m_score,
    r_score + f_score + m_score AS rfm_total,
    CASE
        WHEN r_score >= 4 AND f_score >= 4 AND m_score >= 4 THEN 'Champions'
        WHEN r_score >= 3 AND f_score >= 3 THEN 'Loyal Customers'
        WHEN r_score >= 4 AND f_score <= 2 THEN 'New Customers'
        WHEN r_score <= 2 AND f_score >= 3 THEN 'At Risk'
        WHEN r_score <= 2 AND f_score <= 2 THEN 'Lost'
        ELSE 'Potential Loyalists'
    END AS rfm_segment
FROM rfm_scores
ORDER BY rfm_total DESC;
```

---

*Related: [[04-System-Design/ETL-Pipelines]] | [[03-Tools/Snowflake]] | [[06-Interview-Prep/SQL-Questions]] | Back to [[00-Index]]*
