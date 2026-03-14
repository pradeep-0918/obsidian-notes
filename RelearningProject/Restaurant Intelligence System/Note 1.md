# 🍽️ Restaurant Intelligence System — Project Sprint

## 📌 Project Goal

Build an **end-to-end data + machine learning system** that analyzes restaurant data and provides insights.

This project demonstrates skills required for:

- Data Engineering
    
- Machine Learning
    
- SQL Analytics
    
- Data Pipelines
    
- Dashboard Development
    
- Real-world system design

Final deliverables:

- Cleaned dataset
    
- Exploratory data analysis
    
- SQL analysis queries
    
- ML model predicting restaurant ratings
    
- Restaurant recommendation system
    
- Streamlit dashboard
    
- Clean GitHub repository

---

# 🧠 Skills Covered

### Data Engineering

- Data ingestion
    
- Data cleaning pipelines
    
- Feature engineering
    
- Data storage

### Machine Learning

- Regression models
    
- Model evaluation
    
- Feature selection


### Analytics

- Exploratory Data Analysis (EDA)
    
- Visualization
    
- SQL queries


### Deployment

- Streamlit dashboard
    
- GitHub version control

---

# 🏗️ System Architecture

Raw Data  
↓  
Data Cleaning Pipeline  
↓  
Exploratory Data Analysis  
↓  
SQL Analysis  
↓  
Feature Engineering  
↓  
Machine Learning Model  
↓  
Recommendation System  
↓  
Streamlit Dashboard

---

# 📅 2-Day Sprint Plan

## Day 1

- Project setup
    
- Dataset loading
    
- Data cleaning
    
- Exploratory data analysis
    
- SQL analysis

## Day 2

- Feature engineering
    
- Machine learning model
    
- Recommendation system
    
- Streamlit dashboard
    
- GitHub polishing


---

# 🗂️ Project Folder Structure

```
restaurant-intelligence-system
│
├── data
│   ├── raw
│   └── processed
│
├── notebooks
│
├── src
│   ├── data
│   ├── features
│   ├── models
│   └── visualization
│
├── dashboard
│
├── tests
│
├── requirements.txt
├── README.md
└── .gitignore
```

---

# ⚙️ STEP 1 — Project Setup

## 1️⃣ Create Project Folder

Open terminal and run:

```
mkdir restaurant-intelligence-system
cd restaurant-intelligence-system
```

Expected result:

```
restaurant-intelligence-system/
```

---

## 2️⃣ Initialize Git

```
git init
```

Expected output:

```
Initialized empty Git repository
```

---

## 3️⃣ Create Virtual Environment

```
python -m venv venv
```

Activate environment:

```
venv\Scripts\activate
```

Expected output:

```
(venv)
```

---

## 4️⃣ Open Project in PyCharm

Open PyCharm → Open Project → Select:

```
restaurant-intelligence-system
```

Set interpreter:

```
venv\Scripts\python.exe
```

---

## 5️⃣ Create Folder Structure

Create the following folders:

```
data
data/raw
data/processed
notebooks
src
src/data
src/features
src/models
src/visualization
dashboard
tests
```

---

## 6️⃣ Create Project Files

Create empty files:

```
requirements.txt
README.md
.gitignore
```

---

## 7️⃣ Add .gitignore

```
venv/
__pycache__/
*.pyc
.ipynb_checkpoints
data/processed
```

---

## 8️⃣ Install Dependencies

Run in terminal:

```
pip install pandas numpy matplotlib seaborn scikit-learn streamlit jupyter sqlalchemy
```

Save dependencies:

```
pip freeze > requirements.txt
```

Expected output:

```
requirements.txt updated
```

---

## 9️⃣ First Git Commit

```
git add .
git commit -m "Initial project setup"
```

Expected output:

```
X files changed
```

---

## 🔗 Connect GitHub Repository

Create repo on GitHub:

```
restaurant-intelligence-system
```

Run:

```
git remote add origin git@github.com:YOUR_USERNAME/restaurant-intelligence-system.git
git branch -M main
git push -u origin main
```

Expected output:

```
branch 'main' set up to track 'origin/main'
```

---

# 📊 Progress Tracker

```
[ ] Project folder created
[ ] Git initialized
[ ] Virtual environment created
[ ] Project opened in PyCharm
[ ] Folder structure created
[ ] Dependencies installed
[ ] First commit pushed to GitHub
```

---

# 🧾 Git Commit Log

Use this section to track commits.

Example:

```
commit 1
Initial project setup

commit 2
Added dataset

commit 3
Implemented data cleaning pipeline

commit 4
EDA analysis
```

---

# 📁 Expected Final Project Structure

```
restaurant-intelligence-system
│
├── data
│   ├── raw
│   └── processed
│
├── notebooks
├── src
├── dashboard
├── tests
│
├── requirements.txt
├── README.md
└── .gitignore
```

---