# 🔀 04 — Control Flow

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[03-Variables-Types|← Previous: Variables & Types]]**

## 🧠 Feynman Explanation
> **Control flow is like a GPS.** Based on conditions (traffic, road closures), the GPS chooses different routes. `if/else` and `match` are your Scala GPS — they pick the right path based on conditions.

---

## 🔀 if / else — The Decision Maker

### Basic if/else
```scala
val age = 18

if (age >= 18) {
  println("Adult")
} else {
  println("Minor")
}
```

### 🔥 SUPERPOWER: if-else is an EXPRESSION
```scala
// Returns a value! No need for ternary operator like Java
val status = if (age >= 18) "Adult" else "Minor"
println(status)  // "Adult"

// Multi-branch expression
val grade = if (score >= 90) "A"
            else if (score >= 80) "B"
            else if (score >= 70) "C"
            else "F"
```

> **6th Grade Analogy:** In Java, `if-else` is like a traffic light — it just controls flow. In Scala, it's like a vending machine — you put in a condition and GET SOMETHING OUT!

---

## 🎯 Pattern Matching — Scala's Superweapon

> This is Scala's MOST POWERFUL feature. It's like a super-charged `switch` that can match anything.

### Basic Match
```scala
val day = "Monday"

val type_ = day match {
  case "Monday" | "Tuesday" | "Wednesday" | "Thursday" | "Friday" => "Weekday"
  case "Saturday" | "Sunday" => "Weekend"
  case _ => "Unknown"  // Default case (like else)
}
```

### Match on Types
```scala
def describe(x: Any): String = x match {
  case i: Int    => s"Integer: $i"
  case s: String => s"String: '$s' with length ${s.length}"
  case d: Double => s"Double: $d"
  case true      => "It's true!"
  case false     => "It's false!"
  case _         => "Unknown type"
}

describe(42)      // "Integer: 42"
describe("hi")    // "String: 'hi' with length 2"
describe(3.14)    // "Double: 3.14"
```

### Match with Guards (Conditions)
```scala
val score = 85

val grade = score match {
  case s if s >= 90 => "A"
  case s if s >= 80 => "B"
  case s if s >= 70 => "C"
  case s if s >= 60 => "D"
  case _             => "F"
}
```

### Match on Case Classes (Advanced but Important!)
```scala
case class Person(name: String, age: Int)

val person = Person("Pradeep", 21)

person match {
  case Person("Pradeep", age) => println(s"Found Pradeep, age $age")
  case Person(name, age) if age > 18 => println(s"$name is an adult")
  case Person(name, _) => println(s"Just $name")
}
```

---

## 🔄 Loops — Repeating Actions

### `for` Loop
```scala
// Basic range loop
for (i <- 1 to 5) {         // 1, 2, 3, 4, 5 (inclusive)
  println(s"Count: $i")
}

for (i <- 1 until 5) {      // 1, 2, 3, 4 (exclusive end)
  println(s"Count: $i")
}

// Step
for (i <- 1 to 10 by 2) {   // 1, 3, 5, 7, 9
  println(i)
}

// Reverse
for (i <- 5 to 1 by -1) {   // 5, 4, 3, 2, 1
  println(i)
}
```

### `for` with Collections
```scala
val fruits = List("apple", "banana", "cherry")

for (fruit <- fruits) {
  println(fruit.toUpperCase)
}

// With index
for ((fruit, index) <- fruits.zipWithIndex) {
  println(s"$index: $fruit")
}
```

### `for` with Guards (Filters)
```scala
for (i <- 1 to 20 if i % 2 == 0) {  // Only even numbers
  println(i)  // 2, 4, 6, 8, 10, 12, 14, 16, 18, 20
}

for {
  i <- 1 to 5
  j <- 1 to 5
  if i != j
} println(s"($i, $j)")  // All pairs where i ≠ j
```

### 🔥 For Comprehension (Returns a Collection!)
```scala
// for + yield = FOR COMPREHENSION
val squares = for (i <- 1 to 5) yield i * i
// squares = Vector(1, 4, 9, 16, 25)

val evenSquares = for {
  i <- 1 to 10
  if i % 2 == 0
} yield i * i
// evenSquares = Vector(4, 16, 36, 64, 100)
```

> **For Comprehension Analogy:** A for comprehension is like a FACTORY. Regular for = workers doing tasks. For comprehension = workers BUILDING products that come out on a conveyor belt!

---

## 🔁 while Loop

```scala
var count = 0
while (count < 5) {
  println(s"Count: $count")
  count += 1
}
```

### do-while
```scala
var input = ""
do {
  input = scala.io.StdIn.readLine("Enter 'quit': ")
} while (input != "quit")
println("Goodbye!")
```

---

## 🚫 break and continue — The Scala Way

> **Scala doesn't have `break` or `continue` keywords!** This is intentional — functional style avoids mid-loop exits.

### Using `breakable` (when you really need break)
```scala
import scala.util.control.Breaks._

breakable {
  for (i <- 1 to 100) {
    if (i == 10) break()  // Exit the loop
    println(i)
  }
}
```

### The Functional Alternative (BETTER!)
```scala
// Instead of break, use takeWhile / find / exists
val numbers = 1 to 100

// Find first number > 50
val firstBig = numbers.find(_ > 50)  // Some(51)

// All numbers until condition fails
val untilTen = numbers.takeWhile(_ < 10)  // Range(1, 2, 3, 4, 5, 6, 7, 8, 9)
```

---

## 🎭 Exception Handling

```scala
try {
  val result = 10 / 0
  println(result)
} catch {
  case e: ArithmeticException => println(s"Math error: ${e.getMessage}")
  case e: Exception           => println(s"General error: ${e.getMessage}")
} finally {
  println("This always runs!")
}
```

### Using Try (Functional Style — PREFERRED)
```scala
import scala.util.{Try, Success, Failure}

val result = Try(10 / 0)

result match {
  case Success(value) => println(s"Got: $value")
  case Failure(ex)    => println(s"Failed: ${ex.getMessage}")
}

// Chain operations
val parsed = Try("42".toInt)
  .map(_ * 2)           // If success: multiply by 2
  .getOrElse(0)         // If failure: return 0
// parsed = 84
```

---

## 📊 Control Flow Decision Chart

```
Need to decide?
     │
     ├── Simple true/false? → if/else expression
     │
     ├── Multiple cases on a value? → match/case
     │
     ├── Matching on TYPE? → match/case with type patterns
     │
     ├── Repeat N times? → for (i <- 1 to N)
     │
     ├── Repeat over collection? → for (x <- collection)
     │
     ├── Repeat until condition? → while / do-while
     │
     └── Transform while iterating? → for comprehension + yield
```

---

## 🧪 Practice Exercises

1. Write a match expression that classifies numbers: negative, zero, positive, or "big" (>1000)
2. Use a for comprehension to generate a multiplication table (1-10)
3. Use `Try` to safely parse user input as Double

---

## 🏷️ Tags
#scala #control-flow #if-else #pattern-matching #for-loop #while #for-comprehension #try #beginner #intermediate

## 🔗 Related Notes
- [[03-Variables-Types]] — Types used in conditions
- [[06-Functions]] — Functions with control flow
- [[07-Collections]] — Iterating collections
- [[08-OOP-Concepts]] — Pattern matching with case classes
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
