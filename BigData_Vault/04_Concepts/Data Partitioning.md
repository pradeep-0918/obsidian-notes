# 🗂️ Data Partitioning — Divide to Conquer

#distributed-systems #partitioning #sharding #concepts

---

## 🍅 Pomodoro 8B — How Big Data Gets Divided

---

## 🧒 Feynman Explanation — The Pizza Store Chain

Imagine a pizza chain with 100 stores. All orders go to ONE central kitchen. That kitchen collapses under load.

Solution: **Partition by geography.** Orders from North Chennai → Kitchen A. South Chennai → Kitchen B. Each kitchen handles only its partition.

**Data partitioning = dividing data so each machine handles only its share.**

---

## 🔪 Types of Partitioning

### 1. Horizontal Partitioning (Sharding)

Split **rows** of a table across machines.

```
Full User Table:
| UserID | Name     | City     |
|--------|----------|----------|
| 1      | Pradeep  | Salem    |
| 2      | Anitha   | Chennai  |
| 3      | Ravi     | Coimbatore|

Shard 1 (Users 1-1000): | 1 | Pradeep | Salem |
Shard 2 (Users 1001-2000): ...
```

**Used by:** Most NoSQL databases (Cassandra, MongoDB, HBase)

### 2. Vertical Partitioning

Split **columns** across machines.

```
Machine A: UserID, Name, Email     (frequently accessed)
Machine B: UserID, Address, Bio    (rarely accessed)
```

**Used by:** Column-oriented stores (Parquet, ORC, Cassandra)

### 3. Functional Partitioning (Microservices)

Split data by **function/domain.**

```
User Service DB: users, profiles
Order Service DB: orders, payments
Product Service DB: inventory, catalog
```

---

## 🔑 Partitioning Strategies

### Hash Partitioning

```
Partition = hash(partitionKey) % numberOfPartitions

Example: hash("user_101") % 4 = 2 → goes to Partition 2
```

✅ Even distribution
❌ Range queries cross partitions

### Range Partitioning

```
Partition 1: A-G
Partition 2: H-N
Partition 3: O-Z
```

✅ Range queries efficient
❌ Can lead to hotspots (e.g., 70% of users have names starting with A-C)

### List Partitioning

```
Partition 1: Tamil Nadu, Kerala
Partition 2: Karnataka, Andhra Pradesh
Partition 3: Maharashtra, Goa
```

✅ Logical grouping
❌ Must know groups in advance

---

## ⚠️ The Hotspot Problem

A **hotspot** is when one partition receives disproportionate traffic.

**Example:** An e-commerce site partitioned by user last name. If "Sharma" is the most common surname in India, the "S" partition gets hammered while others sit idle.

**Fix:** Use a compound partition key or add a random salt to the key.

---

## 📌 HDFS Partitioning

In HDFS, files are split into **128MB blocks** — this is horizontal partitioning at the file level. Each block is stored on a different DataNode.

In Hive, tables can be partitioned by columns:

```sql
CREATE TABLE sales (
  product_id INT,
  amount DOUBLE
)
PARTITIONED BY (year INT, month INT);
```

This creates folders: `sales/year=2024/month=01/`, `sales/year=2024/month=02/`, etc. — allowing Hive to skip irrelevant partitions during queries (**partition pruning**).

---

## 🔗 Connected Notes

- [[CAP Theorem]] — Why partitioning creates trade-offs
- [[HDFS Deep Dive]] — HDFS block partitioning
- [[Top 30 Interview QA]] — Sharding interview questions
