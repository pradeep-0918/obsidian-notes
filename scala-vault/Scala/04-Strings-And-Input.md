# 04 — Strings & Input 💬
#scala #beginner #string #trick

← [[03-Basic-Math-Operations]] | Next → [[05-Control-Flow]]

---

## 🧒 Feynman Explain

> A **String** is a necklace of characters 📿
> `"Hello"` = H + e + l + l + o (5 beads)
> You can cut it, join it, search it — like playing with beads!

---

## 🔤 String Basics

```scala
val name = "Arjun"
val city = "Chennai"

// Join strings (Concatenation)
val greeting = name + " lives in " + city
// = "Arjun lives in Chennai"

// Length
name.length          // 5

// Uppercase / Lowercase
name.toUpperCase     // "ARJUN"
name.toLowerCase     // "arjun"

// Access a character (0-indexed!)
name(0)              // 'A'  ← first character
name(4)              // 'n'  ← last character

// Check if contains
name.contains("jun")  // true
```

---

## ⚡ TRICK — String Interpolation (The PRO Way!)

```scala
val name = "Ravi"
val score = 95
val grade = 'A'

// OLD way (messy):
println("Name: " + name + ", Score: " + score)

// ⚡ s"" interpolation (FAST & CLEAN!):
println(s"Name: $name, Score: $score, Grade: $grade")
// Output: Name: Ravi, Score: 95, Grade: A

// For expressions, use ${}:
println(s"Double score: ${score * 2}")   // Double score: 190
println(s"Name length: ${name.length}")  // Name length: 4
```

---

## ⚡ TRICK — f"" for Formatted Numbers

```scala
val pi = 3.14159

// f-strings for number formatting:
println(f"Pi = $pi%.2f")      // Pi = 3.14
println(f"Pi = $pi%.4f")      // Pi = 3.1416
println(f"Score: $score%05d") // Score: 00095 (pad with zeros)
```

---

## ⚡ TRICK — raw"" for Escape Characters

```scala
// Normal string — \n means newline
println("Hello\nWorld")
// Hello
// World

// raw string — \n is LITERAL (not escape)
println(raw"Hello\nWorld")
// Hello\nWorld  (prints the \n literally)

// Triple quotes — multiline strings!
val poem = """
  Roses are red,
  Scala is great,
  Learning it daily,
  Is never too late!
"""
println(poem)
```

---

## 🔪 String Methods — Must Know!

```scala
val s = "  Hello, Scala World!  "

s.trim              // "Hello, Scala World!"  (removes spaces)
s.trim.length       // 21

val s2 = "Hello, Scala World!"
s2.split(", ")      // Array("Hello", "Scala World!")
s2.split(" ")       // Array("Hello,", "Scala", "World!")

s2.replace("Scala", "Amazing")   // "Hello, Amazing World!"
s2.startsWith("Hello")           // true
s2.endsWith("!")                  // true
s2.indexOf("Scala")              // 7  (position)

// Substring
s2.substring(7, 12)   // "Scala"  (from index 7 to 11)
s2.take(5)            // "Hello"  (first 5 chars)
s2.drop(7)            // "Scala World!"  (skip first 7)
s2.takeRight(6)       // "World!"  (last 6 chars)
s2.dropRight(6)       // "Hello, Scala "

// Reverse a string
"Scala".reverse       // "alacS"

// Count occurrences
"banana".count(_ == 'a')   // 3  (counts 'a' in banana)
```

---

## ⚡ TRICK — Check Anagram in 1 Line!

```scala
def isAnagram(a: String, b: String): Boolean =
  a.sorted == b.sorted

isAnagram("listen", "silent")   // true
isAnagram("hello", "world")    // false
```

---

## ⚡ TRICK — Check Palindrome String

```scala
def isPalindrome(s: String): Boolean = {
  val clean = s.toLowerCase.filter(_.isLetter)
  clean == clean.reverse
}

isPalindrome("racecar")          // true
isPalindrome("A man a plan")     // true (ignores spaces)
isPalindrome("hello")            // false
```

---

## 📥 Reading User Input

```scala
import scala.io.StdIn._

// Read a line of text
val name = readLine("Enter your name: ")
println(s"Hello, $name!")

// Read an integer
val age = readInt()
println(s"You are $age years old")

// Read a double
val height = readDouble()

// Read and convert in one line
val num = readLine("Enter number: ").toInt
```

---

## ⚡ TRICK — Parse Multiple Numbers from One Line

```scala
import scala.io.StdIn._

// User types: "10 20 30"
val numbers = readLine().split(" ").map(_.toInt)
// numbers = Array(10, 20, 30)

val sum = numbers.sum          // 60
val max = numbers.max          // 30
val min = numbers.min          // 10

// ⚡ One line to read and sum:
val total = readLine().split(" ").map(_.toInt).sum
```

---

## 🧒 Feynman Summary

> String = Necklace of characters 📿
> `s"$var"` = Best way to print variables in text
> `.split()` = Cut string into pieces
> `.trim` = Remove extra spaces
> `.reverse`, `.sorted` = 1-line palindrome & anagram checks!

---

## 🔗 See Also
- Previous: [[03-Basic-Math-Operations]]
- Next: [[05-Control-Flow]]
- Functions using strings: [[07-Functions]]
- Full tricks: [[09-Speed-Cheatsheet]]
