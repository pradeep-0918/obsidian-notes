# 07 — Functions 🛠️
#scala #intermediate #functions #trick

← [[06-Loops-And-Collections]] | Next → [[08-Advanced-Pro-Tricks]]

---

## 🧒 Feynman Explain

> A **function** = A recipe 📋
> You write the recipe once → Use it 1000 times!
> Input (ingredients) → Function (cook) → Output (dish)
> Like a machine: put something IN, get something OUT!

---

## 🛠️ Basic Function Syntax

```scala
// def functionName(param: Type): ReturnType = body

def add(a: Int, b: Int): Int = {
  a + b   // Last expression is automatically returned!
}

println(add(3, 5))   // 8
```

> ⚡ **TRICK:** No `return` keyword needed! Last line IS the return value!

---

## ⚡ TRICK — One-Line Functions

```scala
// Multi-line (slow):
def square(x: Int): Int = {
  x * x
}

// ⚡ ONE LINE (fast!):
def square(x: Int): Int = x * x

// Even shorter — let Scala infer return type:
def square(x: Int) = x * x

// Call it:
square(7)    // 49
```

---

## ⚡ TRICK — Default Parameters

```scala
def greet(name: String, greeting: String = "Hello"): String =
  s"$greeting, $name!"

greet("Arjun")           // "Hello, Arjun!"   (uses default)
greet("Priya", "Hi")    // "Hi, Priya!"       (custom greeting)
```

---

## ⚡ TRICK — Named Arguments

```scala
def power(base: Int, exp: Int): Double = math.pow(base, exp)

// Normal call:
power(2, 10)              // 1024.0

// Named call — order doesn't matter!
power(exp = 10, base = 2) // 1024.0  ← Same result!
```

---

## ⚡ TRICK — Variable Arguments (varargs)

```scala
def sumAll(nums: Int*): Int = nums.sum

sumAll(1, 2, 3)           // 6
sumAll(1, 2, 3, 4, 5)     // 15
sumAll(10, 20)             // 30

// Pass a list with _*:
val myList = List(1, 2, 3, 4, 5)
sumAll(myList: _*)         // 15
```

---

## 🔄 Recursion — Functions that call themselves!

```scala
// 🧒 Think of it like Russian dolls 🪆
// Big problem → breaks into smaller version of SAME problem

// Factorial (n! = n × (n-1) × ... × 1)
def factorial(n: Int): BigInt =
  if (n <= 1) 1
  else n * factorial(n - 1)

factorial(5)    // 120  (5×4×3×2×1)
factorial(10)   // 3628800

// Fibonacci
def fib(n: Int): Int =
  if (n <= 1) n
  else fib(n - 1) + fib(n - 2)

fib(10)   // 55
```

---

## ⚡ TRICK — Tail Recursion (Fast Recursion!)

```scala
import scala.annotation.tailrec

// Regular recursion can crash for big N
// Tail recursion = NO stack overflow!

@tailrec
def factorial(n: Int, acc: BigInt = 1): BigInt =
  if (n <= 1) acc
  else factorial(n - 1, n * acc)   // Accumulator!

factorial(10000)  // Works! (regular would crash)

// ⚡ RULE: Put the accumulator as last argument with default value
```

---

## 🏹 Lambda Functions (Anonymous Functions)

```scala
// Named function:
def double(x: Int): Int = x * 2

// Lambda (arrow function):
val double = (x: Int) => x * 2
double(5)   // 10

// Even shorter with _ (underscore shorthand!):
val double: Int => Int = _ * 2

// Multiple params:
val add = (a: Int, b: Int) => a + b
add(3, 4)   // 7
```

---

## ⚡ TRICK — The Underscore Shorthand _ (Level Up!)

```scala
// Instead of: x => x * 2
// Write: _ * 2

List(1,2,3).map(x => x * 2)     // Verbose
List(1,2,3).map(_ * 2)          // ⚡ SHORT!

// Instead of: x => x > 5
List(1,2,3,4,5,6).filter(_ > 5)  // List(6)

// Instead of: (a, b) => a + b
List(1,2,3,4).reduce(_ + _)     // 10

// The _ substitutes each parameter in order!
```

---

## 🧰 Higher-Order Functions (Functions of Functions)

```scala
// A function that TAKES another function as parameter
def applyTwice(f: Int => Int, x: Int): Int = f(f(x))

applyTwice(_ * 2, 3)    // 12  (3→6→12)
applyTwice(_ + 10, 5)   // 25  (5→15→25)

// A function that RETURNS a function
def multiplier(factor: Int): Int => Int = _ * factor

val triple = multiplier(3)
triple(5)    // 15
triple(10)   // 30

val quadruple = multiplier(4)
quadruple(5) // 20
```

---

## ⚡ TRICK — Compose Functions

```scala
val double = (x: Int) => x * 2
val addOne = (x: Int) => x + 1

// Compose: apply addOne THEN double
val doubleThenAdd = double andThen addOne
doubleThenAdd(5)   // 11  (5*2=10, 10+1=11)

// Or reverse order:
val addThenDouble = double compose addOne
addThenDouble(5)   // 12  (5+1=6, 6*2=12)
```

---

## ⚡ TRICK — Memoization (Cache results!)

```scala
import scala.collection.mutable

// Store results so we don't recalculate!
val memo = mutable.Map[Int, Long]()

def fib(n: Int): Long = memo.getOrElseUpdate(n, {
  if (n <= 1) n
  else fib(n-1) + fib(n-2)
})

fib(100)   // Instant! Without memo, this would take forever
```

---

## 🧒 Feynman Summary

> Function = Recipe (input → process → output)
> `def name(params): ReturnType = body`
> Last line = automatic return value
> `_` shorthand = write less code, do more
> Recursion = function calls itself for smaller problems
> Higher-order = functions that work with other functions

---

## 🔗 See Also
- Previous: [[06-Loops-And-Collections]]
- Next: [[08-Advanced-Pro-Tricks]]
- Math tricks: [[03-Basic-Math-Operations]]
- Full cheatsheet: [[09-Speed-Cheatsheet]]
