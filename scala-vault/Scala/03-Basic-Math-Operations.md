# 03 — Basic Math Operations 🔢
#scala #math #trick #beginner #intermediate

← [[02-Variables-And-Types]] | Next → [[04-Strings-And-Input]]

---

## 🧒 Feynman Explain

> Scala is a **super calculator**.
> Everything a calculator does + more!
> Let's learn every math trick so you solve problems in **1 SECOND** ⚡

---

## ➕ The 7 Basic Operators

```scala
val a = 20
val b = 6

println(a + b)    // 26  → Addition
println(a - b)    // 14  → Subtraction
println(a * b)    // 120 → Multiplication
println(a / b)    // 3   → Division (Int! drops remainder)
println(a % b)    // 2   → Modulo (REMAINDER) ← Very important!
println(a.max(b)) // 20  → Maximum of two
println(a.min(b)) // 6   → Minimum of two
```

---

## ⚡ TRICK — Integer Division vs Double Division

```scala
// INTEGER DIVISION (drops decimal)
val x = 7 / 2       // Result: 3  (NOT 3.5!)

// DOUBLE DIVISION (keeps decimal)
val y = 7.0 / 2     // Result: 3.5
val z = 7 / 2.0     // Result: 3.5
val w = 7.toDouble / 2  // Result: 3.5 ← CLEANEST WAY

// ⚡ TRICK: Add .0 to ONE number → gets decimal answer
```

---

## ⚡ TRICK — Modulo (%) is Your Best Friend!

```scala
// % gives the REMAINDER
10 % 3    // = 1  (10 = 3×3 + 1)
15 % 5    // = 0  (perfectly divisible!)
7  % 2    // = 1  (odd number!)
8  % 2    // = 0  (even number!)

// ⚡ TRICKS with Modulo:
val n = 17

// Is it even or odd?
if (n % 2 == 0) "Even" else "Odd"    // "Odd"

// Last digit of any number
val lastDigit = n % 10               // 7

// Is it divisible by 5?
if (n % 5 == 0) "Yes" else "No"     // "No"

// Wrap numbers (like clock!)
val hour = 14 % 12   // 2 (14:00 = 2 PM in 12-hour clock)
```

---

## 🔢 Math Library — scala.math

```scala
import scala.math._    // Import ALL math functions at once!

// Powers
pow(2, 10)    // 1024.0  (2^10)
pow(3, 3)     // 27.0    (3^3 = 27)

// Square root
sqrt(144)     // 12.0
sqrt(2)       // 1.4142135...

// Absolute value
abs(-42)      // 42
abs(42)       // 42

// Ceiling & Floor
ceil(3.2)     // 4.0  (round UP always)
floor(3.9)    // 3.0  (round DOWN always)

// Round
round(3.5)    // 4    (normal rounding)
round(3.4)    // 3

// Log
log(math.E)   // 1.0  (natural log)
log10(1000)   // 3.0  (log base 10)

// Trig
sin(math.Pi / 2)   // 1.0
cos(0)             // 1.0

// Min / Max of two values
max(10, 20)   // 20
min(10, 20)   // 10
```

---

## ⚡ TRICK — Compound Assignment (Save Time!)

```scala
var x = 10

// SLOW WAY:
x = x + 5    // x is now 15
x = x - 3    // x is now 12
x = x * 2    // x is now 24
x = x / 4    // x is now 6

// FAST WAY (same result, fewer chars!):
var x = 10
x += 5    // x = 15
x -= 3    // x = 12
x *= 2    // x = 24
x /= 4    // x = 6
```

---

## ⚡ TRICK — BigInt for HUGE Numbers

```scala
// Regular Int maxes out at ~2 billion
// BigInt handles numbers of ANY size!

val huge = BigInt("999999999999999999999999999")
val result = huge * huge
println(result)   // Works perfectly! No overflow!

// Factorial of 100 (impossible with Int!)
def factorial(n: Int): BigInt =
  if (n <= 1) BigInt(1) else BigInt(n) * factorial(n - 1)

println(factorial(100))  // 93326215443944152681699238...
```

---

## ⚡ TRICK — Number Formatting

```scala
val pi = 3.14159265358979

// Round to N decimal places
val rounded = f"$pi%.2f"    // "3.14"  (2 decimals)
val rounded3 = f"$pi%.3f"   // "3.142" (3 decimals)

// Format big numbers with commas
val big = 1000000
val formatted = f"$big%,d"   // "1,000,000"

// Scientific notation
val sci = 0.000123
val sciStr = f"$sci%e"       // "1.230000e-04"
```

---

## ⚡ TRICK — Chained Math in One Line

```scala
// Instead of multiple lines:
val a = 5
val b = 3
val sum = a + b
val doubled = sum * 2
val result = doubled - 1

// One-liner:
val result = (5 + 3) * 2 - 1   // 15 ← Done in 1 line!
```

---

## 🔢 Operator Precedence (BODMAS in Scala!)

```
1st: ()  Brackets
2nd: * / %  (Multiply, Divide, Modulo)
3rd: + -  (Add, Subtract)
```

```scala
2 + 3 * 4       // = 14  (3*4 first, then +2)
(2 + 3) * 4     // = 20  (brackets first!)
10 - 4 / 2      // = 8   (4/2=2, then 10-2)
10 % 3 + 1      // = 2   (10%3=1, then 1+1)
```

---

## 🧮 Common Problem Patterns (Instant Solvers!)

```scala
import scala.math._

// ✅ Is N a perfect square?
def isPerfectSquare(n: Int): Boolean = {
  val s = sqrt(n.toDouble).toInt
  s * s == n
}
isPerfectSquare(16)  // true
isPerfectSquare(15)  // false

// ✅ GCD (Greatest Common Divisor)
def gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
gcd(12, 8)   // 4

// ✅ LCM (Least Common Multiple)
def lcm(a: Int, b: Int): Int = a / gcd(a, b) * b
lcm(4, 6)    // 12

// ✅ Sum of digits
def sumDigits(n: Int): Int = n.toString.map(_.asDigit).sum
sumDigits(123)   // 6 (1+2+3)

// ✅ Reverse a number
def reverseNum(n: Int): Int = n.toString.reverse.toInt
reverseNum(123)  // 321

// ✅ Is it a palindrome number?
def isPalindrome(n: Int): Boolean = {
  val s = n.toString
  s == s.reverse
}
isPalindrome(121)  // true
isPalindrome(123)  // false

// ✅ Count digits
def countDigits(n: Int): Int = n.toString.length
countDigits(12345)  // 5
```

---

## 🧒 Feynman Summary

> `+ - * /` = Basic calculator
> `%` = Remainder → checks even/odd, divisibility, last digit
> `scala.math._` = Your math toolkit
> `BigInt` = For GIANT numbers
> Know BODMAS → no mistakes!

---

## 🔗 See Also
- Previous: [[02-Variables-And-Types]]
- Next: [[04-Strings-And-Input]]
- Loops for sums: [[06-Loops-And-Collections]]
- Quick reference: [[09-Speed-Cheatsheet]]
