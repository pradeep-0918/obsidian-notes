# 08 — Advanced Pro Tricks 🏆
#scala #advanced #pro #pattern-matching

← [[07-Functions]] | Next → [[09-Speed-Cheatsheet]]

---

## 🧒 Feynman Explain

> This is PRO LEVEL — where Scala becomes MAGIC! ✨
> You already know the basics.
> Now you'll learn to write code that looks like ART and runs like LIGHTNING.

---

## 🎭 Pattern Matching — Scala's Superpower

```scala
// Match ANY type — values, types, structures!

// 1. Match on value type:
def describe(x: Any): String = x match {
  case n: Int     => s"Integer: $n"
  case s: String  => s"String: '$s'"
  case d: Double  => s"Double: $d"
  case b: Boolean => s"Boolean: $b"
  case _          => "Unknown"
}

describe(42)      // "Integer: 42"
describe("hi")    // "String: 'hi'"
describe(true)    // "Boolean: true"

// 2. Match on List structure:
def listInfo(lst: List[Int]): String = lst match {
  case Nil         => "Empty list!"
  case x :: Nil    => s"One element: $x"
  case x :: y :: _ => s"Starts with $x and $y"
}

listInfo(List())         // "Empty list!"
listInfo(List(5))        // "One element: 5"
listInfo(List(1,2,3,4)) // "Starts with 1 and 2"
```

---

## 🏗️ Case Classes — Data Blueprints

```scala
// A case class is like a smart data container
case class Point(x: Int, y: Int)
case class Student(name: String, grade: Int, score: Double)

val p = Point(3, 4)
p.x    // 3
p.y    // 4

val s = Student("Arjun", 6, 95.5)
s.name   // "Arjun"

// ⚡ TRICK: Case classes work with match!
val p2 = Point(0, 5)
val location = p2 match {
  case Point(0, 0) => "Origin"
  case Point(0, y) => s"On Y-axis at $y"
  case Point(x, 0) => s"On X-axis at $x"
  case Point(x, y) => s"At ($x, $y)"
}
// "On Y-axis at 5"

// ⚡ TRICK: Copy with changes (immutable update!)
val p3 = p.copy(y = 10)   // Point(3, 10)
```

---

## 🎯 for-comprehension (Advanced!)

```scala
// Like a mini SQL query!
val students = List(
  ("Arjun", 95), ("Priya", 88), ("Ravi", 72), ("Meera", 91)
)

// Get names of students with score > 85
val topStudents = for {
  (name, score) <- students
  if score > 85
} yield name

// List("Arjun", "Priya", "Meera")

// With Option (safe value extraction):
val maybeAge: Option[Int] = Some(15)
val maybeScore: Option[Int] = Some(90)

val result = for {
  age   <- maybeAge
  score <- maybeScore
  if age >= 12
} yield s"Age $age, Score $score"
// Some("Age 15, Score 90")
```

---

## 🔧 Traits — Mixable Behaviors

```scala
// Trait = abilities you can add to anything!
trait Printable {
  def display: String
  def print(): Unit = println(display)
}

trait Calculable {
  def compute(a: Int, b: Int): Int
}

// Mix multiple traits:
class Calculator extends Printable with Calculable {
  override def display = "I am a Calculator!"
  override def compute(a: Int, b: Int): Int = a + b
}

val calc = new Calculator()
calc.print()           // I am a Calculator!
calc.compute(3, 4)     // 7
```

---

## ⚡ TRICK — implicit + Extension Methods

```scala
// Add methods to existing types without changing them!
implicit class IntOps(n: Int) {
  def factorial: BigInt =
    if (n <= 1) 1 else n * (n-1).factorial

  def isPrime: Boolean =
    n > 1 && (2 to math.sqrt(n).toInt).forall(n % _ != 0)

  def digits: List[Int] =
    n.toString.map(_.asDigit).toList
}

// Now you can call these ON any Int!
5.factorial    // 120
17.isPrime     // true
123.digits     // List(1, 2, 3)
```

---

## ⚡ TRICK — Try/Catch (Safe Computations)

