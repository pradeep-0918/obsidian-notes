# 📁 Project 1 — ETL Data Pipeline (Real Estate Data)
**Target Companies:** Mr. Cooper + JPMorgan Chase
**Duration:** 2 weeks | **Difficulty:** ⭐⭐ Beginner-Friendly

---

## 🎯 What You're Building

An **ETL (Extract → Transform → Load) pipeline** that:
1. **Extracts** raw housing/loan data from a CSV or API
2. **Transforms** it — cleans nulls, normalises columns, engineers features
3. **Loads** it into a SQLite or PostgreSQL database
4. **Visualises** key metrics using Matplotlib or Tableau

> This is the #1 most asked-about skill in data engineering interviews. Every company runs ETL pipelines.

---

## 📂 Folder Structure to Create

```
etl-real-estate-pipeline/
├── data/
│   ├── raw/              ← original CSV files go here
│   └── processed/        ← cleaned files go here
├── notebooks/
│   └── exploration.ipynb ← initial data exploration
├── src/
│   ├── extract.py        ← loading data
│   ├── transform.py      ← cleaning + feature engineering
│   ├── load.py           ← saving to database
│   └── pipeline.py       ← runs all 3 steps in order
├── dashboard/
│   └── visuals.py        ← charts
├── requirements.txt
└── README.md
```

---

## 📚 Concepts to Study First (Week 1, Days 1–3)

### What is ETL?
- **Extract**: Read data from a source (CSV, API, database)
- **Transform**: Clean it, fix errors, create new columns
- **Load**: Save it to a destination (database, data warehouse)

### Why does it matter at Mr. Cooper / JPMorgan?
> Every morning, their data pipelines automatically pull millions of loan records, clean them, and load them into dashboards. If the pipeline breaks, analysts see wrong data. Your job is to build and maintain these.

### Key Python Concepts to Learn
```python
# 1. Reading CSV files
import pandas as pd
df = pd.read_csv("data/raw/housing.csv")

# 2. Checking your data
df.shape          # how many rows and columns?
df.info()         # column types
df.isnull().sum() # how many missing values?
df.describe()     # statistics

# 3. Cleaning data
df.dropna(subset=["price"])              # remove rows with missing price
df["price"].fillna(df["price"].mean())   # fill missing with average
df.drop_duplicates()                     # remove duplicate rows
df.rename(columns={"sqft_living": "square_feet"})  # rename column

# 4. Creating new columns (Feature Engineering)
df["price_per_sqft"] = df["price"] / df["square_feet"]
df["is_expensive"] = df["price"] > 500000   # True/False column

# 5. Filtering data
df[df["bedrooms"] >= 3]           # only houses with 3+ bedrooms
df[(df["price"] > 100000) & (df["price"] < 500000)]  # price range
```

### SQL Concepts to Learn
```sql
-- Create a table
CREATE TABLE properties (
    id INTEGER PRIMARY KEY,
    price REAL,
    bedrooms INTEGER,
    square_feet REAL,
    city TEXT
);

-- Insert data
INSERT INTO properties VALUES (1, 350000, 3, 1500, 'Dallas');

-- Query data
SELECT city, AVG(price) as avg_price, COUNT(*) as total
FROM properties
GROUP BY city
ORDER BY avg_price DESC;

-- Filter
SELECT * FROM properties WHERE price > 300000 AND bedrooms >= 3;
```

---

## 🔨 Step-by-Step Build Guide (Week 1 Days 4–7 + Week 2)

### Step 1: Get the Dataset
- Go to **kaggle.com** and search "House Prices"
- Download the "House Prices - Advanced Regression Techniques" dataset
- Put `train.csv` into your `data/raw/` folder

### Step 2: Write `extract.py`
```python
import pandas as pd

def extract_data(filepath):
    """Load raw CSV data"""
    df = pd.read_csv(filepath)
    print(f"Extracted {len(df)} rows and {len(df.columns)} columns")
    return df

if __name__ == "__main__":
    df = extract_data("data/raw/train.csv")
    print(df.head())
```

