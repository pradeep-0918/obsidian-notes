# 📁 Project 4 — Mortgage Market Analytics Dashboard
**Target Companies:** Mr. Cooper (primary)
**Duration:** 2 weeks | **Difficulty:** ⭐⭐ Beginner-Friendly

---

## 🎯 What You're Building

An interactive **analytics dashboard** that analyses the US mortgage market using real Fannie Mae data. It will show:
- Loan volume trends over time
- Default rates by state and loan type
- Interest rate impact on origination
- Borrower profile breakdowns (LTV, DTI, credit score)

> Mr. Cooper's data teams produce exactly these reports to track their loan portfolio health. This project will make you look like you already know their business.

---

## 📚 Concepts to Study First (Week 1, Days 1–3)

### Key Mortgage Terms You Must Know

| Term | Meaning | Why It Matters |
|---|---|---|
| **LTV** | Loan-to-Value ratio = Loan / Property Value | Higher LTV = riskier loan |
| **DTI** | Debt-to-Income ratio = Monthly Debt / Monthly Income | Higher DTI = harder to repay |
| **FICO** | Credit score (300–850) | Lower score = higher default risk |
| **Origination** | When a loan is first issued | Volume of new loans |
| **Delinquency** | Missed payment (30/60/90 days late) | Early sign of default |
| **Servicer** | Company collecting monthly payments | That's Mr. Cooper's job |
| **Prepayment** | Paying off loan early | Happens when rates drop & people refinance |

### Pandas for Time Series Data
```python
import pandas as pd

# Parse dates
df["orig_date"] = pd.to_datetime(df["orig_date"])

# Set date as index
df = df.set_index("orig_date")

# Resample by month or quarter
monthly_volume = df["loan_amount"].resample("M").sum()
quarterly_defaults = df["default_flag"].resample("Q").mean()

# Rolling average (12-month)
df["rolling_avg_rate"] = df["interest_rate"].rolling(window=12).mean()
```

### Groupby for Segmentation
```python
# Average credit score by state
by_state = df.groupby("state")["credit_score"].mean().sort_values(ascending=False)

# Default rate by loan type
df["default_rate"] = df.groupby("loan_type")["default_flag"].transform("mean")

# Multiple aggregations
summary = df.groupby("loan_purpose").agg(
    total_volume=("loan_amount", "sum"),
    avg_fico=("credit_score", "mean"),
    avg_ltv=("ltv", "mean"),
    default_rate=("default_flag", "mean")
).round(3)
```

### Matplotlib + Seaborn Charting
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Time series line chart
plt.figure(figsize=(12, 5))
monthly_volume.plot(color="steelblue", linewidth=2)
plt.title("Monthly Mortgage Origination Volume")
plt.ylabel("Total Loan Volume ($)")
plt.xlabel("Date")
plt.grid(alpha=0.3)
plt.tight_layout()
plt.savefig("dashboard/origination_trend.png")

# Heatmap (default rate by state)
pivot = df.groupby(["year", "state"])["default_flag"].mean().unstack()
plt.figure(figsize=(16, 8))
sns.heatmap(pivot, cmap="RdYlGn_r", linewidths=0.3, fmt=".2%")
plt.title("Mortgage Default Rate by State and Year")
plt.savefig("dashboard/default_heatmap.png")

# Scatter plot (FICO vs LTV)
plt.figure(figsize=(8, 6))
plt.scatter(df["credit_score"], df["ltv"], alpha=0.1, c=df["default_flag"], cmap="coolwarm")
plt.colorbar(label="Default (1=Yes)")
plt.xlabel("Credit Score (FICO)")
plt.ylabel("LTV Ratio")
plt.title("Credit Score vs LTV — Coloured by Default")
plt.savefig("dashboard/fico_ltv_scatter.png")
```

---

## 🔨 Step-by-Step Build Guide

### Step 1: Get the Data

**Option A (Best): Fannie Mae Single Family Loan Data**
- URL: fanniemae.com/research-and-insights/datasets/single-family-loan-performance
- Free but requires registration
- Contains: origination data + quarterly performance updates

**Option B (Easier to Start): Kaggle**
- Search "Freddie Mac Loan Level Dataset" or "Fannie Mae Mortgage"
- Or use "US Mortgage Market" datasets

### Step 2: Clean and Prepare
```python
import pandas as pd

