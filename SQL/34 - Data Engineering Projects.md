# 34 — Data Engineering Projects

#advanced #dataengineering #realworld

---

## 🧠 Introduction

Projects are how you **prove competency** in interviews. These are end-to-end projects that integrate all SQL and Data Engineering concepts — design, implement, optimize, and document.

---

## 🏗️ Project 1 — E-Commerce Analytics Data Warehouse

**Business Problem:** A growing e-commerce company has 3 years of transaction data in MySQL. The analytics team needs a fast data warehouse for reporting.

### Architecture

```
MySQL (OLTP) → ETL Pipeline (Python) → MySQL DW (OLAP)
                                         └── Star Schema
```

### Star Schema Design

```sql
-- Dimension: Date (pre-populated)
CREATE TABLE dim_date (
    date_key    INT PRIMARY KEY,          -- YYYYMMDD e.g. 20240715
    full_date   DATE UNIQUE,
    day_of_week TINYINT,
    day_name    VARCHAR(10),
    week_num    TINYINT,
    month_num   TINYINT,
    month_name  VARCHAR(10),
    quarter     TINYINT,
    year        SMALLINT,
    is_weekend  BOOLEAN
);

-- Dimension: Customer (SCD Type 2)
CREATE TABLE dim_customer (
    customer_sk  INT PRIMARY KEY AUTO_INCREMENT,  -- surrogate key
    customer_id  INT,
    name         VARCHAR(100),
    email        VARCHAR(255),
    city         VARCHAR(50),
    tier         VARCHAR(20),
    effective_dt DATE,
    expiry_dt    DATE,
    is_current   BOOLEAN DEFAULT TRUE
);

-- Dimension: Product
CREATE TABLE dim_product (
    product_sk   INT PRIMARY KEY AUTO_INCREMENT,
    product_id   INT UNIQUE,
    name         VARCHAR(200),
    category     VARCHAR(50),
    sub_category VARCHAR(50),
    brand        VARCHAR(100),
    price        DECIMAL(10,2)
);

-- Fact: Orders
CREATE TABLE fact_orders (
    order_sk        BIGINT PRIMARY KEY AUTO_INCREMENT,
    order_id        INT,
    date_key        INT,
    customer_sk     INT,
    product_sk      INT,
    quantity        INT,
    unit_price      DECIMAL(10,2),
    discount_amount DECIMAL(10,2),
    total_amount    DECIMAL(10,2),
    FOREIGN KEY (date_key)    REFERENCES dim_date(date_key),
    FOREIGN KEY (customer_sk) REFERENCES dim_customer(customer_sk),
    FOREIGN KEY (product_sk)  REFERENCES dim_product(product_sk),
    INDEX idx_date     (date_key),
    INDEX idx_customer (customer_sk),
    INDEX idx_product  (product_sk)
)
PARTITION BY RANGE (date_key) (
    PARTITION p2022 VALUES LESS THAN (20230101),
    PARTITION p2023 VALUES LESS THAN (20240101),
    PARTITION p2024 VALUES LESS THAN (20250101),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);
```

### ETL Pipeline

```python
import pandas as pd
from sqlalchemy import create_engine
from datetime import datetime, timedelta

source  = create_engine("mysql+pymysql://user:pass@oltp-host/ecommerce")
target  = create_engine("mysql+pymysql://user:pass@dw-host/ecommerce_dw")

def etl_daily_orders(run_date):
    # Extract
    df = pd.read_sql(f"""
        SELECT o.order_id, o.user_id, o.order_date,
               oi.product_id, oi.quantity, oi.unit_price,
               o.discount, oi.quantity * oi.unit_price AS line_total
        FROM orders o
        JOIN order_items oi ON o.order_id = oi.order_id
        WHERE DATE(o.order_date) = '{run_date}'
          AND o.status = 'delivered'
    """, source)
    
    # Transform: resolve dimension keys
    df['date_key'] = pd.to_datetime(df['order_date']).dt.strftime('%Y%m%d').astype(int)
    
    # Get surrogate keys for customers
    customers = pd.read_sql(f"""
        SELECT customer_id, customer_sk FROM dim_customer
        WHERE is_current = TRUE
    """, target)
    df = df.merge(customers, left_on='user_id', right_on='customer_id')
    
    # Load
    df[['order_id','date_key','customer_sk','product_sk',
        'quantity','unit_price','discount','line_total']].to_sql(
        'fact_orders', target, if_exists='append', index=False
    )
    print(f"Loaded {len(df)} rows for {run_date}")

# Run
etl_daily_orders((datetime.now() - timedelta(days=1)).strftime('%Y-%m-%d'))
```

### Business Queries on the DW

```sql
-- Monthly revenue by category
SELECT
    d.year, d.month_name,
    p.category,
    SUM(f.total_amount) AS revenue,
    SUM(f.quantity)     AS units_sold
FROM fact_orders f
JOIN dim_date d     ON f.date_key    = d.date_key
JOIN dim_product p  ON f.product_sk  = p.product_sk
WHERE d.year = 2024
GROUP BY d.year, d.month_num, d.month_name, p.category
ORDER BY d.month_num, revenue DESC;

-- Customer LTV with SCD2 history
SELECT
    c.customer_id,
    c.name,
    c.city AS current_city,
    COUNT(DISTINCT f.order_id) AS total_orders,
    SUM(f.total_amount) AS lifetime_value
FROM fact_orders f
JOIN dim_customer c ON f.customer_sk = c.customer_sk
WHERE c.is_current = TRUE
GROUP BY c.customer_id, c.name, c.city
HAVING lifetime_value > 50000
ORDER BY lifetime_value DESC;
```

