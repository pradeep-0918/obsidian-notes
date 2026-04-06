# 09 — ⚡ Speed Cheatsheet — Solve in 1 Second!
#scala #trick #shortcut #cheatsheet

← [[08-Advanced-Pro-Tricks]] | Home → [[00-MOC-Scala-Master]]

---

> 🏆 **This is your ULTIMATE reference card**
> Memorize these patterns → Solve any question INSTANTLY!

---

## 🔢 NUMBER TRICKS

```scala
import scala.math._

// ✅ Even/Odd
n % 2 == 0          // true = even

// ✅ Last digit
n % 10              // last digit of n

// ✅ Remove last digit
n / 10              // removes last digit

// ✅ Digit sum
n.toString.map(_.asDigit).sum

// ✅ Digit count
n.toString.length

// ✅ Reverse a number
n.toString.reverse.toInt

// ✅ Is palindrome number
n.toString == n.toString.reverse

// ✅ GCD (Euclid's algorithm)
def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)

// ✅ LCM
def lcm(a: Int, b: Int): Int = a / gcd(a, b) * b

// ✅ Power
pow(base, exp).toLong    // base^exp

// ✅ Square root (integer)
sqrt(n.toDouble).toInt

// ✅ Is perfect square?
val s = sqrt(n.toDouble).toInt; s * s == n

// ✅ Absolute value
abs(n)

// ✅ Max/Min of two
a.max(b)    // same as max(a, b)
a.min(b)

// ✅ Factorial (one-liner!)
(1 to n).product            // ⚠️ Int overflow for n > 12
(1 to n).map(BigInt(_)).product  // Safe for any n

// ✅ Sum 1 to N (Gauss formula)
n * (n + 1) / 2

// ✅ Sum of squares 1 to N
n * (n + 1) * (2 * n + 1) / 6

// ✅ Sum of cubes 1 to N
val s = n * (n + 1) / 2; s * s
```

---

## 🔡 STRING TRICKS

```scala
// ✅ Reverse
s.reverse

// ✅ Is palindrome
s == s.reverse

// ✅ Sort characters
s.sorted            // "scala" → "aacls"

// ✅ Is anagram
a.sorted == b.sorted

// ✅ Count char occurrences
s.count(_ == 'a')

// ✅ Remove spaces
s.filter(_ != ' ')

// ✅ Only letters
s.filter(_.isLetter)

// ✅ Only digits
s.filter(_.isDigit)

// ✅ Convert to Int
"123".toInt

// ✅ Frequency map
s.groupBy(identity).map { case (c, s) => c -> s.length }

// ✅ Most frequent character
s.groupBy(identity).maxBy(_._2.length)._1
```

---

## 📦 COLLECTION TRICKS

```scala
val lst = List(3, 1, 4, 1, 5, 9, 2, 6)

// ✅ Sum
lst.sum

// ✅ Max / Min
lst.max
lst.min

// ✅ Sort
lst.sorted             // ascending
lst.sorted.reverse     // descending

// ✅ Remove duplicates
lst.distinct

// ✅ Count matching
lst.count(_ > 4)

// ✅ All match?
lst.forall(_ > 0)

// ✅ Any match?
lst.exists(_ > 8)

// ✅ Find first match
lst.find(_ > 5)        // Some(9)

// ✅ Map (transform all)
lst.map(_ * 2)

// ✅ Filter (keep matching)
lst.filter(_ % 2 == 0)

// ✅ Flatten
List(List(1,2), List(3,4)).flatten

// ✅ Zip
List(1,2,3).zip(List('a','b','c'))

// ✅ Group by
lst.groupBy(_ % 2 == 0)

// ✅ Split into two lists
val (evens, odds) = lst.partition(_ % 2 == 0)

// ✅ Sliding window
lst.sliding(3).toList

// ✅ Running sum (prefix)
lst.scanLeft(0)(_ + _)

// ✅ Frequency count
lst.groupBy(identity).map { case (k, v) => k -> v.length }

// ✅ Most frequent element
lst.groupBy(identity).maxBy(_._2.length)._1

// ✅ 2nd largest
lst.distinct.sorted.reverse(1)

// ✅ Rotate list left by k
lst.drop(k) ++ lst.take(k)

// ✅ Check if sorted
lst == lst.sorted
```

---

## 🔁 RANGE TRICKS

```scala
// ✅ Sum 1..N
(1 to n).sum

// ✅ Sum of evens
(1 to n).filter(_ % 2 == 0).sum

// ✅ Sum of squares
(1 to n).map(x => x*x).sum

// ✅ Primes up to N
(2 to n).filter(isPrime)

// ✅ Countdown
(n to 1 by -1).toList

// ✅ Step by k
(0 to 100 by 5).toList

// ✅ List of digits
(0 to 9).toList
```

