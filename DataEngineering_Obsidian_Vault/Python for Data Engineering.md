# 🐍 Python for Data Engineering

> **Level:** Beginner | **Stage:** 1 of 5
> Python is the most-used programming language in Data Engineering — for automation, data processing, and pipeline building.

---

## 🤔 Why Python for Data Engineering?

- **Glue language:** Connects databases, APIs, files, and cloud services
- **Rich ecosystem:** pandas, SQLAlchemy, boto3, requests, pyspark
- **Automation:** Schedule tasks, move files, trigger pipelines
- **Data manipulation:** Transform messy data before loading

> **Real-world analogy:** If SQL is how you query a database, Python is how you *automate* everything around it — fetching, cleaning, moving, and monitoring data.

---

## 🧠 Key Concepts

### 1. File Handling (CSV, JSON, Parquet)
```python
import pandas as pd

# Read CSV
df = pd.read_csv("sales.csv")

# Read JSON
df = pd.read_json("events.json")

# Read Parquet (columnar format — fast!)
df = pd.read_parquet("data.parquet")

# Write to Parquet
df.to_parquet("output.parquet", index=False)
```

### 2. Data Cleaning with Pandas
```python
# Drop duplicates
df.drop_duplicates(inplace=True)

# Fill missing values
df["age"].fillna(df["age"].median(), inplace=True)

# Rename columns
df.rename(columns={"amt": "amount", "usr": "user_id"}, inplace=True)

# Filter rows
high_value = df[df["amount"] > 1000]

# Group and aggregate
summary = df.groupby("region")["amount"].sum().reset_index()
```

### 3. Connecting to Databases
```python
import sqlalchemy as sa

engine = sa.create_engine("postgresql://user:password@localhost:5432/mydb")

# Read from DB
df = pd.read_sql("SELECT * FROM orders WHERE status = 'completed'", engine)

# Write to DB
df.to_sql("clean_orders", engine, if_exists="replace", index=False)
```

### 4. Working with APIs
```python
import requests

response = requests.get("https://api.example.com/data", headers={"Authorization": "Bearer TOKEN"})
data = response.json()

df = pd.DataFrame(data["records"])
```

### 5. Environment Variables & Config
```python
import os
from dotenv import load_dotenv

load_dotenv()
DB_PASSWORD = os.getenv("DB_PASSWORD")
```

### 6. Logging (Production Best Practice)
```python
import logging

logging.basicConfig(level=logging.INFO, format="%(asctime)s - %(levelname)s - %(message)s")
logger = logging.getLogger(__name__)

logger.info("Pipeline started")
logger.error("Failed to connect to database")
```

---

## 🏭 Real-World Use Cases

| Task | Python Approach |
|------|-----------------|
| Download data from an API daily | `requests` + cron / Airflow |
| Clean and transform a CSV | `pandas` |
| Load data into a database | `SQLAlchemy` + `to_sql()` |
| Process large files in chunks | `pd.read_csv(chunksize=10000)` |
| Run SQL and save results | `pd.read_sql()` + `to_parquet()` |
| Interact with AWS S3 | `boto3` |

---

## 🛠️ Tools & Libraries

| Library | Purpose |
|---------|---------|
| `pandas` | Data manipulation and analysis |
| `sqlalchemy` | Database connections (ORM) |
| `requests` | HTTP API calls |
| `boto3` | AWS SDK for Python |
| `pyspark` | Big data processing (see [[Apache Spark]]) |
| `great_expectations` | Data quality validation |
| `python-dotenv` | Manage secrets/config |
| `loguru` | Better logging |

---

## ✅ Mini Practice Tasks

- [ ] Read a CSV, remove duplicates, and save as Parquet
- [ ] Fetch data from a public API (e.g., OpenWeatherMap) and store it in a DataFrame
- [ ] Connect to a local PostgreSQL database and run a query using SQLAlchemy
- [ ] Write a function that reads a JSON file, filters records, and returns a summary
- [ ] Add logging to an existing Python script

---

## 🔗 Related Notes

- [[SQL for Data Engineering]] — Use Python to run SQL queries programmatically
- [[ETL vs ELT]] — Python powers the Extract and Transform steps
- [[Data Pipelines]] — Pipelines are often written in Python
- [[Apache Spark]] — PySpark is Python's interface to Spark
- [[Airflow]] — DAGs (pipeline workflows) are written in Python

---

## ➡️ Next Step

[[Data Modeling]] — Learn how to design efficient table structures before you start loading data.
