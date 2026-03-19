# ⚡ Apache Spark

> **Level:** Intermediate-Advanced | **Stage:** 3 of 5
> Apache Spark is the most widely used big data processing engine — fast, flexible, and the backbone of modern data lakehouse pipelines.

---

## 🤔 What is Apache Spark?

Spark is a **distributed computing engine** that processes large datasets across a cluster of machines.

- **100x faster than Hadoop MapReduce** (processes in-memory)
- Supports batch processing, SQL, streaming, and ML
- Write in Python (PySpark), Scala, Java, or R

> **Real-world analogy:** If pandas is a bicycle (great for small datasets), Spark is a freight train — slower to start, but can carry a million times more at once.

---

## 🧠 Key Concepts

### 1. Spark Architecture
```
[Driver Program]  ← Your code runs here (SparkSession)
       ↓
[Cluster Manager] ← YARN / Kubernetes / Standalone
       ↓
[Worker Nodes]    ← Executors run tasks in parallel
  Node 1: [Executor] [Executor]
  Node 2: [Executor] [Executor]
  Node 3: [Executor] [Executor]
```

### 2. SparkSession (Entry Point)
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MyPipeline") \
    .config("spark.sql.shuffle.partitions", "200") \
    .getOrCreate()
```

### 3. DataFrame API (Most Common)
```python
# Read data
df = spark.read.parquet("s3://my-bucket/orders/")

# Transformations (LAZY — nothing runs yet)
df_clean = df.filter(df.amount > 0) \
             .select("order_id", "customer_id", "amount") \
             .withColumn("amount_usd", df.amount * 1.0)

# Action (triggers execution)
df_clean.show(10)
df_clean.count()

# Write
df_clean.write.mode("overwrite").parquet("s3://my-bucket/clean_orders/")
```

### 4. Spark SQL
```python
# Register as temp view
df.createOrReplaceTempView("orders")

result = spark.sql("""
  SELECT customer_id, SUM(amount) AS total
  FROM orders
  WHERE status = 'completed'
  GROUP BY customer_id
  ORDER BY total DESC
""")

result.show()
```

### 5. Key Transformations vs Actions

| Transformations (Lazy) | Actions (Trigger Execution) |
|------------------------|------------------------------|
| `filter()` | `show()` |
| `select()` | `count()` |
| `groupBy()` | `collect()` |
| `join()` | `write()` |
| `withColumn()` | `take(n)` |

### 6. Joins in Spark
```python
orders = spark.read.parquet("s3://orders/")
customers = spark.read.parquet("s3://customers/")

# Broadcast join (when one table is small — avoids shuffle!)
from pyspark.sql.functions import broadcast

result = orders.join(broadcast(customers), "customer_id", "left")
```

### 7. Window Functions
```python
from pyspark.sql.window import Window
from pyspark.sql.functions import rank, sum as spark_sum

window = Window.partitionBy("region").orderBy("revenue".desc())
df.withColumn("rank", rank().over(window))
```

### 8. Partitioning Best Practices
```python
# Writing with partitioning (creates folders by date — faster queries!)
df.write \
  .partitionBy("year", "month") \
  .mode("overwrite") \
  .parquet("s3://output/")

# Output structure:
# s3://output/year=2024/month=01/part-000.parquet
```

---

## 🏭 Real-World Use Cases

| Use Case | Spark Feature |
|----------|--------------|
| Process 10TB of logs daily | Batch DataFrame API |
| Run SQL on data lake | Spark SQL |
| Real-time dashboard | Structured Streaming |
| Train ML model on big data | MLlib |
| Delta Lake ACID operations | Delta + Spark |

---

## 🛠️ Tools & Technologies

| Tool | Role |
|------|------|
| **PySpark** | Python API for Spark |
| **Databricks** | Managed Spark platform (cloud) |
| **AWS EMR** | Managed Spark on AWS |
| **Google Dataproc** | Managed Spark on GCP |
| **Delta Lake** | ACID layer on top of Spark |
| **Apache Iceberg** | Alternative to Delta Lake |

---

## ✅ Mini Practice Tasks

- [ ] Install PySpark locally: `pip install pyspark`
- [ ] Read a large CSV with Spark and compare performance vs pandas
- [ ] Write a Spark job that groups by a column and calculates sum + count
- [ ] Implement a broadcast join on two DataFrames
- [ ] Write partitioned output by date and verify folder structure

---

## 🔗 Related Notes

- [[Big Data Technologies]] — Spark's context in the big data ecosystem
- [[Python for Data Engineering]] — PySpark uses Python syntax
- [[Data Lakes vs Data Warehouses]] — Spark processes data in lakes
- [[Airflow]] — Airflow orchestrates Spark jobs (SparkSubmitOperator)
- [[Apache Kafka]] — Spark Structured Streaming consumes Kafka topics

---

## ➡️ Next Step

[[Apache Kafka]] — Learn how to handle real-time data streams that feed into Spark or standalone pipelines.