```scala
import scala.util.{Try, Success, Failure}

// Safe division (no crash on divide by zero!)
def safeDivide(a: Int, b: Int): Try[Int] = Try(a / b)

safeDivide(10, 2) match {
  case Success(result) => println(s"Result: $result")
  case Failure(error)  => println(s"Error: ${error.getMessage}")
}
// Result: 5

safeDivide(10, 0) match {
  case Success(r) => println(r)
  case Failure(e) => println(s"Error: ${e.getMessage}")
}
// Error: / by zero

// One-liner with getOrElse:
val result = Try(10 / 0).getOrElse(0)   // 0 (safe!)
```

---

## ⚡ TRICK — Parallel Collections (Speed Boost!)

```scala
// Regular (sequential):
(1 to 1000000).filter(isPrime).length   // Slow (one core)

// Parallel (all CPU cores!):
(1 to 1000000).par.filter(isPrime).length  // FAST! (.par = parallel)

// Just add .par to any collection!
List(1,2,3,4,5).par.map(_ * 2)
```

---

## ⚡ TRICK — Lazy Lists (Infinite Sequences!)

```scala
// LazyList can represent INFINITE sequences!
// Evaluated ONLY when you need the values

// Infinite stream of natural numbers:
val naturals: LazyList[Int] = LazyList.from(1)

// Take only what you need:
naturals.take(10).toList   // List(1,2,3,4,5,6,7,8,9,10)

// Infinite Fibonacci:
def fibs: LazyList[BigInt] = {
  def go(a: BigInt, b: BigInt): LazyList[BigInt] =
    a #:: go(b, a + b)   // #:: is lazy cons
  go(0, 1)
}

fibs.take(15).toList
// List(0,1,1,2,3,5,8,13,21,34,55,89,144,233,377)

fibs(100)  // 100th Fibonacci — instant with lazy evaluation!
```

---

## ⚡ TRICK — Tail-Recursive Algorithms

```scala
import scala.annotation.tailrec

// Binary Search (O log n — very fast!)
@tailrec
def binarySearch(arr: Array[Int], target: Int,
                  lo: Int = 0, hi: Int = -1): Int = {
  val high = if (hi == -1) arr.length - 1 else hi
  if (lo > high) -1
  else {
    val mid = (lo + high) / 2
    if (arr(mid) == target) mid
    else if (arr(mid) < target) binarySearch(arr, target, mid + 1, high)
    else binarySearch(arr, target, lo, mid - 1)
  }
}

val sorted = Array(1, 3, 5, 7, 9, 11, 15, 20)
binarySearch(sorted, 7)    // 3  (index 3)
binarySearch(sorted, 6)    // -1 (not found)
```

---

## 🏆 PRO Pattern — Solve Any Sum Problem

```scala
// Sliding window sum of size k:
def windowSum(nums: List[Int], k: Int): List[Int] =
  nums.sliding(k).map(_.sum).toList

windowSum(List(1,2,3,4,5), 3)   // List(6,9,12)

// Running total (prefix sums):
def prefixSums(nums: List[Int]): List[Int] =
  nums.scanLeft(0)(_ + _).tail

prefixSums(List(1,2,3,4,5))   // List(1,3,6,10,15)

// Sum of subarray [l..r] using prefix sums:
val prefix = Array(0, 1, 3, 6, 10, 15)
def rangeSum(l: Int, r: Int): Int = prefix(r+1) - prefix(l)
rangeSum(1, 3)   // 2+3+4 = 9
```

---

## 🧒 Feynman Summary

> Pattern matching = Swiss Army knife for conditions
> Case classes = Smart data holders with auto-match
> Traits = Mix-in abilities (like power-ups 🎮)
> Try = Safe computation (no crashes!)
> LazyList = Infinite sequences evaluated on demand
> `.par` = Free speed boost on any collection!

---

## 🔗 See Also
- Previous: [[07-Functions]]
- Next: [[09-Speed-Cheatsheet]]
- Collections: [[06-Loops-And-Collections]]
- Control flow: [[05-Control-Flow]]
