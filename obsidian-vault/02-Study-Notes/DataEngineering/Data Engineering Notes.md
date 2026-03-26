# 📚 Data Engineering Notes — Pipelines, Tools & Concepts

---

## 🏗️ What is Data Engineering?

Data Engineers **build and maintain the infrastructure** that moves, stores, and prepares data for analysts and ML models.

```
Raw Data Sources         →    ETL Pipeline      →    Storage         →  Consumers
(CSVs, APIs, DBs)        →  (Python/Spark)      →  (DB/Warehouse)   →  (Analysts, ML)

Mr. Cooper Example:
Loan Origination Files   →   Python ETL         →   Azure SQL        →  Risk Dashboard
Servicer Payments Data   →   PySpark Job        →   Data Warehouse   →  Compliance Report

JPMorgan Example:
Transaction Streams      →   Real-time Pipeline →   Data Lake        →  Fraud Detection
Market Data API          →   Batch ETL          →   Data Warehouse   →  Trading Desk
```

---

## 🔧 ETL Deep Dive

### Extract — Getting Data In
```python
import pandas as pd
import requests
import sqlite3

# From CSV
df = pd.read_csv("loans.csv")

# From API
response = requests.get("https://api.example.com/loans", headers={"Authorization": "Bearer TOKEN"})
data = response.json()
df = pd.DataFrame(data["results"])

# From SQL database
conn = sqlite3.connect("database.db")
df = pd.read_sql("SELECT * FROM loans WHERE status = 'active'", conn)

# From multiple files (batch)
import glob
all_files = glob.glob("data/raw/*.csv")
df_list = [pd.read_csv(f) for f in all_files]
df = pd.concat(df_list, ignore_index=True)
```

### Transform — Data Quality Rules
```python
class DataQualityChecker:
    def __init__(self, df):
        self.df = df
        self.issues = []

    def check_nulls(self, required_cols):
        for col in required_cols:
            null_count = self.df[col].isnull().sum()
            if null_count > 0:
                self.issues.append(f"Column '{col}' has {null_count} nulls")
        return self

    def check_duplicates(self, id_col):
        dupe_count = self.df.duplicated(subset=[id_col]).sum()
        if dupe_count > 0:
            self.issues.append(f"Found {dupe_count} duplicate {id_col} values")
        return self

    def check_ranges(self, col, min_val, max_val):
        out_of_range = ((self.df[col] < min_val) | (self.df[col] > max_val)).sum()
        if out_of_range > 0:
            self.issues.append(f"Column '{col}' has {out_of_range} values outside [{min_val}, {max_val}]")
        return self

    def report(self):
        if self.issues:
            print(f"⚠️  {len(self.issues)} data quality issue(s) found:")
            for issue in self.issues:
                print(f"   - {issue}")
        else:
            print("✅ All data quality checks passed!")
        return self

# Usage
checker = DataQualityChecker(df)
(checker
    .check_nulls(["loan_id", "loan_amount", "credit_score"])
    .check_duplicates("loan_id")
    .check_ranges("credit_score", 300, 850)
    .check_ranges("ltv", 0, 200)
    .report())
```

### Load — Storing Data
```python
import sqlite3
import pandas as pd

def load_incremental(df, table_name, id_col, db_path):
    """
    Load only NEW records (incremental load).
    Skips records already in the database.
    """
    conn = sqlite3.connect(db_path)

    # Get existing IDs
    existing_ids = pd.read_sql(f"SELECT {id_col} FROM {table_name}", conn)[id_col].tolist()

    # Filter to only new records
    new_records = df[~df[id_col].isin(existing_ids)]

    if len(new_records) > 0:
        new_records.to_sql(table_name, conn, if_exists="append", index=False)
        print(f"Loaded {len(new_records)} new records")
    else:
        print("No new records to load")

    conn.close()
```

---

## ⚡ PySpark Basics (For JPMorgan — Big Data)

