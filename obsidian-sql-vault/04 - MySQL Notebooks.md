# 04 — MySQL Notebooks (Workbench, DBeaver, Jupyter)

#beginner #tools

---

## 🧠 Introduction

A "notebook" or SQL client is the tool you use to **write, run, and visualize** SQL queries. Choosing the right tool boosts your productivity significantly.

---

## 🛠️ Tool Comparison

| Tool | Type | Best For | Cost |
|------|------|----------|------|
| **MySQL Workbench** | GUI Desktop | MySQL-specific work | Free |
| **DBeaver** | GUI Desktop | Multiple DB types | Free/Pro |
| **DataGrip** | GUI Desktop | Professional IDE experience | Paid |
| **Jupyter Notebook** | Web Notebook | Python + SQL combined | Free |
| **VS Code + Extension** | Code Editor | Developers | Free |
| **TablePlus** | GUI Desktop | Clean UI, fast queries | Free/Paid |

---

## 🔧 MySQL Workbench

**Official MySQL GUI tool.**

### Key Features
- Query Editor with syntax highlighting
- Schema / ER Diagram viewer
- Data Import/Export wizard
- Performance Dashboard
- Server Administration panel

### Shortcuts to Know

```
Ctrl + Enter    → Execute current query
Ctrl + Shift+Enter → Execute entire script
Ctrl + /        → Comment/uncomment line
F5              → Refresh schema
```

### Useful Settings
```
Edit → Preferences → SQL Editor
  ✅ Safe Updates — prevents accidental mass UPDATE/DELETE
  Query Timeout → set to 60s for large datasets
```

---

## 🔧 DBeaver (Highly Recommended)

**Best free universal database tool.** Supports MySQL, PostgreSQL, SQLite, MongoDB, Oracle, and 80+ databases — essential for Data Engineers who work across multiple systems.

### Installation

```bash
# Ubuntu
sudo snap install dbeaver-ce

# macOS
brew install --cask dbeaver-community

# Windows: Download from https://dbeaver.io
```

### Connect to MySQL in DBeaver

```
New Connection → MySQL
  Host: localhost
  Port: 3306
  Database: (leave blank for all)
  Username: root
  Password: ****
→ Test Connection → Finish
```

### Key Features
- SQL auto-complete
- ER Diagram auto-generation
- Data export to CSV, Excel, JSON
- Query history
- SSH tunnel support (critical for production DBs)

---

## 🐍 Jupyter Notebook + SQL (Data Engineering Combo)

Perfect for combining **Python + SQL** in a data pipeline context.

### Setup

```bash
pip install jupyter notebook ipython-sql sqlalchemy pymysql
jupyter notebook
```

### Usage in Jupyter

```python
# Load SQL magic
%load_ext sql

# Connect to MySQL
%sql mysql+pymysql://root:password@localhost/mydb

# Run SQL inline
%sql SELECT * FROM employees LIMIT 5;
```

```python
# Multi-line SQL
%%sql
SELECT 
    department,
    COUNT(*) as headcount,
    AVG(salary) as avg_salary
FROM employees
GROUP BY department;
```

---

## 📝 VS Code Setup

```
Extensions to install:
1. MySQL (by Weijan Chen) — run queries directly
2. SQLTools — multi-database support
3. SQLTools MySQL/MariaDB driver
```

---

## 💼 Interview Questions

1. **What tools do you use for SQL development?**
   > Mention DBeaver or MySQL Workbench for daily use; Jupyter for Python + SQL notebooks in data pipelines.

2. **How do you connect to a remote database securely?**
   > Use SSH tunneling — DBeaver supports it natively. Never expose port 3306 publicly.

3. **What is an ER Diagram and how do you generate one?**
   > Entity-Relationship Diagram shows tables and their relationships. MySQL Workbench and DBeaver can auto-generate them from existing schemas.

---

## 💡 Tips & Best Practices

- Use **DBeaver** if you work with multiple database types (very common in Data Engineering)
- Always enable **Safe Updates** in MySQL Workbench to prevent accidental data loss
- Use **Jupyter** when you need to document your SQL analysis with explanations
- Enable **query history** in your tool — you'll thank yourself later

---

## 🔗 Related Topics

- [[02 - MySQL Installation Windows]] — Install MySQL first
- [[03 - MySQL Installation Linux]] — Linux setup
- [[06 - SQL Basic Commands CRUD]] — Start writing queries
- [[33 - Python JDBC Connectivity]] — Connecting Python to MySQL programmatically
