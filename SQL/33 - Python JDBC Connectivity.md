# 33 — Python + MySQL Connectivity

#intermediate #dataengineering #python

---

## 🧠 Introduction

Data Engineers use Python to **automate SQL operations** — running queries, loading data, building ETL pipelines. Python connects to MySQL via `mysql-connector-python`, `PyMySQL`, or `SQLAlchemy`.

---

## 📦 Libraries Overview

| Library | Use Case | Install |
|---------|---------|---------|
| `mysql-connector-python` | Pure Python MySQL driver | `pip install mysql-connector-python` |
| `PyMySQL` | Pure Python, MySQL-compatible | `pip install pymysql` |
| `SQLAlchemy` | ORM + connection pooling | `pip install sqlalchemy` |
| `pandas` + SQLAlchemy | Read SQL → DataFrame | `pip install pandas sqlalchemy` |

---

## 🔷 mysql-connector-python (Basic)

```python
import mysql.connector

# Connect
conn = mysql.connector.connect(
    host     = "localhost",
    port     = 3306,
    user     = "root",
    password = "your_password",
    database = "ecommerce"
)

cursor = conn.cursor()

# Execute query
cursor.execute("SELECT user_id, name, city FROM users WHERE city = %s", ("Chennai",))

# Fetch results
rows = cursor.fetchall()
for row in rows:
    print(row)

# Fetch one row
cursor.execute("SELECT * FROM users WHERE user_id = %s", (1,))
user = cursor.fetchone()
print(user)

# Insert with parameterized query (ALWAYS use this — prevents SQL injection)
cursor.execute(
    "INSERT INTO users (name, email, city) VALUES (%s, %s, %s)",
    ("Karthik", "karthik@gmail.com", "Chennai")
)
conn.commit()
print(f"Inserted ID: {cursor.lastrowid}")

# Close
cursor.close()
conn.close()
```

---

## 🔶 Context Manager (Best Practice)

```python
import mysql.connector
from contextlib import contextmanager

@contextmanager
def get_connection():
    conn = mysql.connector.connect(
        host="localhost", user="root",
        password="password", database="ecommerce"
    )
    try:
        yield conn
    finally:
        conn.close()

# Usage
with get_connection() as conn:
    with conn.cursor(dictionary=True) as cursor:  # returns dicts instead of tuples
        cursor.execute("SELECT * FROM orders LIMIT 5")
        for row in cursor:
            print(row['order_id'], row['amount'])
```

---

## 🔵 SQLAlchemy — ORM & Connection Pooling

```python
from sqlalchemy import create_engine, text
import pandas as pd

# Create engine (connection pool built-in)
engine = create_engine(
    "mysql+pymysql://root:password@localhost:3306/ecommerce",
    pool_size=5,          # connections in pool
    max_overflow=10,      # extra connections allowed
    pool_timeout=30       # seconds to wait for connection
)

# Execute raw SQL
with engine.connect() as conn:
    result = conn.execute(text("SELECT * FROM orders LIMIT 10"))
    for row in result:
        print(dict(row))

# Read directly into pandas DataFrame
df_orders = pd.read_sql(
    "SELECT * FROM orders WHERE status = 'delivered'",
    engine
)
print(df_orders.head())
print(df_orders.shape)

# Write DataFrame to MySQL
df_orders.to_sql(
    name='orders_backup',
    con=engine,
    if_exists='replace',   # 'replace' drops & recreates, 'append' adds rows, 'fail' raises error
    index=False,
    dtype={'amount': 'DECIMAL(10,2)'}
)
```

---

## 🔧 ETL Pipeline Pattern

```python
import pandas as pd
from sqlalchemy import create_engine
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

class ETLPipeline:
    def __init__(self, source_url, target_url):
        self.source = create_engine(source_url)
        self.target = create_engine(target_url)
    
    def extract(self, query):
        """Extract data from source"""
        logger.info("Extracting data...")
        df = pd.read_sql(query, self.source)
        logger.info(f"Extracted {len(df)} rows")
        return df
    
    def transform(self, df):
        """Apply business logic transformations"""
        logger.info("Transforming data...")
        df = df.copy()
        df['amount'] = df['amount'].fillna(0)
        df['status'] = df['status'].str.upper()
        df['loaded_at'] = pd.Timestamp.now()
        df = df[df['amount'] > 0]  # filter invalid
        logger.info(f"After transform: {len(df)} rows")
        return df
    
    def load(self, df, table_name, mode='append'):
        """Load data to target"""
        logger.info(f"Loading {len(df)} rows to {table_name}...")
        df.to_sql(table_name, self.target, if_exists=mode, index=False, chunksize=1000)
        logger.info("Load complete!")

# Run pipeline
pipeline = ETLPipeline(
    source_url="mysql+pymysql://user:pass@source-host/source_db",
    target_url="mysql+pymysql://user:pass@warehouse-host/warehouse_db"
)

df = pipeline.extract("SELECT * FROM orders WHERE DATE(order_date) = CURDATE()")
df = pipeline.transform(df)
pipeline.load(df, 'fact_orders_daily')
```

---

## 🔐 Using .env for Credentials (Security Best Practice)

```python
# .env file (never commit to git!)
# DB_HOST=localhost
# DB_USER=root
# DB_PASSWORD=secret
# DB_NAME=ecommerce

from dotenv import load_dotenv
import os

load_dotenv()

conn = mysql.connector.connect(
    host     = os.getenv("DB_HOST"),
    user     = os.getenv("DB_USER"),
    password = os.getenv("DB_PASSWORD"),
    database = os.getenv("DB_NAME")
)
```

---

## 💼 Interview Questions

1. **What is the difference between `fetchone()` and `fetchall()`?**
   > `fetchone()` returns the next single row (memory efficient). `fetchall()` returns all rows at once (loads everything into memory — avoid for large result sets).

2. **Why use parameterized queries instead of f-strings?**
   > Prevents SQL injection. Always use `%s` placeholders and pass values separately — never concatenate user input directly into SQL strings.

3. **What is SQLAlchemy's connection pool?**
   > A cache of database connections reused across requests — avoids overhead of creating new connections per query. Critical for high-traffic applications.

4. **How do you load a large MySQL table into pandas efficiently?**
   ```python
   df = pd.read_sql("SELECT * FROM large_table", engine, chunksize=10000)
   for chunk in df:  # df is an iterator
       process(chunk)
   ```

5. **Scenario:** *"Build a pipeline to load yesterday's orders from MySQL into a data warehouse daily."*
   > Use SQLAlchemy to extract with date filter, pandas to transform/clean, `to_sql()` to load. Schedule with cron/Airflow. Use logging and error handling.

---

## 🔗 Related Topics

- [[34 - Data Engineering Projects]] — Full pipeline projects
- [[32 - Slowly Changing Dimensions]] — ETL for SCD
- [[04 - MySQL Notebooks]] — Local SQL development tools
- [[29 - GRANT REVOKE]] — Create restricted DB users for ETL
- [[28 - COMMIT ROLLBACK]] — Transaction handling in Python
