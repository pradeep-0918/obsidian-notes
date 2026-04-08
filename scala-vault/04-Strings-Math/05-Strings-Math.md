# 🔤 05 — Strings & Math

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[04-Control-Flow|← Previous: Control Flow]]**

## 🧠 Feynman Explanation
> **Strings are like necklaces — each bead is a character.** Scala lets you restring, cut, reverse, and decorate them easily. Math operations work exactly like a calculator, but with extra shortcuts.

---

## 🔤 STRINGS

### Creating Strings
```scala
val s1 = "Hello, World!"              // Basic string
val s2 = 'A'                          // NOT a string — it's a Char!
val multiline = """Line 1
Line 2
Line 3"""                              // Triple-quoted (raw, no escapes)
val clean = """  |First
               |Second""".stripMargin // | marks the true start
```

### String Interpolation (3 Modes)
```scala
val name = "Pradeep"
val score = 95.567

// s"" — insert variables
println(s"Hello $name, score = $score")

// f"" — formatted
println(f"Score: $score%.1f%%")  // "Score: 95.6%"

// raw"" — no escape processing
println(raw"Tab: \t Newline: \n")  // Literal \t and \n
```

---

## ✂️ String Operations Cheatsheet

```scala
val s = "Hello, Scala World!"

// Length & Access
s.length           // 19
s.charAt(0)        // 'H'
s(0)               // 'H'  (shorthand)
s.head             // 'H'  (first character)
s.last             // '!'  (last character)
s.tail             // "ello, Scala World!"

// Case
s.toUpperCase      // "HELLO, SCALA WORLD!"
s.toLowerCase      // "hello, scala world!"
s.capitalize       // "Hello, scala world!"  (first letter only)

// Trimming
"  hello  ".trim          // "hello"
"  hello  ".strip         // "hello" (Unicode-aware)
"  hello  ".stripLeading  // "hello  "
"  hello  ".stripTrailing // "  hello"

// Searching
s.contains("Scala")       // true
s.startsWith("Hello")     // true
s.endsWith("!")            // true
s.indexOf("Scala")        // 7  (-1 if not found)
s.lastIndexOf("l")        // 15

// Extracting
s.substring(7, 12)        // "Scala"
s.slice(7, 12)            // "Scala"  (same as substring)
s.take(5)                 // "Hello"
s.drop(7)                 // "Scala World!"
s.takeRight(6)            // "World!"
s.dropRight(6)            // "Hello, Scala "

// Splitting & Joining
"a,b,c".split(",")        // Array("a", "b", "c")
"a,b,c".split(",", 2)     // Array("a", "b,c")  (max 2 parts)
List("a","b","c").mkString(",")     // "a,b,c"
List("a","b","c").mkString("[",",","]") // "[a,b,c]"

// Replacing
s.replace("Scala", "Java")         // "Hello, Java World!"
s.replaceAll("\\s+", "_")          // "Hello,_Scala_World!"
s.replaceFirst("[aeiou]", "*")     // "H*llo, Scala World!"

// Checking
s.isEmpty                 // false
"".isEmpty                // true
s.nonEmpty                // true
"123".forall(_.isDigit)   // true
"abc123".exists(_.isDigit) // true

// Reversing
"hello".reverse           // "olleh"

// Counting
"banana".count(_ == 'a')  // 3

// Converting
"42".toInt                // 42
"3.14".toDouble           // 3.14
42.toString               // "42"
```

---

## 🔡 Char Operations

```scala
val c = 'A'
c.isLetter     // true
c.isDigit      // false
c.isWhitespace // false
c.isUpper      // true
c.isLower      // false
c.toLower      // 'a'
c.toUpper      // 'A'
c.toInt        // 65 (ASCII)
65.toChar      // 'A'
```

---

## 📝 Regex in Scala

```scala
import scala.util.matching.Regex

val emailRegex = """[\w.-]+@[\w.-]+\.\w{2,}""".r
val email = "user@example.com"

// Test match
emailRegex.matches(email)   // true

// Find first match
val text = "Call 9876543210 or 1234567890"
val phoneRegex = """\d{10}""".r
phoneRegex.findFirstIn(text)  // Some("9876543210")

// Find all matches
phoneRegex.findAllIn(text).toList  // List("9876543210", "1234567890")

// Extract groups
val dateRegex = """(\d{4})-(\d{2})-(\d{2})""".r
val date = "2024-03-15"
date match {
  case dateRegex(year, month, day) => println(s"Year: $year")
  case _ => println("No match")
}
```

