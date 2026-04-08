# 🔢 03 — Variables & Types

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[02-Input-Output|← Previous: I/O]]**

## 🧠 Feynman Explanation
> **Variables are like labeled boxes.** `val` is a box you SEAL after putting something in (immutable). `var` is a box you can REOPEN and swap the contents (mutable). Scala STRONGLY prefers sealed boxes — it makes code safer!

---

## 🔑 val vs var — The Most Important Decision

```scala
val name = "Pradeep"   // Immutable — CANNOT change
var score = 100        // Mutable — CAN change

name = "Kumar"   // ❌ ERROR! val cannot be reassigned
score = 200      // ✅ Fine! var can change
```

### When to use which?
| Rule | Advice |
|------|--------|
| **Default** | Always use `val` first |
| **Use `var` only when** | You MUST update the value (counters, accumulators) |
| **Why prefer `val`?** | Safer, easier to reason about, functional programming style |

> **6th Grade Analogy:** `val` = carved in stone tablet. `var` = written on a whiteboard. Would you trust important info carved in stone or scribbled on a board? Use stone (val) by default!

---

## 🏷️ Type System — Scala's Type Hierarchy

```
Any (The root of ALL types)
├── AnyVal (Primitive-like)
│   ├── Int        (32-bit integer: -2B to 2B)
│   ├── Long       (64-bit integer: huge numbers)
│   ├── Double     (64-bit decimal: 3.14159)
│   ├── Float      (32-bit decimal: less precise)
│   ├── Char       (single character: 'A')
│   ├── Boolean    (true/false)
│   ├── Byte       (8-bit: -128 to 127)
│   └── Short      (16-bit: -32768 to 32767)
└── AnyRef (Reference types — like Java's Object)
    ├── String
    ├── List, Map, etc.
    └── Your custom classes
        └── Null (subtype of all AnyRef)

Nothing — The bottom type (subtype of EVERYTHING)
```

---

## 📊 All Types at a Glance

```scala
// AnyVal types
val i: Int = 42
val l: Long = 1_000_000_000L    // L suffix for Long
val d: Double = 3.14159
val f: Float = 3.14f            // f suffix for Float
val c: Char = 'A'               // Single quotes
val b: Boolean = true
val byte: Byte = 127
val s: Short = 32767

// AnyRef types  
val str: String = "Hello"       // Double quotes
val list: List[Int] = List(1, 2, 3)
val opt: Option[Int] = Some(42)
```

---

## 🔍 Type Inference — Scala's Magic

```scala
// You don't need to declare types — Scala INFERS them!
val x = 42          // Scala infers: Int
val y = 3.14        // Scala infers: Double
val z = "Hello"     // Scala infers: String
val flag = true     // Scala infers: Boolean

// But you CAN be explicit (recommended for public APIs)
val speed: Double = 99.9
val message: String = "Go!"
```

> **Analogy:** Type inference is like a smart waiter. You say "I'll have the usual" and they remember your order. You don't have to spell it out every time!

---

## 🔄 Type Conversion (Casting)

### Safe Widening (automatic)
```scala
val i: Int = 42
val l: Long = i    // Int → Long: automatic! (no data loss)
val d: Double = i  // Int → Double: automatic!
```

### Explicit Narrowing (manual, may lose data)
```scala
val d: Double = 9.99
val i: Int = d.toInt     // 9 (truncates, no rounding!)
val s: String = d.toString  // "9.99"

// Conversion methods
42.toString      // "42"
"42".toInt       // 42
"3.14".toDouble  // 3.14
'A'.toInt        // 65 (ASCII value)
65.toChar        // 'A'
```

### Safe String to Number
```scala
import scala.util.Try

val result = Try("not a number".toInt)
// result = Failure(NumberFormatException)

val safe = Try("42".toInt).getOrElse(-1)
// safe = 42

val safe2 = Try("abc".toInt).getOrElse(-1)
// safe2 = -1
```

---

## 🌟 Option — Scala's Null-Safety Hero

```scala
// ❌ Java/other languages — NullPointerException danger!
val name: String = null  // 💣 Dangerous!

// ✅ Scala way — Option wraps potential null
val name: Option[String] = Some("Pradeep")   // Has value
val empty: Option[String] = None              // No value

// Using Option
name match {
  case Some(n) => println(s"Name is $n")
  case None    => println("No name given")
}

// getOrElse — provide a default
val n = name.getOrElse("Anonymous")  // "Pradeep"
val e = empty.getOrElse("Anonymous") // "Anonymous"
```

> **Option Analogy:** Option is like a gift box. `Some(value)` = box with a gift inside. `None` = empty box. You always CHECK the box before assuming there's a gift!

---

## 🏗️ Lazy Values

```scala
// Regular val — computed IMMEDIATELY
val expensive = computeHeavyCalculation()  // Runs right now

// lazy val — computed only when FIRST ACCESSED
lazy val lazyExpensive = computeHeavyCalculation()  // Not yet!
println(lazyExpensive)  // NOW it computes (only once)
```

> **Use case:** Reading a config file. If the config is never needed, why read it?

---

## 📦 Tuples — Multiple Values in One Variable

```scala
// Tuple — bundle different types together
val person = ("Pradeep", 21, true)  // Tuple3[String, Int, Boolean]

// Access by position (1-indexed!)
println(person._1)  // "Pradeep"
println(person._2)  // 21
println(person._3)  // true

// Named tuple unpacking
val (name, age, isStudent) = person
println(name)  // "Pradeep"

// Common 2-tuple (Pair)
val pair: (String, Int) = ("score", 100)
```

---

## 🔐 Unit — The "Nothing to Return" Type

```scala
// Unit = void in Java — means "this function returns nothing"
def printGreeting(): Unit = {
  println("Hello!")
  // implicitly returns ()
}

val result: Unit = printGreeting()
println(result)  // ()  ← Unit's only value
```

---

## 🧮 Number Literals (Readability Tricks)

```scala
val million = 1_000_000        // Underscores for readability!
val hex = 0xFF                 // Hexadecimal = 255
val binary = Integer.parseInt("1010", 2)  // Binary = 10
val scientific = 1.5e10        // Scientific notation
```

---

## 📐 Type Aliases

```scala
// Give complex types a simple name
type Name = String
type Age = Int
type PersonRecord = (Name, Age, Boolean)

val p: PersonRecord = ("Pradeep", 21, true)
```

---

## 🧪 Practice Exercises

1. Create `val` and `var` for your name, age, and GPA. Try reassigning — what happens?
2. Convert `"3.14"` to Double, then to Int. What value do you get?
3. Write a function that safely converts a String to Int using `Try`, returning -999 if invalid.

---

## 🏷️ Tags
#scala #variables #types #val #var #option #type-system #immutability #beginner

## 🔗 Related Notes
- [[01-Basic-Syntax]] — Syntax foundation
- [[04-Control-Flow]] — Using types in conditions
- [[06-Functions]] — Functions with typed parameters
- [[08-OOP-Concepts]] — Custom types with classes
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
