# 📋 Data Engineering Cheatsheet

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[10-Data-Engineering|← Full DE Notes]]**

## 🏷️ Tags
#scala #spark #kafka #data-engineering #cheatsheet #etl

---

## ⚡ Spark Session Boilerplate

```scala
val spark = SparkSession.builder()
  .appName("App").master("local[*]").getOrCreate()
import spark.implicits._
```

## 📊 DataFrame Top 10 Operations

```scala
df.show(5)                              // Preview
df.printSchema()                        // Schema
df.count()                              // Row count
df.select("col1", "col2")              // Pick columns
df.filter(col("age") > 25)             // WHERE
df.groupBy("dept").agg(avg("salary"))  // GROUP BY
df.join(other, "key")                  // JOIN
df.withColumn("new", col("a")+col("b")) // Add column
df.na.fill(0)                           // Fill nulls
df.orderBy(desc("salary"))             // ORDER BY
```

## 🔤 Spark SQL Functions

```scala
// String
upper, lower, trim, length, substring, concat, regexp_replace

// Date
year, month, dayofweek, date_format, datediff, date_add, current_date

// Agg
count, sum, avg, max, min, first, last, collect_list

// Window
rank, dense_rank, row_number, lag, lead, sum.over(window)
```

## 🗄️ Read / Write

```scala
// Read
spark.read.option("header","true").csv("path")
spark.read.parquet("path")
spark.read.json("path")

// Write
df.write.mode("overwrite").parquet("path")
df.write.partitionBy("year","month").parquet("path")
df.write.format("delta").save("path")
```

## 🌊 Kafka + Spark Streaming

```scala
spark.readStream.format("kafka")
  .option("kafka.bootstrap.servers","localhost:9092")
  .option("subscribe","topic").load()
  .writeStream.format("console").start()
```

## ⚡ Performance

```scala
df.cache()                          // Cache in memory
df.repartition(100)                 // Shuffle to N parts
df.coalesce(10)                     // Reduce parts
employees.join(broadcast(small),k) // Broadcast join
```

---

## 🔗 Related Notes
[[10-Data-Engineering]] | [[07-Collections]] | [[08-OOP-Concepts]] | [[🗺️ SCALA MASTER MAP]]
