# 🧩 06 — Functions

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[05-Strings-Math|← Previous: Strings & Math]]**

## 🧠 Feynman Explanation
> **A function is a machine.** You put ingredients in (parameters), the machine processes them, and outputs a product (return value). In Scala, functions are FIRST-CLASS CITIZENS — you can put them in variables, pass them to other functions, and return them. Functions are as powerful as any data!

---

## 📐 Function Basics

### Defining Functions
```scala
// def keyword, name, params, return type, body
def add(x: Int, y: Int): Int = {
  x + y  // Last expression = return value (no 'return' needed!)
}

// Single expression — no braces needed
def add(x: Int, y: Int): Int = x + y

// With explicit return (avoid this style)
def add(x: Int, y: Int): Int = return x + y

// No return value
def greet(name: String): Unit = println(s"Hello, $name!")

// Type inference on return type (omit ": Int")
def multiply(x: Int, y: Int) = x * y  // Scala infers Int
```

---

## 🎯 Parameters — All Flavors

### Default Parameters
```scala
def greet(name: String, greeting: String = "Hello"): String =
  s"$greeting, $name!"

greet("Pradeep")              // "Hello, Pradeep!"
greet("Pradeep", "Namaste")   // "Namaste, Pradeep!"
```

### Named Parameters
```scala
def createUser(name: String, age: Int, isAdmin: Boolean = false): String =
  s"User: $name, Age: $age, Admin: $isAdmin"

// Call with named params — order doesn't matter!
createUser(age = 21, name = "Pradeep")
createUser(name = "Pradeep", isAdmin = true, age = 21)
```

### Variadic Parameters (varargs)
```scala
def sum(nums: Int*): Int = nums.sum

sum(1, 2, 3)        // 6
sum(1, 2, 3, 4, 5)  // 15
sum()               // 0

// Pass a collection with :_*
val numbers = List(1, 2, 3, 4)
sum(numbers: _*)    // 10
```

### By-Name Parameters (Lazy Evaluation)
```scala
// Normal: expression evaluated BEFORE function call
def double(x: Int): Int = x * 2

// By-name: expression evaluated INSIDE function, each time it's used
def repeat(x: => Int): Unit = {
  println(x)  // Evaluates x
  println(x)  // Evaluates x AGAIN (could be different!)
}

var counter = 0
repeat { counter += 1; counter }
// Prints: 1
//         2
```

---

## 🔥 First-Class Functions — The Big Deal

```scala
// Functions ARE values — store in variables!
val addFn: (Int, Int) => Int = (x, y) => x + y
addFn(3, 4)  // 7

// Pass functions to other functions
def applyTwice(f: Int => Int, x: Int): Int = f(f(x))
applyTwice(x => x * 2, 3)  // 12  (3*2=6, 6*2=12)

// Return functions from functions
def multiplier(factor: Int): Int => Int = x => x * factor
val triple = multiplier(3)
triple(5)  // 15
```

---

## λ Lambda / Anonymous Functions

```scala
// Lambda syntax: (params) => body
val square: Int => Int = x => x * x
val add: (Int, Int) => Int = (x, y) => x + y

// Underscore shorthand (when param used once)
val square2: Int => Int = _ * _  // Wait, this is wrong!
val square3: Int => Int = _ * 2  // Hmm, that's double
// Use explicit:
val sq: Int => Int = x => x * x

// Underscore works great with collections:
List(1,2,3).map(_ * 2)      // List(2, 4, 6)
List(1,2,3).filter(_ > 1)   // List(2, 3)
List(1,2,3).map(_ + 10)     // List(11, 12, 13)
```

### Lambda Types (Function Types)
```scala
// Function1: A => B
val f1: Int => String = x => s"Number $x"

// Function2: (A, B) => C
val f2: (Int, Int) => Boolean = (x, y) => x > y

// Function0: () => A (no input)
val f0: () => Int = () => 42
f0()  // 42
```

---

## 🏗️ Higher-Order Functions (HOF)

> Functions that TAKE or RETURN other functions.

