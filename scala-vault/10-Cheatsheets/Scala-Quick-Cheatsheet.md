# 📋 Scala Quick Cheatsheet

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]**

## 🏷️ Tags
#scala #cheatsheet #quick-reference #syntax

---

## 🔑 Basics
```scala
// Variables
val x = 10          // Immutable
var y = 20          // Mutable
lazy val z = 30     // Lazy

// Output
println(s"x=$x, y=$y")

// Types
Int, Long, Double, Float, Char, Boolean, String, Unit
```

## 🔢 Type Conversion
```scala
42.toString    "42".toInt    3.14.toInt → 3
'A'.toInt → 65    65.toChar → 'A'
Try("x".toInt).getOrElse(0)   // Safe
```

## 🔀 Control Flow
```scala
val r = if (x > 5) "big" else "small"     // Expression!

x match {
  case 1 => "one"
  case n if n > 10 => "big"
  case _ => "other"
}

for (i <- 1 to 5 if i % 2 == 0) yield i*i  // Comprehension
```

## 🧩 Functions
```scala
def add(x: Int, y: Int): Int = x + y
val fn: Int => Int = _ * 2              // Lambda
def f(x: Int)(y: Int) = x + y          // Curried

List(1,2,3).map(_ * 2)    // HOF
List(1,2,3).filter(_ > 1)
List(1,2,3).foldLeft(0)(_ + _)  // 6
```

## 📦 Collections
```scala
List(1,2,3).head / .tail / .last / .take(2) / .drop(1)
Set(1,2,2,3) → Set(1,2,3)           // Unique
Map("a"->1).get("a") → Some(1)
Vector — fast random access
```

## 🏛️ OOP
```scala
class Foo(val x: Int)
case class Bar(x: Int, y: String)    // Auto equals/copy/toString
object Singleton                      // No 'new'
trait MyTrait { def m(): Unit }
sealed trait Color
case object Red extends Color
```

## ⚡ Option / Try / Either
```scala
Some(5).map(_ * 2)   // Some(10)
None.getOrElse(0)    // 0

Try(risky()).getOrElse(default)

Right(value)  // Success
Left(error)   // Failure
```

## 🏭 Spark One-Liners
```scala
df.select("col").filter($"col" > 5).groupBy("dept").agg(avg("salary"))
df.na.fill(0).withColumn("new", col("a") + col("b"))
spark.sql("SELECT * FROM table WHERE x > 5")
df.write.parquet("path")
```

---

## 🔗 All Notes
[[01-Basic-Syntax]] | [[02-Input-Output]] | [[03-Variables-Types]] | [[04-Control-Flow]] | [[05-Strings-Math]] | [[06-Functions]] | [[07-Collections]] | [[08-OOP-Concepts]] | [[09-Advanced-Scala]] | [[10-Data-Engineering]]
