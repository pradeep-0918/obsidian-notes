# 🏭 10 — Data Engineering with Scala

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[09-Advanced-Scala|← Previous: Advanced Scala]]**

## 🧠 Feynman Explanation
> **Data Engineering is like running a water supply system for a city.** Raw water (raw data) comes from rivers (sources). It goes through filtration plants (transformation). Gets stored in reservoirs (data warehouses). Gets delivered to homes (consumers). Scala is the PIPES and PUMPS of this system — Apache Spark, Kafka, and Akka are the engineering plants!

---

## 🌟 Why Scala for Data Engineering?

| Tool | Written In | Why Scala? |
|------|-----------|------------|
| Apache Spark | Scala | Spark's native API is Scala |
| Apache Kafka | Scala/Java | Best client = Scala |
| Apache Flink | Java/Scala | Scala API available |
| Akka Streams | Scala | Pure Scala framework |
| Play Framework | Scala | Web layer for DE APIs |

---

## ⚡ APACHE SPARK — The King of Big Data

> **Spark Analogy:** Spark is like a team of workers that split a huge mountain of paperwork (data) into stacks, each worker processes their stack simultaneously, then combines results. This is distributed computing!

### Setup — build.sbt
```sbt
libraryDependencies ++= Seq(
  "org.apache.spark" %% "spark-core" % "3.5.0",
  "org.apache.spark" %% "spark-sql"  % "3.5.0",
  "org.apache.spark" %% "spark-streaming" % "3.5.0"
)
```

### SparkSession — Your Entry Point
```scala
import org.apache.spark.sql.SparkSession
import org.apache.spark.sql.functions._

val spark = SparkSession.builder()
  .appName("MySparkApp")
  .master("local[*]")    // local = on your machine, * = all CPU cores
  .config("spark.sql.shuffle.partitions", "8")
  .getOrCreate()

// Import implicit encoders
import spark.implicits._
```

---

## 📊 DATAFRAME API — The SQL-Like Interface

```scala
// Create DataFrame from file
val df = spark.read
  .option("header", "true")
  .option("inferSchema", "true")
  .csv("data/sales.csv")

// Show data
df.show(5)        // First 5 rows
df.printSchema()  // Column names and types
df.count()        // Number of rows
df.columns        // Array of column names

// Select columns
df.select("name", "salary")
df.select(col("name"), col("salary") * 1.1 as "new_salary")

// Filter (WHERE)
df.filter(col("age") > 25)
df.filter($"department" === "Engineering")
df.where("salary > 50000")

// GroupBy + Aggregate
df.groupBy("department")
  .agg(
    count("*").as("headcount"),
    avg("salary").as("avg_salary"),
    max("salary").as("max_salary"),
    sum("salary").as("total_salary")
  )
  .orderBy(desc("avg_salary"))

// Join DataFrames
val employees = spark.read.csv("employees.csv")
val departments = spark.read.csv("departments.csv")

employees.join(departments, "dept_id")               // Inner join
employees.join(departments, Seq("dept_id"), "left")  // Left join
employees.join(departments, Seq("dept_id"), "right") // Right join

// Window Functions
import org.apache.spark.sql.expressions.Window

val windowSpec = Window.partitionBy("department").orderBy(desc("salary"))

df.withColumn("rank", rank().over(windowSpec))
  .withColumn("dense_rank", dense_rank().over(windowSpec))
  .withColumn("salary_pct", col("salary") / sum("salary").over(
    Window.partitionBy("department")
  ))

// Add/Modify columns
df.withColumn("full_name", concat(col("first_name"), lit(" "), col("last_name")))
  .withColumn("salary_k", col("salary") / 1000)
  .withColumnRenamed("old_name", "new_name")
  .drop("unnecessary_column")

// Handle Nulls
df.na.drop()                   // Drop rows with ANY null
df.na.drop("any", Seq("col1", "col2"))  // Drop if null in these cols
df.na.fill(0)                  // Fill nulls with 0
df.na.fill(Map("age" -> 0, "name" -> "Unknown"))

// String functions
df.select(
  upper(col("name")),
  lower(col("name")),
  trim(col("name")),
  length(col("name")),
  substring(col("name"), 1, 5),
  regexp_replace(col("phone"), "[^0-9]", "")
)

// Date functions
df.select(
  current_date(),
  current_timestamp(),
  year(col("date")),
  month(col("date")),
  dayofweek(col("date")),
  date_format(col("date"), "yyyy-MM-dd"),
  datediff(col("end_date"), col("start_date")),
  date_add(col("date"), 30)
)
```

