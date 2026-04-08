# 05 — Control Flow 🚦
#scala #beginner #intermediate #trick

← [[04-Strings-And-Input]] | Next → [[06-Loops-And-Collections]]

---

## 🧒 Feynman Explain

> Control Flow = **Decision making** 🤔
> Like traffic lights 🚦:
> - 🟢 Green → Go this way
> - 🔴 Red → Go another way
> Scala does the SAME — checks conditions and decides what to do!

---

## 🔀 if / else — The Basic Decision

```scala
val marks = 85

// Basic if-else
if (marks >= 90) {
  println("Grade A")
} else if (marks >= 80) {
  println("Grade B")
} else if (marks >= 70) {
  println("Grade C")
} else {
  println("Grade D")
}
// Output: Grade B
```

---

## ⚡ TRICK — if is an Expression! (Returns a value!)

```scala
// In Scala, if-else RETURNS a value — use it directly!
val marks = 85

// SLOW WAY:
var grade = ""
if (marks >= 90) grade = "A"
else if (marks >= 80) grade = "B"
else grade = "C"

// ⚡ FAST WAY (one expression!):
val grade = if (marks >= 90) "A" else if (marks >= 80) "B" else "C"
println(grade)  // B

// Even simpler for binary choice:
val result = if (marks >= 50) "Pass" else "Fail"
```
> 🔥 This is like Python's ternary — saves 5 lines every time!

---

## 🎭 match — Scala's Super Switch!

```scala
val day = 3

val dayName = day match {
  case 1 => "Monday"
  case 2 => "Tuesday"
  case 3 => "Wednesday"
  case 4 => "Thursday"
  case 5 => "Friday"
  case 6 => "Saturday"
  case 7 => "Sunday"
  case _ => "Invalid"   // _ is the DEFAULT case
}
println(dayName)  // Wednesday
```

---

## ⚡ TRICK — match with Guards (Conditions!)

```scala
val n = 42

val description = n match {
  case 0                  => "Zero"
  case x if x < 0        => "Negative"
  case x if x % 2 == 0   => s"$x is Even"   // Guards!
  case x                  => s"$x is Odd"
}
println(description)  // 42 is Even
```

---

## ⚡ TRICK — match Multiple Cases at Once

```scala
val month = 4

val season = month match {
  case 12 | 1 | 2  => "Winter ❄️"   // | means OR
  case 3 | 4 | 5   => "Spring 🌸"
  case 6 | 7 | 8   => "Summer ☀️"
  case 9 | 10 | 11 => "Autumn 🍂"
  case _           => "Invalid month"
}
println(season)  // Spring 🌸
```

---

## ⚡ TRICK — match on Ranges

```scala
val score = 75

val grade = score match {
  case s if s >= 90 => "A"
  case s if s >= 80 => "B"
  case s if s >= 70 => "C"
  case s if s >= 60 => "D"
  case _            => "F"
}
println(grade)   // C
```

---

## 🔗 Boolean Operators

```scala
val a = true
val b = false

a && b    // AND → false (both must be true)
a || b    // OR  → true  (at least one true)
!a        // NOT → false (flips it)

// Short-circuit evaluation:
// && stops at first false
// || stops at first true

// Comparisons:
5 > 3      // true
5 < 3      // false
5 >= 5     // true
5 <= 4     // false
5 == 5     // true  (equal)
5 != 4     // true  (not equal)
```

---

## ⚡ TRICK — Chained Comparisons (Scala Way)

```scala
val x = 15

// Check if x is between 10 and 20:
val inRange = x > 10 && x < 20    // true

// Check if year is a leap year (1 line!):
def isLeapYear(y: Int): Boolean =
  (y % 4 == 0 && y % 100 != 0) || (y % 400 == 0)

isLeapYear(2024)   // true
isLeapYear(1900)   // false
isLeapYear(2000)   // true
```

---

## ⚡ TRICK — Option (No More Null Errors!)

```scala
// The safe way to handle "maybe no value"
val found: Option[Int] = Some(42)   // Has a value
val empty: Option[Int] = None       // No value (safe null)

// Use getOrElse to provide default:
found.getOrElse(0)   // 42
empty.getOrElse(0)   // 0  (safe!)

// Use in match:
found match {
  case Some(v) => println(s"Found: $v")
  case None    => println("Not found")
}
```

---

## 🧒 Feynman Summary

> `if/else` = Traffic light (pick one road)
> `match` = Menu selector (pick from many options)
> `&&` = Both must be true | `||` = At least one true | `!` = Flip it
> `Option` = Safe way to say "maybe has value, maybe not"
> ⚡ In Scala, `if-else` RETURNS a value — use it!

---

## 🔗 See Also
- Previous: [[04-Strings-And-Input]]
- Next: [[06-Loops-And-Collections]]
- Advanced patterns: [[08-Advanced-Pro-Tricks]]
- Quick reference: [[09-Speed-Cheatsheet]]
