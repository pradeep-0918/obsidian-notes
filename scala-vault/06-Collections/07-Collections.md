# 📦 07 — Collections

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[06-Functions|← Previous: Functions]]**

## 🧠 Feynman Explanation
> **Collections are like different types of containers.** A `List` = a row of lockers (ordered, can have duplicates). A `Set` = a bag of marbles (unordered, no duplicates). A `Map` = a dictionary (key → value pairs). Choose the right container for the right job!

---

## 🗂️ The Collection Hierarchy

```
Iterable
├── Seq (ordered, index-accessible)
│   ├── List      ← Immutable linked list (most common!)
│   ├── Vector    ← Immutable, fast random access
│   ├── ArrayBuffer ← Mutable, like ArrayList
│   ├── Array     ← Fixed-size, Java-compatible
│   └── Range     ← 1 to 10, 1 until 10
├── Set (no duplicates)
│   ├── HashSet   ← Immutable, fast lookup
│   └── TreeSet   ← Immutable, sorted
└── Map (key-value)
    ├── HashMap   ← Immutable, fast lookup
    └── TreeMap   ← Immutable, sorted by key
```

**Golden Rule:** Prefer **immutable** collections by default. Import mutable only when needed.

---

## 📋 LIST — The Workhorse

```scala
// Creating
val nums = List(1, 2, 3, 4, 5)
val empty = List.empty[Int]  // or Nil
val filled = List.fill(3)(0)  // List(0, 0, 0)
val range = (1 to 5).toList   // List(1, 2, 3, 4, 5)

// Prepend (fast! O(1))
val bigger = 0 :: nums    // List(0, 1, 2, 3, 4, 5)
val more = 0 :: 1 :: 2 :: Nil  // List(0, 1, 2)

// Append (slow! O(n))
val appended = nums :+ 6  // List(1, 2, 3, 4, 5, 6)

// Concatenate
val combined = nums ++ List(6, 7, 8)  // List(1,2,3,4,5,6,7,8)

// Access
nums.head        // 1 (throws if empty!)
nums.tail        // List(2, 3, 4, 5)
nums(2)          // 3 (index)
nums.last        // 5
nums.init        // List(1, 2, 3, 4)
nums.headOption  // Some(1) — safe!

// Info
nums.length      // 5
nums.size        // 5
nums.isEmpty     // false
nums.nonEmpty    // true

// Transformation (all return NEW lists!)
nums.map(_ * 2)          // List(2, 4, 6, 8, 10)
nums.filter(_ % 2 == 0)  // List(2, 4)
nums.flatMap(x => List(x, x))  // List(1,1,2,2,3,3,4,4,5,5)
nums.reverse             // List(5, 4, 3, 2, 1)
nums.sorted              // List(1, 2, 3, 4, 5)
nums.distinct            // removes duplicates

// Searching
nums.find(_ > 3)          // Some(4)
nums.exists(_ > 10)       // false
nums.forall(_ > 0)        // true
nums.count(_ % 2 == 0)    // 2

// Folding
nums.sum                  // 15
nums.product              // 120
nums.max                  // 5
nums.min                  // 1
nums.foldLeft(0)(_ + _)   // 15
nums.reduce(_ + _)        // 15

// Grouping
nums.grouped(2).toList    // List(List(1,2), List(3,4), List(5))
nums.sliding(3).toList    // List(List(1,2,3), List(2,3,4), List(3,4,5))
nums.partition(_ % 2 == 0) // (List(2,4), List(1,3,5))
nums.span(_ < 4)          // (List(1,2,3), List(4,5))
nums.splitAt(3)           // (List(1,2,3), List(4,5))

// Sorting
val words = List("banana", "apple", "cherry")
words.sorted              // List("apple", "banana", "cherry")
words.sortBy(_.length)    // List("apple", "banana", "cherry")
words.sortWith(_.length > _.length)  // Longest first

// Zipping
val a = List(1, 2, 3)
val b = List("one", "two", "three")
a.zip(b)           // List((1,"one"), (2,"two"), (3,"three"))
a.zipWithIndex     // List((1,0), (2,1), (3,2))
a.zipAll(b, 0, "?")  // handles unequal lengths

// Flattening
List(List(1,2), List(3,4)).flatten  // List(1,2,3,4)

// Taking / Dropping
nums.take(3)       // List(1, 2, 3)
nums.drop(3)       // List(4, 5)
nums.takeWhile(_ < 4)  // List(1, 2, 3)
nums.dropWhile(_ < 4)  // List(4, 5)
```

---

## 🔵 VECTOR — Fast Random Access List

```scala
val v = Vector(1, 2, 3, 4, 5)

// Same API as List, but different performance!
v(3)           // 4  ← O(log n), faster than List!
v :+ 6         // Append: O(log n)
0 +: v         // Prepend: O(log n)
v.updated(2, 99)  // Vector(1, 2, 99, 4, 5)
```

### List vs Vector
| Operation | List | Vector |
|-----------|------|--------|
| Prepend `x :: xs` | ⚡ O(1) | 🐢 O(log n) |
| Append `xs :+ x` | 🐢 O(n) | ⚡ O(log n) |
| Random access `xs(i)` | 🐢 O(n) | ⚡ O(log n) |
| Head/Tail | ⚡ O(1) | ⚡ O(1) |
| **Best for** | Prepend-heavy | Random access |

