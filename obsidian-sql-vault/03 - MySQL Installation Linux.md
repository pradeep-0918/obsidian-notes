# 03 — MySQL Installation (Linux)

#beginner #setup

---

## 🧠 Introduction

Linux is the **preferred environment** for Data Engineers. Production servers, cloud VMs, and Docker containers almost always run Linux. Knowing how to install and manage MySQL on Linux is essential.

---

## 📦 Installation on Ubuntu / Debian

### Method 1: APT Package Manager (Recommended)

```bash
# Step 1: Update package index
sudo apt update

# Step 2: Install MySQL Server
sudo apt install mysql-server -y

# Step 3: Check MySQL status
sudo systemctl status mysql

# Step 4: Secure the installation (important!)
sudo mysql_secure_installation
```

**During `mysql_secure_installation`:**
- Set root password → Yes
- Remove anonymous users → Yes
- Disallow remote root login → Yes (for security)
- Remove test database → Yes
- Reload privilege tables → Yes

### Method 2: Official MySQL APT Repository

```bash
# Download MySQL APT config package
wget https://dev.mysql.com/get/mysql-apt-config_0.8.29-1_all.deb

# Install the config
sudo dpkg -i mysql-apt-config_0.8.29-1_all.deb

# Update and install
sudo apt update
sudo apt install mysql-server -y
```

---

## 📦 Installation on CentOS / RHEL / Fedora

```bash
# Install using dnf
sudo dnf install mysql-server -y

# Start the service
sudo systemctl start mysqld
sudo systemctl enable mysqld

# Get temporary root password (RHEL sets one automatically)
sudo grep 'temporary password' /var/log/mysqld.log

# Secure installation
sudo mysql_secure_installation
```

---

## 🔑 Connect to MySQL on Linux

```bash
# Connect as root
sudo mysql -u root -p

# Or without sudo (after setting password)
mysql -u root -p
```

---

## ⚙️ Essential Service Commands

```bash
# Start MySQL
sudo systemctl start mysql

# Stop MySQL
sudo systemctl stop mysql

# Restart MySQL
sudo systemctl restart mysql

# Enable at boot
sudo systemctl enable mysql

# Check status
sudo systemctl status mysql
```

---

## 🐳 Docker Alternative (Great for Practice)

```bash
# Pull MySQL image
docker pull mysql:8.0

# Run MySQL container
docker run --name mysql-dev \
  -e MYSQL_ROOT_PASSWORD=mypassword \
  -p 3306:3306 \
  -d mysql:8.0

# Connect to container
docker exec -it mysql-dev mysql -u root -p
```

> Docker is the fastest way to spin up a clean MySQL instance for testing and practice.

---

## 🔧 Configuration File Location

```bash
# MySQL config file
/etc/mysql/mysql.conf.d/mysqld.cnf    # Ubuntu
/etc/my.cnf                           # CentOS/RHEL

# View current config
cat /etc/mysql/mysql.conf.d/mysqld.cnf

# Log files
/var/log/mysql/error.log
```

---

## 💼 Interview Questions

1. **Where is MySQL's config file on Ubuntu?**
   > `/etc/mysql/mysql.conf.d/mysqld.cnf`

2. **How do you check if MySQL service is running on Linux?**
   > `sudo systemctl status mysql`

3. **What does `mysql_secure_installation` do?**
   > It removes anonymous users, disables remote root login, removes test databases, and sets root password — hardening the MySQL installation.

4. **Scenario:** *"You need to run MySQL on a cloud VM. What approach do you take?"*
   > Install via package manager, run `mysql_secure_installation`, configure bind-address in mysqld.cnf for remote access, add firewall rule for port 3306.

---

## ⚠️ Common Mistakes

- Not running `mysql_secure_installation` — leaves anonymous users and test database
- Forgetting to enable service with `systemctl enable` — MySQL won't start after reboot
- Binding MySQL to `0.0.0.0` without firewall rules — security risk

---

## 🔗 Related Topics

- [[02 - MySQL Installation Windows]] — Windows setup
- [[04 - MySQL Notebooks]] — GUI tools for SQL
- [[29 - GRANT REVOKE]] — Create secure database users
