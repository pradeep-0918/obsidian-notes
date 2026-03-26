# 📚 Python Notes — Data Engineering & ML Focus

> Study these before starting projects. Return here to revise.

---

## 🐍 Core Python You Must Know

### Lists, Dicts, Comprehensions
```python
# List comprehension — faster than for loops
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]

# Dict comprehension
word_lengths = {word: len(word) for word in ["hello", "world", "python"]}

# Nested list comprehension
matrix = [[i*j for j in range(1,4)] for i in range(1,4)]
```

### Functions — Write Clean Reusable Code
```python
def calculate_ltv(loan_amount, property_value):
    """
    Calculate Loan-to-Value ratio.
    Args:
        loan_amount: float
        property_value: float
    Returns:
        float: LTV as percentage
    """
    if property_value == 0:
        raise ValueError("Property value cannot be zero")
    return (loan_amount / property_value) * 100

# Default arguments
def clean_dataframe(df, drop_nulls=True, fill_value=0):
    if drop_nulls:
        return df.dropna()
    return df.fillna(fill_value)
```

### Error Handling
```python
try:
    df = pd.read_csv("data/loans.csv")
except FileNotFoundError:
    print("Error: File not found. Check your path.")
except pd.errors.EmptyDataError:
    print("Error: File is empty.")
finally:
    print("Attempted to load data.")
```

---

## 🐼 Pandas — The Most Important Library

### Reading & Writing Data
```python
import pandas as pd

# Read
df = pd.read_csv("file.csv")
df = pd.read_excel("file.xlsx", sheet_name="Sheet1")
df = pd.read_sql("SELECT * FROM loans", conn)

# Write
df.to_csv("output.csv", index=False)
df.to_excel("output.xlsx", index=False)
df.to_sql("table_name", conn, if_exists="replace", index=False)
```

### Exploring Data — Do This FIRST on Every Dataset
```python
df.shape            # (rows, columns)
df.dtypes           # column types
df.info()           # summary with nulls
df.describe()       # statistics for numeric columns
df.head(10)         # first 10 rows
df.tail(10)         # last 10 rows
df.isnull().sum()   # count nulls per column
df.duplicated().sum() # count duplicate rows
df["column"].value_counts()  # frequency count
df["column"].nunique()       # number of unique values
```

### Cleaning Data
```python
# Drop
df.drop(columns=["useless_col"], inplace=True)
df.dropna(subset=["important_col"], inplace=True)
df.drop_duplicates(inplace=True)

# Fill missing values
df["col"].fillna(df["col"].mean(), inplace=True)    # fill with mean
df["col"].fillna(df["col"].median(), inplace=True)  # fill with median
df["col"].fillna("Unknown", inplace=True)           # fill with string

# Change types
df["col"] = pd.to_numeric(df["col"], errors="coerce")
df["date"] = pd.to_datetime(df["date"])
df["col"] = df["col"].astype(str)

# Rename
df.rename(columns={"old_name": "new_name"}, inplace=True)

# String operations
df["name"] = df["name"].str.lower()
df["name"] = df["name"].str.strip()
df["state"] = df["state"].str.upper()
```

### Feature Engineering
```python
# Create new columns
df["price_per_sqft"] = df["price"] / df["sqft"]
df["is_high_risk"] = (df["ltv"] > 90) | (df["credit_score"] < 620)
df["loan_age"] = 2024 - df["origination_year"]

# Binning / bucketing
df["credit_tier"] = pd.cut(df["credit_score"],
    bins=[0, 579, 669, 739, 799, 850],
    labels=["Poor", "Fair", "Good", "Very Good", "Excellent"])

# Log transformation (for skewed data like income/prices)
import numpy as np
df["log_income"] = np.log1p(df["income"])
```

### Groupby & Aggregations
```python
# Simple groupby
df.groupby("state")["loan_amount"].mean()

# Multiple columns
df.groupby(["state", "year"])["loan_amount"].agg(["sum", "mean", "count"])

# Custom aggregation
df.groupby("loan_type").agg(
    total_loans=("loan_id", "count"),
    avg_amount=("loan_amount", "mean"),
    default_rate=("default_flag", "mean")
)

# Transform (keep same row count)
df["state_avg_price"] = df.groupby("state")["price"].transform("mean")
```

---

## 📊 Matplotlib Quick Reference

```python
import matplotlib.pyplot as plt

# Line chart
plt.figure(figsize=(12, 5))
plt.plot(x, y, color="steelblue", linewidth=2, label="Line 1")
plt.title("Title", fontsize=14)
plt.xlabel("X Axis")
plt.ylabel("Y Axis")
plt.legend()
plt.grid(alpha=0.3)
plt.tight_layout()
plt.savefig("chart.png", dpi=150)
plt.show()

# Bar chart
plt.bar(categories, values, color="teal", edgecolor="white")

# Histogram
plt.hist(df["col"], bins=50, color="steelblue", edgecolor="white")

# Scatter plot
plt.scatter(x, y, alpha=0.3, c=colours, cmap="coolwarm")
plt.colorbar()

# Subplots (multiple charts)
fig, axes = plt.subplots(2, 2, figsize=(12, 10))
axes[0,0].plot(x, y)       # top-left
axes[0,1].bar(cats, vals)  # top-right
axes[1,0].hist(data)       # bottom-left
axes[1,1].scatter(x, y)   # bottom-right
plt.tight_layout()
```

---

## 💾 SQLite with Python

```python
import sqlite3
import pandas as pd

# Connect
conn = sqlite3.connect("database.db")
cursor = conn.cursor()

# Create table
cursor.execute("""
    CREATE TABLE IF NOT EXISTS loans (
        id INTEGER PRIMARY KEY,
        borrower_name TEXT,
        loan_amount REAL,
        credit_score INTEGER,
        default_flag INTEGER
    )
""")
conn.commit()

# Load dataframe into SQLite
df.to_sql("loans", conn, if_exists="replace", index=False)

# Query back
df_from_db = pd.read_sql("SELECT * FROM loans WHERE default_flag = 1", conn)

# Close
conn.close()
```

---

## 📦 Requirements File (Always Include This)

Create `requirements.txt`:
```
pandas==2.0.3
numpy==1.24.3
scikit-learn==1.3.0
matplotlib==3.7.2
seaborn==0.12.2
xgboost==1.7.6
imbalanced-learn==0.11.0
jupyter==1.0.0
yfinance==0.2.28
transformers==4.33.0
vaderSentiment==3.3.2
flask==2.3.3
```

Install all: `pip install -r requirements.txt`

---
**Back to Home →** [[Dashboard]]
**Next Study Note →** [[SQL Notes]]