```scala
// Most important HOFs on collections
val nums = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

// map — transform each element
nums.map(x => x * x)           // List(1, 4, 9, 16, 25, 36, 49, 64, 81, 100)
nums.map(_ * 2)                  // List(2, 4, 6, 8, 10, ...)

// filter — keep matching elements
nums.filter(_ % 2 == 0)         // List(2, 4, 6, 8, 10)
nums.filter(x => x > 5)         // List(6, 7, 8, 9, 10)

// reduce — combine all into one (no starting value)
nums.reduce(_ + _)               // 55
nums.reduce(_ * _)               // 3628800 (10 factorial)

// fold — like reduce but with STARTING value
nums.foldLeft(0)(_ + _)          // 55
nums.foldLeft(1)(_ * _)          // 3628800
nums.foldRight(0)(_ + _)         // 55

// flatMap — map then flatten
List(1,2,3).flatMap(x => List(x, x*2))  // List(1,2,2,4,3,6)

// foreach — just do something (returns Unit)
nums.foreach(println)

// exists — any match?
nums.exists(_ > 5)   // true

// forall — all match?
nums.forall(_ > 0)   // true

// find — first match
nums.find(_ > 5)     // Some(6)
```

---

## 🔄 Recursion — Functions Calling Themselves

```scala
// Factorial with recursion
def factorial(n: Int): Long =
  if (n <= 1) 1L
  else n * factorial(n - 1)

factorial(5)  // 120

// Fibonacci
def fib(n: Int): Int = n match {
  case 0 => 0
  case 1 => 1
  case _ => fib(n-1) + fib(n-2)
}
```

### Tail Recursion (The Efficient Way!)
```scala
import scala.annotation.tailrec

// @tailrec ensures the compiler optimizes this to a loop
@tailrec
def factorial(n: Int, acc: Long = 1L): Long =
  if (n <= 1) acc
  else factorial(n - 1, n * acc)  // Last call is the recursive call

factorial(10)  // 3628800
```

> **Why tailrec?** Regular recursion creates a stack frame for each call — deep recursion = stack overflow! Tail recursion is rewritten as a loop by the compiler — no stack growth!

---

## 🧊 Currying — Functions in Stages

```scala
// Normal function
def add(x: Int, y: Int): Int = x + y

// Curried version
def addCurried(x: Int)(y: Int): Int = x + y

addCurried(3)(4)  // 7

// Partial application — fill in one argument now
val add5 = addCurried(5)  // Returns a function!
add5(10)  // 15
add5(20)  // 25

// Convert normal to curried
val addC = (add _).curried
addC(3)(4)  // 7
```

---

## 🔒 Closures

```scala
// A closure "closes over" variables from its outer scope
def makeCounter(): () => Int = {
  var count = 0
  () => { count += 1; count }  // Captures 'count'!
}

val counter = makeCounter()
counter()  // 1
counter()  // 2
counter()  // 3

// Each call to makeCounter creates INDEPENDENT counters
val c1 = makeCounter()
val c2 = makeCounter()
c1()  // 1  (c1's count)
c1()  // 2
c2()  // 1  (c2's own count)
```

---

## 🧩 Function Composition

```scala
val double: Int => Int = _ * 2
val addOne: Int => Int = _ + 1

// andThen: apply left first, then right
val doubleThenAdd = double andThen addOne
doubleThenAdd(5)  // (5*2) + 1 = 11

// compose: apply right first, then left
val addThenDouble = double compose addOne
addThenDouble(5)  // (5+1) * 2 = 12
```

---

## 📊 Functions Summary Table

| Type | Syntax | Use Case |
|------|--------|----------|
| Named function | `def f(x: Int): Int = x + 1` | Reusable named logic |
| Lambda | `x => x + 1` or `_ + 1` | Short, inline logic |
| HOF | Takes/returns function | Map, filter, transform |
| Recursive | Calls itself | Tree traversal, math |
| Tail-recursive | `@tailrec` | Safe deep recursion |
| Curried | `f(x)(y)` | Partial application |
| Closure | Captures outer vars | Stateful functions |

---

## 🧪 Practice Exercises

1. Write a HOF `applyN(f, n, x)` that applies function `f` to `x` exactly `n` times
2. Implement `myMap` using `foldLeft` (reimplement the `map` function yourself)
3. Write a tail-recursive Fibonacci function
4. Create a curried `power(base)(exponent)` function

---

## 🏷️ Tags
#scala #functions #lambda #hof #recursion #currying #closures #functional-programming #intermediate

## 🔗 Related Notes
- [[04-Control-Flow]] — Control flow inside functions
- [[07-Collections]] — HOF on collections
- [[08-OOP-Concepts]] — Methods vs functions
- [[09-Advanced-Scala]] — Implicits, type classes
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
