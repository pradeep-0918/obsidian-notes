# 📁 Project 3 — Credit Card Fraud Detection System
**Target Companies:** JPMorgan Chase (primary) + Mr. Cooper
**Duration:** 2 weeks | **Difficulty:** ⭐⭐⭐ Intermediate

---

## 🎯 What You're Building

A **fraud detection ML pipeline** that flags suspicious credit card transactions in real time.

You'll solve one of the hardest problems in ML: **class imbalance** — where out of 284,000 transactions, only 492 are fraud (0.17%). This is exactly the challenge JPMorgan Chase faces every single day.

---

## 📚 Concepts to Study First (Week 1, Days 1–3)

### Why is Fraud Detection Hard?

```
Out of 284,807 transactions:
  ✅ Legitimate:  284,315  (99.83%)
  ❌ Fraud:           492   (0.17%)

If your model just says "LEGIT" for everything → 99.83% accuracy
But it catches ZERO fraud → completely useless!

This is called the CLASS IMBALANCE PROBLEM
```

### The Solution: SMOTE + Balanced Models

**SMOTE** = Synthetic Minority Over-sampling Technique
- Creates **synthetic** fraud examples to balance the dataset
- So your model gets to "see" more fraud cases during training

```python
from imblearn.over_sampling import SMOTE

sm = SMOTE(random_state=42)
X_resampled, y_resampled = sm.fit_resample(X_train, y_train)

# Before SMOTE: 284,315 legit, 492 fraud
# After SMOTE:  284,315 legit, 284,315 synthetic fraud
# Now 50/50 balance → model learns both classes equally
```

### Key Metric: Precision vs Recall Trade-off

```
In fraud detection, you must choose:

HIGH RECALL (catch more fraud)
→ Flag more transactions as suspicious
→ More false alarms (annoying for customers)
→ But catch more real fraud
→ Banks usually prefer this

HIGH PRECISION (be more sure before flagging)
→ Only flag transactions you're very confident about
→ Fewer false alarms
→ But miss some real fraud
```

### Isolation Forest — Anomaly Detection Approach
```python
# Instead of supervised learning, treat fraud as an "anomaly"
# Train only on NORMAL transactions
# Flag anything that looks unusual as fraud
# No need for labels!

from sklearn.ensemble import IsolationForest

iso = IsolationForest(contamination=0.002, random_state=42)
iso.fit(X_train_normal)
predictions = iso.predict(X_test)  # -1 = anomaly (fraud), 1 = normal
```

---

## 🔨 Step-by-Step Build Guide

### Step 1: Get the Dataset
- Kaggle: search **"Credit Card Fraud Detection"** by ULB
- File: `creditcard.csv` (143 MB)
- Columns: V1-V28 (PCA-transformed features), Amount, Time, Class

### Step 2: Explore the Data
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv("data/creditcard.csv")

# Check class imbalance
fraud_count = df["Class"].value_counts()
print(fraud_count)
# 0: 284315, 1: 492

# Plot
plt.figure(figsize=(6,4))
fraud_count.plot(kind="bar", color=["steelblue", "crimson"])
plt.title("Class Distribution: Fraud vs Legit")
plt.xticks([0,1], ["Legit", "Fraud"], rotation=0)
plt.savefig("reports/class_imbalance.png")

# Fraud amounts vs legit amounts
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))
df[df["Class"]==0]["Amount"].hist(ax=ax1, bins=50, color="steelblue")
ax1.set_title("Legit Transaction Amounts")
df[df["Class"]==1]["Amount"].hist(ax=ax2, bins=50, color="crimson")
ax2.set_title("Fraud Transaction Amounts")
plt.savefig("reports/amount_distribution.png")
```

### Step 3: Preprocess
```python
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

def preprocess(df):
    # Scale Amount and Time (V1-V28 are already scaled)
    scaler = StandardScaler()
    df["Amount_Scaled"] = scaler.fit_transform(df[["Amount"]])
    df["Time_Scaled"] = scaler.fit_transform(df[["Time"]])

    # Drop original columns
    df = df.drop(columns=["Amount", "Time"])

    X = df.drop(columns=["Class"])
    y = df["Class"]

    return train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)
```

### Step 4: Apply SMOTE and Train
```python
from imblearn.over_sampling import SMOTE
from sklearn.ensemble import RandomForestClassifier
from xgboost import XGBClassifier

