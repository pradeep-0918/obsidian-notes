# Ubuntu WSL Setup for AI / Data Engineering Lab

## Overview
This guide explains how to install **Ubuntu Linux using WSL2 on Windows** and configure it for **Python, Machine Learning, and Data Engineering development**.

Environment Goal:

Windows → WSL2 → Ubuntu → Python → Conda → Docker → Spark → Jupyter

---

# 1. Install WSL and Ubuntu

## Step 1: Open PowerShell as Administrator

Run:

wsl --install

This will automatically install:
- WSL2
- Ubuntu
- Linux Kernel

After installation **restart your computer**.

---

# 2. First Ubuntu Setup

Open **Ubuntu** from the Start Menu.

You will see:

Enter new UNIX username:

Example:

pradeep

Then create a password.

Note:
Password will **not appear while typing**. This is normal.

---

# 3. Update Ubuntu System

Run the following commands:

sudo apt update
sudo apt upgrade -y

Purpose:
- Updates system packages
- Installs latest security updates

---

# 4. Install Essential Developer Tools

Run:

sudo apt install build-essential git curl wget unzip software-properties-common -y

This installs:

build-essential → compilers and development tools  
git → version control  
curl → download tool  
wget → file downloader  
unzip → extract files  

Check Git:

git --version

---

# 5. Install Python Environment

Install Python and pip:

sudo apt install python3 python3-pip python3-venv -y

Verify installation:

python3 --version
pip3 --version

---

# 6. Install Miniconda (Recommended for ML)

Miniconda helps manage Python environments.

Download installer:

wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

Run installer:

bash Miniconda3-latest-Linux-x86_64.sh

Press:

ENTER  
yes  
yes  

Reload environment:

source ~/.bashrc

Verify installation:

conda --version

---

# 7. Create ML Development Environment

Create environment:

conda create -n ml_env python=3.10

Activate environment:

conda activate ml_env

---

# 8. Install Data Science Libraries

Install common ML libraries:

pip install numpy pandas matplotlib seaborn scikit-learn

Libraries:

numpy → numerical computing  
pandas → data manipulation  
matplotlib → plotting  
seaborn → visualization  
scikit-learn → machine learning  

---

# 9. Install Jupyter Lab

Install:

pip install jupyterlab

Run:

jupyter lab

This opens **Jupyter Lab in browser**.

Used for:
- ML experiments
- Data analysis
- notebooks

---

# 10. Install PySpark (Important for Data Engineers)

Install PySpark:

pip install pyspark

Test Spark:

python

Then run:

from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("test").getOrCreate()

---

# 11. Install Node.js (Optional but useful)

sudo apt install nodejs npm -y

Check version:

node -v
npm -v

---

# 12. Install Docker (Important for Data Engineering)

Install Docker:

sudo apt install docker.io -y

Start Docker service:

sudo service docker start

Check installation:

docker --version

Docker is used for:

- Apache Airflow
- Kafka
- Spark clusters
- Data pipelines

---

# 13. Access Windows Files from Ubuntu

Windows drives are mounted inside:

/mnt/

Example:

cd /mnt/c

This gives access to:

C drive  
Documents  
Projects  

Example:

cd /mnt/c/Users

---

# 14. Recommended Development Workflow

Typical workflow:

1. Write code in PyCharm or VS Code
2. Run scripts inside Ubuntu terminal
3. Use Conda environments for ML projects
4. Use Docker for data engineering tools

---

# 15. Final Development Architecture

Windows
│
├── WSL2
│    └── Ubuntu
│         ├── Python
│         ├── Conda
│         ├── Docker
│         ├── PySpark
│         ├── Jupyter Lab
│         └── ML Libraries
│
├── PyCharm
└── GitHub

---

# 16. Future Tools to Install

Next tools for Data Engineering:

Apache Spark  
Apache Kafka  
Apache Airflow  
PostgreSQL  
Redis  

These will help build **real data pipelines and ML systems**.

---

# 17. Project Ideas Using This Setup

1. Restaurant Recommendation System
2. Real-time Data Pipeline with Kafka
3. ML Model Training System
4. AI Course Recommendation Engine
5. Data Analytics Dashboard

These projects help build **strong portfolio for Data Engineer / ML Engineer roles**.

---