---

## 🏹 LAMBDA SHORTCUTS

```scala
// ✅ _ shorthand
_ * 2          // x => x * 2
_ > 5          // x => x > 5
_ + _          // (a, b) => a + b
_.toString     // x => x.toString
_.toInt        // x => x.toInt
_.length       // x => x.length
_.isEmpty      // x => x.isEmpty

// ✅ Method reference
List("1","2","3").map(_.toInt)   // List(1,2,3)
```

---

## ⚡ PROBLEM PATTERNS — INSTANT SOLVE

```scala
// ✅ Is prime?
def isPrime(n: Int): Boolean =
  n > 1 && (2 to sqrt(n.toDouble).toInt).forall(n % _ != 0)

// ✅ Fibonacci (fast with LazyList)
val fibs: LazyList[BigInt] = {
  def go(a: BigInt, b: BigInt): LazyList[BigInt] = a #:: go(b, a+b)
  go(0, 1)
}
fibs(n)   // nth Fibonacci instantly

// ✅ All divisors of n
(1 to n).filter(n % _ == 0)

// ✅ Is perfect number?
val divs = (1 until n).filter(n % _ == 0)
divs.sum == n

// ✅ Count primes up to N (Sieve of Eratosthenes)
def sieve(n: Int): List[Int] = {
  val composite = Array.fill(n + 1)(false)
  for {
    i <- 2 to math.sqrt(n).toInt
    if !composite(i)
    j <- i * i to n by i
  } composite(j) = true
  (2 to n).filter(!composite(_)).toList
}
sieve(50)   // List(2,3,5,7,11,13,17,19,23,29,31,37,41,43,47)

// ✅ Binary to Decimal
Integer.parseInt("1010", 2)    // 10

// ✅ Decimal to Binary
42.toBinaryString              // "101010"

// ✅ Decimal to Hex
255.toHexString                // "ff"

// ✅ Hex to Decimal
Integer.parseInt("ff", 16)     // 255

// ✅ Check Armstrong number (e.g. 153 = 1³+5³+3³)
def isArmstrong(n: Int): Boolean = {
  val digits = n.toString.map(_.asDigit)
  digits.map(d => pow(d, digits.length).toInt).sum == n
}
isArmstrong(153)   // true
isArmstrong(370)   // true

// ✅ Matrix transpose (2D)
val matrix = List(List(1,2,3), List(4,5,6), List(7,8,9))
matrix.transpose
// List(List(1,4,7), List(2,5,8), List(3,6,9))

// ✅ Flat sum of 2D list
matrix.flatten.sum   // 45
```

---

## 🧠 MENTAL MATH FORMULAS (Scala Edition)

| Problem | Formula | Code |
|---|---|---|
| Sum 1..N | N(N+1)/2 | `n*(n+1)/2` |
| Sum evens 1..N | N/2 × (N/2+1) | `(1 to n).filter(_%2==0).sum` |
| Sum odds 1..N | k² where k=count | `(1 to n).filter(_%2!=0).sum` |
| N! | Product | `(1 to n).map(BigInt(_)).product` |
| 2^N | Power of 2 | `1 << n` or `pow(2,n).toInt` |
| Digit sum | Iterate | `n.toString.map(_.asDigit).sum` |
| Reverse | String trick | `n.toString.reverse.toInt` |

---

## 🚀 ONE-LINE CHALLENGE SOLUTIONS

```scala
// Print 1 to N:
(1 to n).foreach(println)

// Sum of all even numbers in a list:
list.filter(_%2==0).sum

// Find duplicates in list:
list.groupBy(identity).filter(_._2.length > 1).keys.toList

// Intersection of two lists:
list1.toSet.intersect(list2.toSet).toList

// All permutations of a list:
list.permutations.toList

// All combinations of size k:
list.combinations(k).toList

// Transpose rows and columns:
matrix.transpose

// Flatten and unique:
matrix.flatten.distinct

// Sort by 2nd element of tuple:
list.sortBy(_._2)

// Group words by length:
words.groupBy(_.length)
```

---

## 📌 IMPORT CHEATSHEET

```scala
import scala.math._          // All math functions
import scala.util.{Try, Success, Failure}  // Safe computation
import scala.io.StdIn._      // Read input
import scala.collection.mutable  // Mutable collections
```

---

## 🔗 Deep Dive Links

- Math operations: [[03-Basic-Math-Operations]]
- Strings: [[04-Strings-And-Input]]
- Collections: [[06-Loops-And-Collections]]
- Functions: [[07-Functions]]
- Advanced: [[08-Advanced-Pro-Tricks]]
- Home: [[00-MOC-Scala-Master]]