### Step 3: Write `transform.py`
```python
import pandas as pd

def transform_data(df):
    """Clean and engineer features"""

    # 1. Drop columns with too many nulls (>50%)
    threshold = len(df) * 0.5
    df = df.dropna(thresh=threshold, axis=1)

    # 2. Fill remaining numeric nulls with median
    numeric_cols = df.select_dtypes(include='number').columns
    df[numeric_cols] = df[numeric_cols].fillna(df[numeric_cols].median())

    # 3. Fill remaining text nulls with 'Unknown'
    text_cols = df.select_dtypes(include='object').columns
    df[text_cols] = df[text_cols].fillna('Unknown')

    # 4. Feature Engineering
    df["TotalSF"] = df["TotalBsmtSF"] + df["1stFlrSF"] + df["2ndFlrSF"]
    df["HouseAge"] = 2024 - df["YearBuilt"]
    df["PricePerSF"] = df["SalePrice"] / df["TotalSF"]

    # 5. Remove outliers (houses with extremely high prices)
    df = df[df["SalePrice"] < df["SalePrice"].quantile(0.99)]

    print(f"Transformed data: {df.shape}")
    return df
```

### Step 4: Write `load.py`
```python
import sqlite3
import pandas as pd

def load_to_database(df, db_path="data/processed/housing.db"):
    """Load cleaned data into SQLite database"""
    conn = sqlite3.connect(db_path)
    df.to_sql("properties", conn, if_exists="replace", index=False)
    conn.close()
    print(f"Loaded {len(df)} rows into database: {db_path}")

def load_to_csv(df, output_path="data/processed/housing_clean.csv"):
    """Also save a clean CSV"""
    df.to_csv(output_path, index=False)
    print(f"Saved clean CSV: {output_path}")
```

### Step 5: Write `pipeline.py` (runs everything)
```python
from src.extract import extract_data
from src.transform import transform_data
from src.load import load_to_database, load_to_csv

def run_pipeline():
    print("=== Starting ETL Pipeline ===")

    # Step 1: Extract
    print("\n[1/3] Extracting data...")
    df_raw = extract_data("data/raw/train.csv")

    # Step 2: Transform
    print("\n[2/3] Transforming data...")
    df_clean = transform_data(df_raw)

    # Step 3: Load
    print("\n[3/3] Loading data...")
    load_to_database(df_clean)
    load_to_csv(df_clean)

    print("\n=== Pipeline Complete ✅ ===")

if __name__ == "__main__":
    run_pipeline()
```

### Step 6: Add Visualisations (`visuals.py`)
```python
import pandas as pd
import matplotlib.pyplot as plt
import sqlite3

conn = sqlite3.connect("data/processed/housing.db")
df = pd.read_sql("SELECT * FROM properties", conn)

# Chart 1: Price Distribution
plt.figure(figsize=(10, 5))
df["SalePrice"].hist(bins=50, color="steelblue", edgecolor="white")
plt.title("Distribution of House Sale Prices")
plt.xlabel("Sale Price ($)")
plt.ylabel("Count")
plt.tight_layout()
plt.savefig("dashboard/price_distribution.png")

# Chart 2: Price by Neighbourhood
top_neighborhoods = df.groupby("Neighborhood")["SalePrice"].mean().sort_values(ascending=False).head(10)
top_neighborhoods.plot(kind="barh", figsize=(10, 6), color="teal")
plt.title("Top 10 Neighbourhoods by Average Price")
plt.tight_layout()
plt.savefig("dashboard/price_by_neighborhood.png")

print("Charts saved!")
```

---

## ✅ Checklist Before Moving to Next Project

- [ ] Pipeline runs end-to-end without errors
- [ ] Database has the clean data loaded
- [ ] At least 3 charts generated
- [ ] Code pushed to GitHub
- [ ] README explains what the project does

---

## 💼 How to Explain This in an Interview

> *"I built an automated ETL pipeline using Python and Pandas that ingests raw housing data, applies data quality checks, engineers financial features like price-per-square-foot, and loads the output into a relational database. I also built visualisations to summarise key market trends. This is the kind of pipeline that financial data teams like Mr. Cooper's run daily for loan performance monitoring."*

---

## 🔗 Resources
- YouTube: "Python ETL Pipeline Tutorial" by Alex the Analyst
- Kaggle: kaggle.com/datasets — search "House Prices"
- Pandas Docs: pandas.pydata.org/docs

---
**Next Project →** [[P2 - Loan Default Prediction]]
