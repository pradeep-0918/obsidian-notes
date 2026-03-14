# 🍽️ Restaurant Intelligence System

## STEP 2 — Data Ingestion & Dataset Understanding

---

# 🎯 Objective of This Step

Before building ML systems, engineers must **understand and ingest the data properly**.

In this step you will learn:

* What **data ingestion** is
* How to **structure raw datasets**
* How to **load data using pandas**
* How to **inspect datasets**
* How to **identify data quality issues**

At the end of this step you should have:

* Dataset downloaded
* Raw dataset stored correctly
* Dataset loaded into Python
* Initial dataset inspection completed

---

# 🧠 What is Data Ingestion?

**Data ingestion** is the process of **collecting and importing data into a system for analysis or processing**.

In real companies, data comes from:

* APIs
* Databases
* Logs
* CSV files
* Streaming data
* Data warehouses

For this project we will simulate a **real-world analytics pipeline** using a dataset.

Pipeline structure:

```
Raw Data
   ↓
Data Ingestion
   ↓
Data Cleaning
   ↓
Feature Engineering
   ↓
Machine Learning
   ↓
Dashboard
```

---

# 📂 Data Storage Strategy

Good engineers **never mix raw data with processed data**.

We follow this rule:

```
data/
   raw/
   processed/
```

### Raw Data

Original dataset.

Rules:

* Never modify
* Always keep original copy

### Processed Data

Cleaned or transformed datasets used for ML.

---

# 📊 Dataset for This Project

We will use a **restaurant dataset similar to Zomato restaurant data**.

Typical columns include:

* Restaurant Name
* Location
* Cuisine
* Average Cost
* Votes
* Rating
* Delivery availability
* Online ordering

These features are useful for:

* analytics
* rating prediction
* recommendation systems

---

# 🧪 Data Science Rule: First Look at Data

When you receive a dataset you **NEVER start modeling immediately**.

First you must understand:

* dataset size
* column types
* missing values
* duplicates
* distributions

---

# 🐼 Loading Data with Pandas

The pandas library is the **most important library in data engineering and data science**.

It provides the **DataFrame**, which is a table-like structure.

Example mental model:

```
DataFrame = Excel Sheet in Python
```

Rows → observations
Columns → features

---

# 🔍 Dataset Exploration Checklist

When you load a dataset, always inspect:

### 1️⃣ First Rows

Purpose:
Understand the dataset structure.

```
head()
```

---

### 2️⃣ Dataset Shape

Purpose:
Understand size.

```
rows × columns
```

Example:

```
5000 rows
12 columns
```

---

### 3️⃣ Column Names

Purpose:
Understand available features.

Example columns:

```
Restaurant Name
Location
Cuisine
Rating
Votes
Cost
```

---

### 4️⃣ Data Types

Important for ML.

Typical types:

```
int
float
string
boolean
```

Example:

```
Votes → integer
Rating → float
Cuisine → string
```

---

### 5️⃣ Missing Values

Real-world data always has missing values.

Example:

```
Rating = NULL
Cuisine = NULL
Votes = NULL
```

We must later decide how to handle them.

---

### 6️⃣ Duplicate Rows

Duplicate data creates **bias in ML models**.

Example:

```
Same restaurant repeated multiple times
```

---

### 7️⃣ Basic Statistics

Important numeric insights:

* average cost
* average rating
* vote distribution

---

# 📁 Engineering Rule

Never write scripts directly in notebooks.

Instead:

```
notebooks → exploration
src → production code
```

For now we use:

```
notebooks/data_exploration.ipynb
```

---

# 📂 Folder Structure Now

```
restaurant-intelligence-system

data/
   raw/
   processed/

notebooks/

src/
   data/
   features/
   models/
   visualization/
```

---

# 🎯 Your Tasks

### Task 1 — Download Dataset

Download a restaurant dataset and place it inside:

```
data/raw/
```

Example filename:

```
restaurants.csv
```

---

### Task 2 — Create Notebook

Create notebook:

```
notebooks/data_exploration.ipynb
```

---

### Task 3 — Load Dataset

Using pandas:

* import pandas
* read CSV file
* store in dataframe

---

### Task 4 — Inspect Dataset

Analyze:

* head
* shape
* columns
* info
* missing values
* duplicates

---

### Task 5 — Write Observations

Create a section in the notebook:

```
Dataset Observations
```

Write things like:

Example:

```
Dataset contains 5000 restaurants
Rating column has missing values
Cuisine column contains multiple categories
Votes column ranges from 0–5000
```

---

# 📊 Expected Output

You should have:

```
dataset loaded successfully
dataset statistics printed
data issues identified
```

Example findings:

```
Missing ratings
duplicate restaurants
mixed cuisines in one column
```

---

# 🧠 Important Learning

You are now learning **EDA thinking**, which is essential for:

* Data Scientists
* Data Engineers
* ML Engineers

A good engineer spends **60–70% of time understanding data**.

---

# 📺 Short Learning Resource

Recommended video:

**"Pandas Data Analysis Full Tutorial"**

Length: ~30 minutes

Watch until:

* reading CSV
* head
* info
* describe

You do not need advanced topics yet.

---

# 📌 Progress Tracker

```
[ ] Dataset downloaded
[ ] Dataset placed in data/raw
[ ] Notebook created
[ ] Dataset loaded with pandas
[ ] Dataset inspected
[ ] Observations written
```

---

# 🧾 Git Commit

Once finished:

```
git add .
git commit -m "Added dataset and performed initial data exploration"
git push
```

---

# 🚀 What Comes Next

Next step will be **extremely important**.

You will learn:

```
STEP 3 — Data Cleaning Pipeline
```

Topics include:

* handling missing values
* removing duplicates
* fixing data types
* feature normalization
* creating a reusable data pipeline

This is **core Data Engineering work**.

---