# Fannie Mae columns (abbreviated)
cols = [
    "credit_score", "first_payment_date", "first_time_homebuyer",
    "maturity_date", "msa", "mip", "units", "occupancy_status",
    "orig_cltv", "orig_dti", "orig_upb", "orig_ltv", "orig_interest_rate",
    "channel", "ppm_flag", "product_type", "state", "property_type",
    "postal_code", "loan_sequence_number", "loan_purpose",
    "orig_loan_term", "num_borrowers", "seller_name", "servicer_name"
]

df = pd.read_csv("data/sample_orig.txt", sep="|", header=None, names=cols)

# Clean
df["credit_score"] = pd.to_numeric(df["credit_score"], errors="coerce")
df["orig_ltv"] = pd.to_numeric(df["orig_ltv"], errors="coerce")
df["orig_dti"] = pd.to_numeric(df["orig_dti"], errors="coerce")
df["orig_interest_rate"] = pd.to_numeric(df["orig_interest_rate"], errors="coerce")

# Parse dates
df["first_payment_date"] = pd.to_datetime(df["first_payment_date"], format="%m%Y")
df["year"] = df["first_payment_date"].dt.year
df["quarter"] = df["first_payment_date"].dt.quarter
```

### Step 3: Build Your Analysis Notebook
```python
# 1. Loan Volume Over Time
monthly = df.groupby(df["first_payment_date"].dt.to_period("Q"))["orig_upb"].sum() / 1e9
monthly.plot(figsize=(14, 5), title="Quarterly Mortgage Origination Volume ($B)")
plt.ylabel("Volume (Billions)")
plt.savefig("dashboard/quarterly_volume.png")

# 2. Average Credit Score Over Time
df.groupby("year")["credit_score"].mean().plot(
    kind="bar", figsize=(10, 5),
    title="Average Borrower Credit Score by Year", color="teal"
)
plt.savefig("dashboard/avg_credit_score.png")

# 3. LTV Distribution
df["orig_ltv"].hist(bins=50, figsize=(8,5), color="steelblue", edgecolor="white")
plt.axvline(80, color="red", linestyle="--", label="80% LTV Line")
plt.title("Distribution of Loan-to-Value Ratios")
plt.xlabel("LTV (%)")
plt.legend()
plt.savefig("dashboard/ltv_distribution.png")

# 4. Top States by Loan Volume
top_states = df.groupby("state")["orig_upb"].sum().sort_values(ascending=False).head(10)
top_states.plot(kind="barh", figsize=(10,6), color="navy",
                title="Top 10 States by Mortgage Origination Volume")
plt.savefig("dashboard/top_states.png")

# 5. Interest Rate vs DTI
df.plot.scatter(x="orig_interest_rate", y="orig_dti", alpha=0.05,
                figsize=(8,6), title="Interest Rate vs Debt-to-Income Ratio")
plt.savefig("dashboard/rate_vs_dti.png")
```

### Step 4: Build a Tableau Dashboard (Bonus)
1. Export your cleaned data: `df.to_csv("data/processed/mortgage_clean.csv")`
2. Open **Tableau Public** (free)
3. Connect to your CSV
4. Build these 4 sheets:
   - Line chart: Origination volume over time
   - Map: Average credit score by state
   - Bar chart: Top servicers by volume
   - Scatter: FICO vs LTV
5. Combine into a Dashboard
6. Publish to Tableau Public and share the link on your resume!

---

## 📊 Summary Stats to Include in README

```
Dataset: Fannie Mae Single Family Loans
Records analysed: X,XXX,XXX loans
Time period: XXXX–XXXX
Total origination volume: $XXX billion
Average credit score: XXX
Average LTV: XX%
Average DTI: XX%
States covered: 50
```

---

## ✅ Checklist

- [ ] Dataset downloaded and cleaned
- [ ] 5+ charts generated and saved
- [ ] Time series trend analysis complete
- [ ] State-level analysis complete
- [ ] Tableau dashboard published (optional but impressive)
- [ ] GitHub repo with charts in README

---

## 💼 How to Explain This in an Interview

> *"I built a mortgage market analytics dashboard using Fannie Mae's publicly available single-family loan performance data, covering millions of records. I analysed origination trends, borrower credit profiles, LTV distributions, and geographic default patterns. I then built a Tableau dashboard surfacing these KPIs for executive consumption. This maps directly to how Mr. Cooper's analytics team monitors their $XXX billion servicing portfolio."*

---

## 🔗 Resources
- Fannie Mae Data: fanniemae.com/research-and-insights/datasets
- Tableau Public: public.tableau.com (free)
- YouTube: "Tableau for Beginners" by Kevin Flerlage

---
**Previous →** [[P3 - Fraud Detection System]]
**Next →** [[P5 - Stock Sentiment NLP]]
