# 06 — Loops & Collections 🔁
#scala #intermediate #trick #collections

← [[05-Control-Flow]] | Next → [[07-Functions]]

---

## 🧒 Feynman Explain

> A **loop** = doing the same thing many times automatically
> Like: "Say hello 100 times" → You don't type it 100 times, you LOOP!
> A **collection** = a bag that holds many things at once 🎒

---

## 🔁 Loops in Scala

### for loop

```scala
// Count from 1 to 5
for (i <- 1 to 5) {
  println(i)
}
// 1, 2, 3, 4, 5

// 1 until 5 (excludes 5!)
for (i <- 1 until 5) {
  println(i)
}
// 1, 2, 3, 4

// Step by 2:
for (i <- 1 to 10 by 2) println(i)
// 1, 3, 5, 7, 9

// Count DOWN:
for (i <- 10 to 1 by -1) println(i)
// 10, 9, 8, ... 1
```

### while loop

```scala
var i = 1
while (i <= 5) {
  println(i)
  i += 1
}
```

---

## ⚡ TRICK — for as Expression (yield!)

```scala
// Instead of a loop that just prints,
// COLLECT the results into a new list!

val squares = for (i <- 1 to 5) yield i * i
// squares = Vector(1, 4, 9, 16, 25)

val evens = for (i <- 1 to 20 if i % 2 == 0) yield i
// evens = Vector(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)

// Double loop (like nested for):
val pairs = for {
  x <- 1 to 3
  y <- 1 to 3
  if x != y
} yield (x, y)
// Vector((1,2),(1,3),(2,1),(2,3),(3,1),(3,2))
```

---

## 📦 Collections — The Big 4

### 1. List (Ordered, Immutable)

```scala
val nums = List(1, 2, 3, 4, 5)

nums.head          // 1       (first element)
nums.tail          // List(2,3,4,5)  (all but first)
nums.last          // 5       (last element)
nums(2)            // 3       (index 2)
nums.length        // 5
nums.sum           // 15
nums.max           // 5
nums.min           // 1
nums.sorted        // List(1,2,3,4,5)  (already sorted)

// Add to front:
0 :: nums           // List(0,1,2,3,4,5)  ← :: means prepend

// Combine two lists:
List(1,2) ++ List(3,4)  // List(1,2,3,4)

// Reverse:
nums.reverse        // List(5,4,3,2,1)
```

### 2. Array (Ordered, Mutable, Fast)

```scala
val arr = Array(10, 20, 30, 40, 50)

arr(0)             // 10
arr(0) = 99        // ✅ Arrays can change!
arr.sum            // 199 (now 99+20+30+40+50)
arr.sorted         // Array(20,30,40,50,99)
arr.length         // 5

// Create with fill:
val zeros = Array.fill(5)(0)    // Array(0,0,0,0,0)
val range = Array.range(1, 6)   // Array(1,2,3,4,5)
```

### 3. Map (Key → Value pairs)

```scala
val scores = Map("Arjun" -> 95, "Priya" -> 88, "Ravi" -> 72)

scores("Arjun")           // 95
scores.get("Priya")       // Some(88)  (safe!)
scores.getOrElse("Xyz", 0) // 0  (safe default)

scores.keys               // Set(Arjun, Priya, Ravi)
scores.values             // Iterable(95, 88, 72)

scores.contains("Ravi")   // true

// Add/update (returns new map since immutable):
val updated = scores + ("Meera" -> 90)
```

### 4. Set (Unique values only!)

```scala
val s = Set(1, 2, 3, 2, 1)   // Set(1,2,3) — duplicates removed!

s.contains(2)    // true
s + 4            // Set(1,2,3,4)
s - 2            // Set(1,3)
s.size           // 3

// Set operations:
val a = Set(1,2,3,4)
val b = Set(3,4,5,6)
a union b        // Set(1,2,3,4,5,6)   (combine)
a intersect b    // Set(3,4)           (common)
a diff b         // Set(1,2)           (in a but not b)
```

---

## ⚡ TRICK — The Power of map, filter, reduce

```scala
val nums = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

// map → Transform EVERY element
val doubled = nums.map(_ * 2)
// List(2, 4, 6, 8, 10, 12, 14, 16, 18, 20)

// filter → Keep only elements that pass a test
val evens = nums.filter(_ % 2 == 0)
// List(2, 4, 6, 8, 10)

// reduce → Combine all into one value
val sum = nums.reduce(_ + _)      // 55
val product = nums.reduce(_ * _)  // 3628800

// fold → like reduce but with a starting value
val sum2 = nums.foldLeft(0)(_ + _)   // 55
val sum3 = nums.foldRight(0)(_ + _)  // 55

// CHAIN THEM! (The PRO way!)
val result = nums
  .filter(_ % 2 == 0)    // Keep evens: 2,4,6,8,10
  .map(_ * _ )            // Square them: 4,16,36,64,100
  .sum                    // Add up: 220
```

---

## ⚡ TRICK — One-Liners for Common Problems

```scala
val nums = List(3, 1, 4, 1, 5, 9, 2, 6, 5, 3)

// Sum
nums.sum                          // 39

// Count elements matching condition
nums.count(_ > 4)                 // 3

// Check if ANY element matches
nums.exists(_ > 8)                // true

// Check if ALL elements match
nums.forall(_ > 0)                // true

// Find first match
nums.find(_ > 4)                  // Some(5)

// Remove duplicates
nums.distinct                     // List(3,1,4,5,9,2,6)

// Group by condition
nums.groupBy(_ % 2 == 0)
// Map(false -> List(3,1,1,5,9,5,3), true -> List(4,2,6))

// Partition (split into two lists)
val (evens, odds) = nums.partition(_ % 2 == 0)
// evens = List(4,2,6), odds = List(3,1,1,5,9,5,3)

// Flatten nested lists
List(List(1,2), List(3,4), List(5)).flatten
// List(1,2,3,4,5)

// flatMap (map + flatten combined)
List(1,2,3).flatMap(x => List(x, x * 10))
// List(1,10,2,20,3,30)

// Zip two lists together
List(1,2,3).zip(List('a','b','c'))
// List((1,a),(2,b),(3,c))

// Sliding window
List(1,2,3,4,5).sliding(3).toList
// List(List(1,2,3), List(2,3,4), List(3,4,5))
```

---

## ⚡ TRICK — Range Operations

```scala
// 1 to 100
(1 to 100).sum           // 5050  ← Instant! No loop needed!
(1 to 100).product       // Factorial-like product
(1 to 10).toList         // List(1,2,...,10)

// Sum of even numbers 1 to 100
(1 to 100).filter(_ % 2 == 0).sum   // 2550

// Sum of squares 1 to 10
(1 to 10).map(x => x * x).sum       // 385
```

---

## 🧒 Feynman Summary

> `List` = Ordered bag (can't change)
> `Array` = Ordered bag (can change)
> `Map` = Dictionary (word → definition)
> `Set` = Bag with NO duplicates
> `map/filter/reduce` = Your 3 POWER TOOLS 🔧
> Ranges + `.sum` = Instant math without loops!

---

## 🔗 See Also
- Previous: [[05-Control-Flow]]
- Next: [[07-Functions]]
- Advanced: [[08-Advanced-Pro-Tricks]]
- Cheatsheet: [[09-Speed-Cheatsheet]]
