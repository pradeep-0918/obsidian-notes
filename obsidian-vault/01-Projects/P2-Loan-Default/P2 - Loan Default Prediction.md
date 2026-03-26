# 📁 Project 2 — Loan Default Prediction Engine
**Target Companies:** Mr. Cooper + JPMorgan Chase
**Duration:** 2 weeks | **Difficulty:** ⭐⭐⭐ Intermediate

---

## 🎯 What You're Building

A **machine learning model** that predicts whether a borrower will **default on their loan** based on financial features like:
- Credit score
- Debt-to-Income ratio (DTI)
- Loan-to-Value ratio (LTV)
- Employment status
- Loan amount

> This is literally what Mr. Cooper and JPMorgan's risk teams do. You will be able to say "I built a loan risk model" in your interview.

---

## 📂 Folder Structure

```
loan-default-prediction/
├── data/
│   └── loan_data.csv
├── notebooks/
│   └── 01_exploration.ipynb
│   └── 02_modelling.ipynb
├── src/
│   ├── preprocess.py
│   ├── train_model.py
│   └── evaluate.py
├── models/
│   └── loan_model.pkl       ← saved trained model
├── reports/
│   └── model_report.md
└── README.md
```

---

## 📚 Concepts to Study First (Week 1, Days 1–3)

### What is Loan Default?
- A borrower **defaults** when they stop making payments
- Banks lose money → they want to predict this BEFORE giving the loan
- Your model will output: **"High Risk"** or **"Low Risk"**

### Key ML Concepts You Need

#### 1. Classification vs Regression
```
Regression  → predicts a NUMBER (e.g., house price = $350,000)
Classification → predicts a CATEGORY (e.g., default = Yes or No)
Loan default is a CLASSIFICATION problem
```

#### 2. Your Features (Inputs) and Target (Output)
```python
Features (X) = what you use to predict:
  - credit_score        (e.g., 650)
  - debt_to_income      (e.g., 0.43 = 43%)
  - loan_to_value       (e.g., 0.80 = 80%)
  - employment_years    (e.g., 5)
  - loan_amount         (e.g., 200000)

Target (y) = what you're predicting:
  - default             (0 = No, 1 = Yes)
```

#### 3. Train/Test Split — Why?
```python
# You train your model on 80% of data
# You TEST it on the remaining 20% (data it has NEVER seen)
# This simulates how it would work in the real world

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

#### 4. The Models You'll Use

**Logistic Regression** — Simple baseline
```python
# Despite the name, it's a CLASSIFICATION model
# Outputs a probability: "70% chance of default"
# Good starting point, easy to explain to business teams
from sklearn.linear_model import LogisticRegression
```

**Random Forest** — Better accuracy
```python
# Builds many decision trees and combines them
# More accurate than logistic regression
# Harder to explain but very powerful
from sklearn.ensemble import RandomForestClassifier
```

**XGBoost** — Best accuracy
```python
# Industry standard for tabular data like loan records
# Used by most banks for credit scoring
# Harder to tune but gives best results
import xgboost as xgb
```

#### 5. How to Measure if Your Model is Good

```python
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

# Accuracy = % of predictions that are correct
# But accuracy alone can be misleading!

# Example of misleading accuracy:
# If only 5% of loans default, a model that ALWAYS says "no default"
# gets 95% accuracy but is completely useless!

# Use these instead:
# Precision = of loans flagged as risky, how many actually defaulted?
# Recall = of all real defaults, how many did you catch?
# F1 Score = balance of precision and recall

print(classification_report(y_test, y_pred))
```

---

## 🔨 Step-by-Step Build Guide

### Step 1: Get the Dataset
Option A (Recommended): **Kaggle — "Home Credit Default Risk"**
- URL: kaggle.com → search "Home Credit Default Risk"
- This is a real-world bank dataset with 300,000+ loan applications

Option B: **LendingClub Loan Data** on Kaggle
- Real peer-to-peer lending data

### Step 2: Explore Your Data (`01_exploration.ipynb`)
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("data/loan_data.csv")

# 1. Basic info
print(df.shape)
print(df.isnull().sum().sort_values(ascending=False).head(20))

# 2. How many defaults vs non-defaults?
df["TARGET"].value_counts().plot(kind="bar", color=["steelblue", "crimson"])
plt.title("Default vs Non-Default")
plt.savefig("reports/class_balance.png")

# 3. Credit score vs default
df.boxplot(column="EXT_SOURCE_2", by="TARGET")
plt.savefig("reports/credit_vs_default.png")
```

