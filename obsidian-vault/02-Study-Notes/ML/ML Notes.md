# 📚 ML Notes — Fundamentals for Finance Roles

---

## 🤖 The ML Workflow (Follow This Every Time)

```
1. Define Problem     → What are you predicting? (classification or regression?)
2. Get Data          → CSV, API, database
3. Explore (EDA)     → Shape, nulls, distributions, correlations
4. Preprocess        → Clean, encode, scale, split
5. Train Models      → Start simple, then complex
6. Evaluate          → Accuracy, AUC, F1 — pick right metric for the problem
7. Improve           → Tune hyperparameters, try new features
8. Deploy (bonus)    → Flask API or save model with pickle
```

---

## 📐 Types of ML Problems

| Type | Output | Example | Algorithm |
|---|---|---|---|
| Classification | Category | Default: Yes/No | Logistic Reg, RF, XGBoost |
| Regression | Number | House price: $350K | Linear Reg, RF, XGBoost |
| Clustering | Groups | Customer segments | K-Means |
| Anomaly Detection | Normal/Outlier | Fraud detection | Isolation Forest |
| NLP | Text sentiment | Positive/Negative | VADER, FinBERT |

---

## 🔧 Preprocessing Pipeline

### Encoding Categorical Variables
```python
from sklearn.preprocessing import LabelEncoder, OneHotEncoder
import pandas as pd

# Label Encoding — for ordinal categories (has order)
# e.g., 'Poor' < 'Fair' < 'Good' < 'Excellent'
le = LabelEncoder()
df["credit_tier_encoded"] = le.fit_transform(df["credit_tier"])

# One-Hot Encoding — for nominal categories (no order)
# e.g., loan_type: 'Fixed', 'ARM', 'FHA'
df = pd.get_dummies(df, columns=["loan_type", "state"], drop_first=True)
```

### Scaling Features
```python
from sklearn.preprocessing import StandardScaler, MinMaxScaler

# StandardScaler — mean=0, std=1 (use for most ML)
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)  # fit AND transform train
X_test_scaled = scaler.transform(X_test)         # ONLY transform test (never fit!)

# MinMaxScaler — scales to 0-1 range
mm = MinMaxScaler()
X_scaled = mm.fit_transform(X)

# IMPORTANT: NEVER fit the scaler on test data
# This causes "data leakage" — your model sees test data during training
```

### Train/Test Split
```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,       # 80% train, 20% test
    random_state=42,     # reproducible results
    stratify=y           # keep same class proportion in train and test
)
```

---

## 🌳 Models — When to Use What

### Logistic Regression
```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(max_iter=1000, C=1.0)
model.fit(X_train, y_train)
y_pred = model.predict(X_test)
y_prob = model.predict_proba(X_test)[:, 1]  # probability of class 1

# Use when:
# → You need an explainable model (for regulators/compliance)
# → Good baseline to beat
# → Linear relationship between features and outcome
```

### Random Forest
```python
from sklearn.ensemble import RandomForestClassifier

rf = RandomForestClassifier(
    n_estimators=100,    # number of trees
    max_depth=10,        # max depth per tree (prevents overfitting)
    random_state=42,
    n_jobs=-1            # use all CPU cores
)
rf.fit(X_train, y_train)

# Feature importance
import pandas as pd
feat_imp = pd.Series(rf.feature_importances_, index=X_train.columns)
feat_imp.sort_values(ascending=False).head(10)

# Use when:
# → Works well out of the box
# → Handles missing values and non-linear relationships
# → Good for tabular data like loans/transactions
```

### XGBoost
```python
from xgboost import XGBClassifier

xgb = XGBClassifier(
    n_estimators=200,
    max_depth=6,
    learning_rate=0.1,
    subsample=0.8,
    colsample_bytree=0.8,
    random_state=42,
    eval_metric="logloss"
)
xgb.fit(X_train, y_train,
        eval_set=[(X_test, y_test)],
        verbose=False)

# Use when:
# → You want best accuracy on tabular data
# → Industry standard for credit scoring, fraud detection
# → Competition-winning algorithm
```

---

## 📏 Evaluation Metrics

### Classification Metrics
```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score,
    f1_score, roc_auc_score, classification_report, confusion_matrix
)

# Core metrics
acc = accuracy_score(y_test, y_pred)
prec = precision_score(y_test, y_pred)
rec = recall_score(y_test, y_pred)
f1 = f1_score(y_test, y_pred)
auc = roc_auc_score(y_test, y_prob)

print(classification_report(y_test, y_pred, target_names=["No Default", "Default"]))
```

### Understanding the Confusion Matrix
```
                  Predicted No    Predicted Yes
Actual No     |  True Negative  |  False Positive  |
Actual Yes    |  False Negative |  True Positive   |

True Positive (TP)  = Correctly predicted default (caught fraud!)
True Negative (TN)  = Correctly predicted no default (not fraud)
False Positive (FP) = Wrongly flagged as default (false alarm)
False Negative (FN) = Missed a real default (dangerous!)

Precision = TP / (TP + FP)   → How precise are your positive predictions?
Recall    = TP / (TP + FN)   → How many real positives did you catch?
F1 Score  = 2 * (Precision * Recall) / (Precision + Recall)
```

### ROC-AUC Explained
```
ROC-AUC = Area Under the ROC Curve
Range: 0.5 (random guessing) to 1.0 (perfect)

0.5 = No better than a coin flip
0.7 = OK
0.8 = Good
0.9 = Excellent
1.0 = Suspicious (likely overfitting)

In finance, AUC > 0.75 on credit data is considered good.
```

---

## 🎛️ Hyperparameter Tuning

```python
from sklearn.model_selection import GridSearchCV

param_grid = {
    "n_estimators": [100, 200, 300],
    "max_depth": [5, 10, None],
    "min_samples_split": [2, 5, 10]
}

grid = GridSearchCV(
    RandomForestClassifier(random_state=42),
    param_grid,
    cv=5,                 # 5-fold cross validation
    scoring="roc_auc",
    n_jobs=-1,
    verbose=1
)
grid.fit(X_train, y_train)

print(f"Best params: {grid.best_params_}")
print(f"Best AUC: {grid.best_score_:.4f}")
```

---

## 💾 Saving & Loading Models

```python
import pickle

# Save
with open("models/model.pkl", "wb") as f:
    pickle.dump(model, f)

# Load
with open("models/model.pkl", "rb") as f:
    loaded_model = pickle.load(f)

# Predict with loaded model
y_pred = loaded_model.predict(X_new)
```

---
**Back to Home →** [[Dashboard]]
**Next Study Note →** [[Data Engineering Notes]]