def train_with_smote(X_train, y_train):
    # Apply SMOTE
    sm = SMOTE(random_state=42)
    X_res, y_res = sm.fit_resample(X_train, y_train)
    print(f"After SMOTE: {y_res.value_counts().to_dict()}")

    # Train Random Forest
    rf = RandomForestClassifier(n_estimators=100, random_state=42, n_jobs=-1)
    rf.fit(X_res, y_res)

    # Train XGBoost
    xgb = XGBClassifier(
        scale_pos_weight=len(y_train[y_train==0])/len(y_train[y_train==1]),
        random_state=42, eval_metric="logloss"
    )
    xgb.fit(X_train, y_train)  # XGBoost handles imbalance with scale_pos_weight

    return rf, xgb
```

### Step 5: Evaluate
```python
from sklearn.metrics import classification_report, confusion_matrix
import seaborn as sns

def evaluate_fraud_model(model, X_test, y_test, name):
    y_pred = model.predict(X_test)

    print(f"\n=== {name} ===")
    print(classification_report(y_test, y_pred, target_names=["Legit", "Fraud"]))

    # Confusion Matrix
    cm = confusion_matrix(y_test, y_pred)
    plt.figure(figsize=(6,5))
    sns.heatmap(cm, annot=True, fmt="d", cmap="Blues",
                xticklabels=["Legit", "Fraud"],
                yticklabels=["Legit", "Fraud"])
    plt.title(f"Confusion Matrix — {name}")
    plt.ylabel("Actual")
    plt.xlabel("Predicted")
    plt.savefig(f"reports/confusion_matrix_{name}.png")
    plt.close()

    # Key numbers from confusion matrix:
    tn, fp, fn, tp = cm.ravel()
    print(f"Real fraud caught: {tp} / {tp+fn} ({tp/(tp+fn)*100:.1f}%)")
    print(f"False alarms: {fp}")
```

### Step 6: Bonus — Real-Time Scoring Function
```python
import pickle
import pandas as pd

# Load saved model
with open("models/fraud_model.pkl", "rb") as f:
    model = pickle.load(f)

def score_transaction(transaction_dict):
    """
    Given a new transaction, return fraud probability
    transaction_dict: {"V1": -1.35, "V2": -0.07, ..., "Amount_Scaled": 0.24}
    """
    df = pd.DataFrame([transaction_dict])
    prob = model.predict_proba(df)[0][1]  # probability of fraud

    if prob > 0.7:
        status = "🚨 HIGH RISK — BLOCK"
    elif prob > 0.3:
        status = "⚠️ MEDIUM RISK — REVIEW"
    else:
        status = "✅ LOW RISK — APPROVE"

    return {"fraud_probability": round(prob, 4), "status": status}
```

---

## 📊 What Good Results Look Like

| Metric | Target |
|---|---|
| Fraud Recall | > 80% |
| Precision (Fraud) | > 70% |
| F1 Score (Fraud) | > 0.75 |
| False Alarms per 1000 txns | < 5 |

---

## ✅ Checklist

- [ ] Dataset downloaded
- [ ] Class imbalance visualised
- [ ] SMOTE applied and verified
- [ ] At least 2 models trained
- [ ] Confusion matrix generated
- [ ] Real-time scoring function working
- [ ] Pushed to GitHub

---

## 💼 How to Explain This in an Interview

> *"I built a fraud detection system on the Kaggle Credit Card dataset which has a severe class imbalance — only 0.17% fraud. I used SMOTE oversampling to balance the training set and trained both a Random Forest and XGBoost classifier. My best model achieved 83% recall on fraud cases with only 4 false alarms per 1000 transactions. I also built a real-time scoring function that takes a new transaction and outputs a risk level. This is directly aligned with how JPMorgan Chase's fraud monitoring systems work at scale."*

---

## 🔗 Resources
- Kaggle: "Credit Card Fraud Detection" by Machine Learning Group - ULB
- YouTube: "Fraud Detection with Python" by Data School
- Article: "SMOTE Explained" on Towards Data Science
- pip install: `imbalanced-learn xgboost`

---
**Previous →** [[P2 - Loan Default Prediction]]
**Next →** [[P4 - Mortgage Market Analytics]]