```python
from pyspark.sql import SparkSession
from pyspark.sql import functions as F

# Start Spark
spark = SparkSession.builder.appName("LoanPipeline").getOrCreate()

# Read CSV
df = spark.read.csv("loans.csv", header=True, inferSchema=True)

# Basic operations (same logic as Pandas but distributed)
df.show(5)
df.printSchema()
df.count()

# Select and filter
high_risk = df.select("loan_id", "credit_score", "ltv") \
              .filter((F.col("credit_score") < 620) | (F.col("ltv") > 90))

# GroupBy (aggregation)
summary = df.groupBy("state") \
            .agg(
                F.count("loan_id").alias("total_loans"),
                F.avg("loan_amount").alias("avg_amount"),
                F.avg("credit_score").alias("avg_fico")
            ) \
            .orderBy(F.desc("total_loans"))

# Add new column
df = df.withColumn("credit_tier",
    F.when(F.col("credit_score") >= 750, "Excellent")
     .when(F.col("credit_score") >= 700, "Good")
     .when(F.col("credit_score") >= 650, "Fair")
     .otherwise("Poor")
)

# Save result
summary.write.csv("output/state_summary.csv", header=True)

# Stop Spark
spark.stop()
```

### Pandas vs PySpark — Key Differences
| Operation | Pandas | PySpark |
|---|---|---|
| Read CSV | `pd.read_csv()` | `spark.read.csv()` |
| Filter | `df[df["col"] > 5]` | `df.filter(F.col("col") > 5)` |
| New column | `df["x"] = df["a"] + df["b"]` | `df.withColumn("x", F.col("a") + F.col("b"))` |
| GroupBy | `df.groupby("col").agg(...)` | `df.groupBy("col").agg(...)` |
| Sort | `df.sort_values("col")` | `df.orderBy("col")` |

---

## ☁️ Azure Basics (For JPMorgan)

```
Key Azure Services for Data Engineering:

Azure Blob Storage    → Store raw files (like S3 in AWS)
Azure SQL Database    → Managed relational database
Azure Data Factory    → ETL orchestration (drag-and-drop pipelines)
Azure Databricks      → PySpark in the cloud
Azure Synapse         → Data warehouse + analytics

For your resume: mention "Azure" for data storage and pipelines
For interviews: "I used Azure Blob Storage to store raw loan files and
Azure SQL to store processed data"
```

---

## 🔀 Git — Version Control (Must Know)

```bash
# Start a new project
git init
git add .
git commit -m "Initial commit"

# Connect to GitHub
git remote add origin https://github.com/username/repo-name.git
git push -u origin main

# Daily workflow
git status              # see what changed
git add filename.py     # stage a file
git add .               # stage all changes
git commit -m "Add ETL transform function"  # save changes
git push                # upload to GitHub

# Create a branch (for new features)
git checkout -b feature/add-fraud-detection
git push -u origin feature/add-fraud-detection

# Merge back to main
git checkout main
git merge feature/add-fraud-detection
```

### Good Commit Message Examples
```
✅ "Add loan default prediction model with 87% AUC"
✅ "Fix null handling in credit_score column"
✅ "Add SMOTE oversampling to balance fraud dataset"
✅ "Add Tableau dashboard for mortgage analytics"

❌ "update"
❌ "fix bug"
❌ "changes"
```

---

## 📋 Good README Template

Copy this for every project:

```markdown
# Project Name

## 📌 Overview
One paragraph describing what this project does and why it matters.

## 🎯 Business Problem
What real-world problem does this solve? (1-2 sentences)

## 📊 Dataset
- Source: Kaggle / Fannie Mae / etc.
- Size: X rows, Y columns
- Time period: XXXX–XXXX

## 🛠️ Tech Stack
Python · Pandas · Scikit-learn · SQL · Matplotlib

## 🔍 Approach
1. Step 1
2. Step 2
3. Step 3

## 📈 Results
- Accuracy: XX%
- ROC-AUC: 0.XX
- Key finding: ...

## 🚀 How to Run
pip install -r requirements.txt
python pipeline.py

## 📁 Project Structure
(paste your folder tree here)
```

---
**Back to Home →** [[Dashboard]]
