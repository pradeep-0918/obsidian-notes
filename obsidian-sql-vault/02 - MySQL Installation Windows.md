# 02 — MySQL Installation (Windows)

#beginner #setup

---

## 🧠 Introduction

Getting MySQL running on Windows is your first hands-on step. This guide covers the **MySQL Community Server** (free) installation — the same version used in most company dev environments.

---

## 📥 Step-by-Step Installation

### Step 1: Download MySQL Installer

1. Go to: `https://dev.mysql.com/downloads/installer/`
2. Download **MySQL Installer for Windows** (choose the larger offline installer ~450MB)
3. No Oracle account needed — click "No thanks, just start my download"

### Step 2: Run the Installer

```
Double-click → mysql-installer-community-X.X.X.msi
```

Choose **"Developer Default"** setup type — installs:
- MySQL Server
- MySQL Workbench (GUI tool)
- MySQL Shell
- Connectors (Python, Java, etc.)

### Step 3: Check Requirements

- Installer will check for Visual C++ Redistributable
- Click **Execute** to install missing requirements automatically

### Step 4: Configure MySQL Server

```
Type and Networking:
  Config Type → Development Computer
  Port        → 3306 (default, keep this)
  
Authentication:
  Choose → Use Strong Password Encryption (recommended)
  
Root Password:
  Set a strong password — REMEMBER THIS!
  
Windows Service:
  Service Name → MySQL80
  Start at boot → ✅ Yes
```

### Step 5: Apply Configuration

Click **Execute** → **Finish** → **Next** → **Finish**

---

## ✅ Verify Installation

Open **Command Prompt** as Administrator:

```bash
mysql -u root -p
```

Enter your password. You should see:

```
Welcome to the MySQL monitor. Commands end with ; or \g.
mysql>
```

---

## 🔧 Common Commands After Install

```sql
-- Check MySQL version
SELECT VERSION();

-- Show all databases
SHOW DATABASES;

-- Create your first database
CREATE DATABASE mydb;

-- Use it
USE mydb;
```

---

## 🛠️ MySQL Workbench (GUI)

After installation, open **MySQL Workbench**:
1. Click the **`+`** icon next to "MySQL Connections"
2. Connection Name: `Local MySQL`
3. Hostname: `127.0.0.1`, Port: `3306`
4. Username: `root`
5. Test Connection → Enter password → OK

> Workbench is your primary GUI tool for writing and running SQL queries visually.

---

## 💼 Interview Questions

1. **What is the default port for MySQL?**
   > 3306

2. **What is the difference between MySQL Server and MySQL Workbench?**
   > MySQL Server is the database engine; Workbench is the GUI client used to connect and run queries.

3. **How do you connect to MySQL from the command line?**
   > `mysql -u root -p` then enter password

---

## ⚠️ Common Mistakes

- Forgetting the root password — store it in a password manager immediately
- Not starting the MySQL service — check Services (`services.msc`) if connection fails
- Firewall blocking port 3306 — add exception if connecting remotely

---

## 🔗 Related Topics

- [[03 - MySQL Installation Linux]] — Linux alternative setup
- [[04 - MySQL Notebooks]] — Tools to write SQL
- [[06 - SQL Basic Commands CRUD]] — Start writing queries