---

## 💡 SPARK SQL

```scala
// Register as temp view
df.createOrReplaceTempView("employees")

// Run SQL queries directly!
val result = spark.sql("""
  SELECT department, 
         COUNT(*) as headcount,
         AVG(salary) as avg_salary
  FROM employees
  WHERE salary > 30000
  GROUP BY department
  HAVING headcount > 5
  ORDER BY avg_salary DESC
""")

result.show()
```

---

## 🏗️ DATASET API — Type-Safe DataFrames

```scala
// Case class for schema
case class Employee(id: Int, name: String, salary: Double, dept: String)

// Create typed Dataset
val ds: Dataset[Employee] = spark.read
  .option("header", "true")
  .csv("employees.csv")
  .as[Employee]  // Magic! Typed now.

// Type-safe operations (compile-time checks!)
ds.filter(_.salary > 50000)
ds.map(e => e.copy(salary = e.salary * 1.1))
ds.groupByKey(_.dept).count()
```

---

## 🔵 RDD — The Original Spark API

```scala
// RDD = Resilient Distributed Dataset
val rdd = spark.sparkContext.parallelize(List(1, 2, 3, 4, 5))

// Transformations (lazy!)
rdd.map(_ * 2)           // RDD[Int]
rdd.filter(_ % 2 == 0)  // RDD[Int]
rdd.flatMap(x => List(x, x*x))

// Actions (trigger computation)
rdd.collect()   // Array[Int] — bring all to driver
rdd.count()     // Long
rdd.sum()       // Double
rdd.take(3)     // Array[Int]

// Pair RDD (key-value)
val pairRDD = rdd.map(x => (x % 3, x))
pairRDD.groupByKey()
pairRDD.reduceByKey(_ + _)
pairRDD.sortByKey()
```

---

## 📈 SPARK STREAMING

```scala
import org.apache.spark.streaming._

val ssc = new StreamingContext(spark.sparkContext, Seconds(5))

// Read from socket
val lines = ssc.socketTextStream("localhost", 9999)

// Word count in real-time
val words = lines.flatMap(_.split(" "))
val pairs = words.map(w => (w, 1))
val counts = pairs.reduceByKey(_ + _)
counts.print()

ssc.start()
ssc.awaitTermination()
```

---

## 🌊 APACHE KAFKA — Real-Time Streaming

```scala
// Kafka = messaging system
// Producers → Topics → Consumers
// Like a post office: senders → mailboxes → recipients

// Spark Structured Streaming + Kafka
val kafkaDF = spark.readStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "localhost:9092")
  .option("subscribe", "my-topic")
  .option("startingOffsets", "earliest")
  .load()

// Parse Kafka messages
val parsed = kafkaDF.select(
  col("key").cast("string"),
  col("value").cast("string"),
  col("timestamp"),
  col("partition"),
  col("offset")
)

// Process and write back to Kafka
val output = parsed
  .select(to_json(struct("*")).as("value"))
  .writeStream
  .format("kafka")
  .option("kafka.bootstrap.servers", "localhost:9092")
  .option("topic", "output-topic")
  .option("checkpointLocation", "/checkpoint")
  .start()
```

### Kafka Scala Producer/Consumer
```scala
import org.apache.kafka.clients.producer._
import org.apache.kafka.clients.consumer._

// Producer
val producerProps = new java.util.Properties()
producerProps.put("bootstrap.servers", "localhost:9092")
producerProps.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer")
producerProps.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer")

val producer = new KafkaProducer[String, String](producerProps)
producer.send(new ProducerRecord("my-topic", "key", "value"))
producer.close()
```

---

## 🗄️ READING/WRITING DATA SOURCES