---

## 🔢 MATH

### Basic Arithmetic
```scala
val a = 10
val b = 3

a + b    // 13
a - b    // 7
a * b    // 30
a / b    // 3    ← INTEGER division! (truncates)
a % b    // 1    ← remainder (modulo)
a.toDouble / b  // 3.333... ← Force float division

// Double arithmetic
val x = 10.0
val y = 3.0
x / y   // 3.3333...
```

### Scala Math Library
```scala
import scala.math._  // or java.lang.Math

// Basic
abs(-5)              // 5
max(10, 20)          // 20
min(10, 20)          // 10

// Powers & Roots
pow(2, 10)           // 1024.0  (2^10)
sqrt(144)            // 12.0
cbrt(27)             // 3.0  (cube root)

// Rounding
ceil(3.2)            // 4.0
floor(3.9)           // 3.0
round(3.5)           // 4  (rounds to Long)
rint(3.5)            // 4.0  (rounds to Double)

// Logarithms
log(Math.E)          // 1.0  (natural log)
log10(100)           // 2.0
log(8) / log(2)      // 3.0  (log base 2 of 8)

// Trigonometry
sin(Pi / 2)          // 1.0
cos(0)               // 1.0
tan(Pi / 4)          // 1.0
toDegrees(Pi)        // 180.0
toRadians(180)       // Pi

// Constants
Pi                   // 3.141592653589793
E                    // 2.718281828459045

// Random
Random.nextInt(10)   // Random Int 0-9
Random.nextDouble()  // Random Double 0.0-1.0
```

---

## 🧮 BigInt & BigDecimal — For Huge Numbers

```scala
// BigInt — unlimited precision integers
val bigNum = BigInt("99999999999999999999999999999")
bigNum * 2    // 199999999999999999999999999998

// BigDecimal — unlimited precision decimals
val precise = BigDecimal("3.1415926535897932384626433832")
precise * 2   // 6.2831853071795864769252867664

// Useful for financial calculations!
val price = BigDecimal("99.99")
val tax = BigDecimal("0.18")
val total = price + (price * tax)  // 117.9882
```

---

## 🎯 Common String Algorithms

```scala
// Palindrome check
def isPalindrome(s: String): Boolean = s == s.reverse

// Count words
def wordCount(s: String): Int = s.trim.split("\\s+").length

// Caesar cipher
def caesar(s: String, shift: Int): String =
  s.map(c => if (c.isLetter) {
    val base = if (c.isUpper) 'A' else 'a'
    ((c - base + shift) % 26 + base).toChar
  } else c)

// Anagram check
def isAnagram(s1: String, s2: String): Boolean =
  s1.toLowerCase.sorted == s2.toLowerCase.sorted

// Count occurrences of each character
def charFreq(s: String): Map[Char, Int] =
  s.groupBy(identity).map { case (c, cs) => c -> cs.length }
```

---

## 📊 Performance Tips

| Operation | Time Complexity |
|-----------|----------------|
| `s.length` | O(1) |
| `s.charAt(i)` | O(1) |
| `s + t` (concatenation) | O(n+m) — creates new String! |
| `StringBuilder` append | O(1) amortized |
| `s.contains(t)` | O(n*m) |
| `s.split(",")` | O(n) |

### StringBuilder for Heavy Concatenation
```scala
val sb = new StringBuilder()
for (i <- 1 to 1000) {
  sb.append(i)
  sb.append(", ")
}
val result = sb.toString()
// ✅ Much faster than string + in a loop!
```

---

## 🧪 Practice Exercises

1. Write a function that checks if a string is a palindrome (ignoring case and spaces)
2. Count the frequency of each character in "Mississippi"
3. Implement a simple Caesar cipher (shift by 3)
4. Format a price with 2 decimal places and currency symbol using string interpolation

---

## 🏷️ Tags
#scala #strings #math #regex #string-operations #bigint #feynman #intermediate

## 🔗 Related Notes
- [[03-Variables-Types]] — String is a type
- [[06-Functions]] — String manipulation functions
- [[07-Collections]] — Working with collections of strings
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