### Step 3: Preprocess (`preprocess.py`)
```python
import pandas as pd
from sklearn.preprocessing import StandardScaler, LabelEncoder

def preprocess(df):
    # 1. Select key features
    features = [
        "EXT_SOURCE_1", "EXT_SOURCE_2", "EXT_SOURCE_3",
        "AMT_CREDIT", "AMT_INCOME_TOTAL", "AMT_ANNUITY",
        "DAYS_BIRTH", "DAYS_EMPLOYED", "NAME_CONTRACT_TYPE"
    ]
    target = "TARGET"

    df = df[features + [target]].copy()

    # 2. Fill missing values
    df["EXT_SOURCE_1"] = df["EXT_SOURCE_1"].fillna(df["EXT_SOURCE_1"].median())
    df["EXT_SOURCE_2"] = df["EXT_SOURCE_2"].fillna(df["EXT_SOURCE_2"].median())
    df["EXT_SOURCE_3"] = df["EXT_SOURCE_3"].fillna(df["EXT_SOURCE_3"].median())
    df["AMT_ANNUITY"] = df["AMT_ANNUITY"].fillna(df["AMT_ANNUITY"].median())

    # 3. Encode categorical columns
    le = LabelEncoder()
    df["NAME_CONTRACT_TYPE"] = le.fit_transform(df["NAME_CONTRACT_TYPE"])

    # 4. Feature Engineering
    df["CREDIT_INCOME_RATIO"] = df["AMT_CREDIT"] / df["AMT_INCOME_TOTAL"]
    df["ANNUITY_INCOME_RATIO"] = df["AMT_ANNUITY"] / df["AMT_INCOME_TOTAL"]
    df["AGE_YEARS"] = -df["DAYS_BIRTH"] / 365

    # 5. Separate features and target
    X = df.drop(columns=[target])
    y = df[target]

    return X, y
```

### Step 4: Train Your Models (`train_model.py`)
```python
import pickle
from sklearn.linear_model import LogisticRegression
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler

def train(X, y):
    # Split
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.2, random_state=42, stratify=y
    )

    # Scale features
    scaler = StandardScaler()
    X_train_scaled = scaler.fit_transform(X_train)
    X_test_scaled = scaler.transform(X_test)

    # Model 1: Logistic Regression (baseline)
    lr = LogisticRegression(max_iter=1000)
    lr.fit(X_train_scaled, y_train)

    # Model 2: Random Forest (better)
    rf = RandomForestClassifier(n_estimators=100, random_state=42, n_jobs=-1)
    rf.fit(X_train, y_train)

    # Save best model
    with open("models/loan_model.pkl", "wb") as f:
        pickle.dump(rf, f)

    return lr, rf, X_test, X_test_scaled, y_test
```

### Step 5: Evaluate (`evaluate.py`)
```python
from sklearn.metrics import classification_report, roc_auc_score
import matplotlib.pyplot as plt
from sklearn.metrics import RocCurveDisplay

def evaluate(model, X_test, y_test, model_name):
    y_pred = model.predict(X_test)
    y_prob = model.predict_proba(X_test)[:, 1]

    print(f"\n=== {model_name} Results ===")
    print(classification_report(y_test, y_pred))
    print(f"ROC-AUC Score: {roc_auc_score(y_test, y_prob):.4f}")

    # Feature Importance (Random Forest)
    if hasattr(model, "feature_importances_"):
        import pandas as pd
        feat_imp = pd.Series(model.feature_importances_, index=X_test.columns)
        feat_imp.sort_values().tail(10).plot(kind="barh", figsize=(8, 5))
        plt.title("Top 10 Most Important Features")
        plt.tight_layout()
        plt.savefig("reports/feature_importance.png")
```

---

## 📊 What Good Results Look Like

| Metric | Acceptable | Good | Excellent |
|---|---|---|---|
| Accuracy | >75% | >82% | >88% |
| ROC-AUC | >0.65 | >0.75 | >0.82 |
| F1 Score (Default class) | >0.40 | >0.55 | >0.65 |

---

## ✅ Checklist

- [ ] Dataset downloaded and placed in `data/`
- [ ] Exploratory notebook complete with 3+ charts
- [ ] Preprocessing pipeline written
- [ ] At least 2 models trained and compared
- [ ] Evaluation report with AUC score generated
- [ ] Feature importance chart saved
- [ ] Pushed to GitHub with clear README

---

## 💼 How to Explain This in an Interview

> *"I built a binary classification model to predict loan defaults using the Home Credit dataset with 300,000+ records. I engineered financial features like credit-to-income ratio and annuity burden, then compared Logistic Regression and Random Forest models. My Random Forest achieved an ROC-AUC of 0.76, meaning it correctly ranks 76% of risky borrowers above safe ones. This directly mirrors the kind of underwriting risk models that JPMorgan and Mr. Cooper use in production."*

---

## 🔗 Resources
- Kaggle: "Home Credit Default Risk" competition
- YouTube: "Loan Default Prediction" by Krish Naik
- Article: "Understanding ROC-AUC" on Towards Data Science

---
**Previous →** [[P1 - ETL Data Pipeline]]
**Next →** [[P3 - Fraud Detection System]]