---

## 🚕 Project 2 — Ride-Sharing Analytics Pipeline

**Problem:** Analyze driver performance and demand patterns for a ride-sharing platform.

### Key SQL Queries

```sql
-- Driver performance dashboard
SELECT
    d.driver_id,
    d.name,
    d.city,
    COUNT(t.trip_id)                                         AS total_trips,
    ROUND(AVG(t.rating), 2)                                  AS avg_rating,
    ROUND(SUM(t.fare), 2)                                    AS total_earnings,
    ROUND(AVG(t.distance_km), 2)                             AS avg_trip_distance,
    RANK() OVER (PARTITION BY d.city ORDER BY SUM(t.fare) DESC) AS city_rank
FROM drivers d
JOIN trips t ON d.driver_id = t.driver_id
WHERE t.status = 'completed'
  AND t.trip_date >= DATE_SUB(CURDATE(), INTERVAL 30 DAY)
GROUP BY d.driver_id, d.name, d.city;

-- Demand heatmap: hour × day of week
SELECT
    DAYNAME(trip_date)    AS day_of_week,
    HOUR(pickup_time)     AS hour_of_day,
    COUNT(*)              AS trip_count,
    AVG(fare)             AS avg_fare,
    CASE
        WHEN COUNT(*) > 200 THEN 'High Demand'
        WHEN COUNT(*) > 100 THEN 'Medium Demand'
        ELSE 'Low Demand'
    END AS demand_level
FROM trips
WHERE status = 'completed'
  AND trip_date >= DATE_SUB(CURDATE(), INTERVAL 90 DAY)
GROUP BY DAYNAME(trip_date), DAYOFWEEK(trip_date), HOUR(pickup_time)
ORDER BY DAYOFWEEK(trip_date), hour_of_day;
```

---

## 🏦 Project 3 — Banking Fraud Detection

```sql
-- Flag suspicious transactions (rapid succession, high amounts)
WITH transaction_analysis AS (
    SELECT
        t.txn_id,
        t.account_id,
        t.amount,
        t.txn_time,
        t.location,
        LAG(t.txn_time) OVER (PARTITION BY t.account_id ORDER BY t.txn_time)
            AS prev_txn_time,
        LAG(t.location) OVER (PARTITION BY t.account_id ORDER BY t.txn_time)
            AS prev_location,
        COUNT(*) OVER (
            PARTITION BY t.account_id
            ORDER BY t.txn_time
            RANGE BETWEEN INTERVAL 1 HOUR PRECEDING AND CURRENT ROW
        ) AS txns_in_last_hour,
        SUM(t.amount) OVER (
            PARTITION BY t.account_id
            ORDER BY t.txn_time
            RANGE BETWEEN INTERVAL 1 HOUR PRECEDING AND CURRENT ROW
        ) AS amount_in_last_hour
    FROM transactions t
)
SELECT *,
    CASE
        WHEN txns_in_last_hour > 5          THEN 'HIGH: Multiple transactions'
        WHEN amount_in_last_hour > 100000   THEN 'HIGH: Large total exposure'
        WHEN amount > 50000                 THEN 'MEDIUM: Large single transaction'
        WHEN prev_location != location
         AND TIMESTAMPDIFF(MINUTE, prev_txn_time, txn_time) < 30
                                            THEN 'HIGH: Impossible travel'
        ELSE 'NORMAL'
    END AS fraud_flag
FROM transaction_analysis
WHERE fraud_flag != 'NORMAL';
```

---

## 📋 Interview Preparation Checklist

### SQL Core (Must Know)
- [ ] All JOIN types with examples
- [ ] Window functions (ROW_NUMBER, LAG, LEAD, running totals)
- [ ] Subqueries vs CTEs vs JOINs — when to use each
- [ ] Indexes: creating, composite, covering, EXPLAIN analysis
- [ ] Aggregation with conditional aggregation (CASE in SUM/COUNT)

### Data Engineering (Must Know)
- [ ] Star schema design (fact + dimension tables)
- [ ] SCD Type 1 vs Type 2 — implementation
- [ ] ETL pipeline in Python with SQLAlchemy
- [ ] Partitioning strategy for large tables
- [ ] ACID properties and transaction design

### Scenario Questions to Practice
1. "Design a database schema for [application]"
2. "This query is slow — optimize it"
3. "How do you handle late-arriving data in your pipeline?"
4. "Explain your SCD2 implementation for customer data"
5. "How do you ensure data quality in your ETL?"

---

## 🔗 Related Topics

- [[32 - Slowly Changing Dimensions]] — SCD in warehouse projects
- [[23 - Window Functions]] — Analytics in project queries
- [[33 - Python JDBC Connectivity]] — Python ETL implementation
- [[27 - Partitioning]] — Large table management
- [[31 - Normalization]] — OLTP schema design
- [[26 - EXPLAIN ANALYZE]] — Query optimization in projects