```scala
// CSV
df.write.option("header", "true").csv("output/data.csv")
df.write.mode("overwrite").csv("output/data.csv")

// Parquet (columnar, compressed — preferred for big data!)
df.write.parquet("output/data.parquet")
val parquetDf = spark.read.parquet("output/data.parquet")

// JSON
df.write.json("output/data.json")
val jsonDf = spark.read.json("data.json")

// ORC
df.write.orc("output/data.orc")

// Delta Lake (ACID transactions for data lakes!)
df.write.format("delta").save("output/delta-table")

// JDBC (databases)
df.write
  .format("jdbc")
  .option("url", "jdbc:postgresql://localhost/mydb")
  .option("dbtable", "public.employees")
  .option("user", "postgres")
  .option("password", "secret")
  .mode("append")
  .save()

// Read from JDBC
val jdbcDf = spark.read
  .format("jdbc")
  .option("url", "jdbc:postgresql://localhost/mydb")
  .option("dbtable", "public.employees")
  .load()
```

---

## 🔧 SPARK PERFORMANCE TUNING

```scala
// Caching — keep data in memory
df.cache()               // Cache in memory (RDD semantics)
df.persist()             // Same as cache
df.persist(StorageLevel.MEMORY_AND_DISK)  // Spill to disk if needed
df.unpersist()           // Release cache

// Partitioning
df.repartition(100)                      // Shuffle into 100 partitions
df.repartition(col("department"))        // Partition by column
df.coalesce(10)                          // Reduce partitions (no shuffle)

// Broadcast join (small table → all nodes)
import org.apache.spark.sql.functions.broadcast
employees.join(broadcast(smallDept), "dept_id")

// Configs
spark.conf.set("spark.sql.shuffle.partitions", "200")
spark.conf.set("spark.executor.memory", "4g")
spark.conf.set("spark.driver.memory", "2g")
```

---

## 🏗️ COMPLETE DATA PIPELINE EXAMPLE

```scala
object SalesPipeline {
  def main(args: Array[String]): Unit = {
    val spark = SparkSession.builder()
      .appName("SalesPipeline")
      .master("local[*]")
      .getOrCreate()

    import spark.implicits._

    // 1. EXTRACT — Read raw data
    val rawSales = spark.read
      .option("header", "true")
      .option("inferSchema", "true")
      .csv("raw/sales_2024.csv")

    // 2. TRANSFORM — Clean & enrich
    val cleanSales = rawSales
      .na.fill(Map("quantity" -> 0, "price" -> 0.0))
      .withColumn("revenue", col("quantity") * col("price"))
      .withColumn("year", year(col("sale_date")))
      .withColumn("month", month(col("sale_date")))
      .filter(col("revenue") > 0)

    // 3. AGGREGATE — Business metrics
    val monthlySummary = cleanSales
      .groupBy("year", "month", "category")
      .agg(
        sum("revenue").as("total_revenue"),
        count("*").as("transaction_count"),
        avg("revenue").as("avg_transaction")
      )

    // 4. LOAD — Write to data warehouse
    monthlySummary.write
      .partitionBy("year", "month")
      .mode("overwrite")
      .parquet("warehouse/monthly_summary")

    println("Pipeline complete!")
    spark.stop()
  }
}
```

---

## 🧩 Key Scala DE Libraries

| Library | Purpose | sbt dependency |
|---------|---------|----------------|
| Apache Spark | Big data processing | `spark-sql`, `spark-core` |
| Delta Lake | ACID data lake | `delta-core` |
| Kafka | Streaming | `kafka_2.13` |
| Akka Streams | Reactive streams | `akka-stream` |
| Slick | Database (type-safe SQL) | `slick` |
| Doobie | Functional JDBC | `doobie-core` |
| Circe | JSON | `circe-core` |
| Cats | Functional programming | `cats-core` |
| ZIO | Effect system | `zio` |

---

## 🏷️ Tags
#scala #data-engineering #spark #kafka #dataframe #rdd #streaming #etl #big-data #parquet #delta-lake

## 🔗 Related Notes
- [[08-OOP-Concepts]] — Case classes for Dataset schemas
- [[07-Collections]] — Scala collections → Spark RDDs
- [[06-Functions]] — HOF patterns used in Spark transformations
- [[09-Advanced-Scala]] — Futures used in async data pipelines
- [[Data-Engineering-Cheatsheet]] — Quick reference
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