---

## 🎯 SET — Unique Elements Only

```scala
val fruits = Set("apple", "banana", "apple", "cherry")
// Set("apple", "banana", "cherry")  — apple only once!

// Operations
fruits.contains("apple")   // true
fruits + "mango"            // Add: Set("apple", "banana", "cherry", "mango")
fruits - "banana"           // Remove: Set("apple", "cherry")

val set2 = Set("banana", "mango")
fruits ++ set2              // Union: all elements
fruits & set2               // Intersection: common elements
fruits &~ set2              // Difference: in fruits but not set2
fruits -- set2              // Same as &~

// Sorted Set
import scala.collection.immutable.SortedSet
val sorted = SortedSet(5, 2, 8, 1, 9)  // SortedSet(1, 2, 5, 8, 9)
```

---

## 🗺️ MAP — Key-Value Pairs

```scala
// Creating
val scores = Map("Alice" -> 95, "Bob" -> 87, "Carol" -> 92)
val empty = Map.empty[String, Int]

// Access
scores("Alice")              // 95 (throws if key missing!)
scores.get("Alice")          // Some(95) — safe!
scores.get("Dave")           // None
scores.getOrElse("Dave", 0)  // 0 — with default
scores.withDefaultValue(0)   // Returns map with default

// Modification (returns new map!)
scores + ("Dave" -> 88)      // Add/update
scores - "Bob"               // Remove key
scores ++ Map("Eve" -> 91)   // Merge maps

// Checking
scores.contains("Alice")     // true
scores.keys                  // Set("Alice", "Bob", "Carol")
scores.values                // Iterable(95, 87, 92)
scores.keySet                // Set("Alice", "Bob", "Carol")
scores.size                  // 3
scores.isEmpty               // false

// Iteration
for ((name, score) <- scores) println(s"$name: $score")

scores.foreach { case (name, score) => println(s"$name: $score") }

// Transformation
scores.map { case (k, v) => k -> v * 2 }   // Double all scores
scores.filter { case (_, v) => v > 90 }     // Only high scorers
scores.mapValues(_ + 5)                      // Add 5 to each score (deprecated in 2.13, use map)

// Grouping
val words = List("apple", "ant", "banana", "bat", "cherry")
val byFirstLetter = words.groupBy(_.head)
// Map('a' -> List("apple", "ant"), 'b' -> List("banana", "bat"), ...)
```

---

## 🔄 ARRAY — Java-Compatible Fixed Size

```scala
val arr = Array(1, 2, 3, 4, 5)
val zeros = Array.fill(5)(0)         // Array(0,0,0,0,0)
val range = Array.range(1, 6)        // Array(1,2,3,4,5)

// Mutable!
arr(0) = 99     // Array(99, 2, 3, 4, 5)

// Same functional methods as List
arr.map(_ * 2)
arr.filter(_ > 2)
arr.sorted

// Java interop
java.util.Arrays.sort(arr)
```

---

## 🔧 MUTABLE Collections

```scala
import scala.collection.mutable

// Mutable List (ArrayBuffer)
val buf = mutable.ArrayBuffer(1, 2, 3)
buf += 4          // Append
buf -= 3          // Remove first occurrence
buf(0) = 99       // Update
buf.clear()       // Empty it

// Mutable Map
val mMap = mutable.HashMap("a" -> 1, "b" -> 2)
mMap("c") = 3     // Add
mMap.remove("a")  // Remove
mMap("b") = 99    // Update

// Mutable Set
val mSet = mutable.HashSet(1, 2, 3)
mSet += 4         // Add
mSet -= 2         // Remove
```

---

## 🏆 Collection Cheatsheet — What to Use When

| Need | Use |
|------|-----|
| Ordered, no change | `List` |
| Ordered, fast random access | `Vector` |
| Unique elements | `Set` |
| Key-value lookup | `Map` |
| Grow/shrink often | `ArrayBuffer` (mutable) |
| Java interop | `Array` |
| Number range | `Range` (1 to 10) |
| Lazy collection | `LazyList` (formerly Stream) |

---

## 🔥 Real-World Patterns

```scala
// Word frequency count
val text = "the quick brown fox jumps over the lazy dog the fox"
val wordFreq = text.split(" ")
  .groupBy(identity)
  .map { case (word, occurrences) => word -> occurrences.length }
  .toList
  .sortBy(-_._2)  // Sort by count descending

// Top 3 words
wordFreq.take(3)

// Transpose a list of lists
val matrix = List(List(1,2,3), List(4,5,6), List(7,8,9))
matrix.transpose  // List(List(1,4,7), List(2,5,8), List(3,6,9))
```

---

## 🏷️ Tags
#scala #collections #list #vector #set #map #array #functional-programming #intermediate

## 🔗 Related Notes
- [[06-Functions]] — HOF on collections
- [[04-Control-Flow]] — for loops with collections
- [[08-OOP-Concepts]] — Custom collection-like classes
- [[10-Data-Engineering]] — Spark RDDs and DataFrames
- [[Collections-Cheatsheet]] — Quick reference
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
