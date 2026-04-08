# 📘 01 — Basic Syntax

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]**

## 🧠 Feynman Explanation
> **Imagine you're writing a recipe.** You need a title (the object name), ingredients (variables), and steps (code). Scala's syntax is that recipe format — clean, structured, and precise.

---

## 🔑 The Golden Rules of Scala Syntax

### 1. The Entry Point — `main` Method
Every Scala program starts here. Think of it as the "ON button" of your program.

```scala
object HelloWorld {
  def main(args: Array[String]): Unit = {
    println("Hello, World!")
  }
}
```

**Breaking it down:**
| Part | Meaning | Real-world analogy |
|------|---------|-------------------|
| `object` | Singleton — one instance only | A unique school building |
| `HelloWorld` | Name of the object | The school's name |
| `def main` | The starting function | The front door |
| `args: Array[String]` | Command-line arguments | Info you shout before entering |
| `: Unit` | Returns nothing (like void) | A task that just "does" something |
| `println` | Print + new line | Writing on a whiteboard |

### 2. Scala 3 — New Simpler Way (2021+)
```scala
@main def hello(): Unit =
  println("Hello, Scala 3!")
```

---

## 📐 Syntax Structure

### Statements vs Expressions
```scala
// ❌ Java style (statements - old way)
int x = 5;

// ✅ Scala style (expressions - everything has a value!)
val x = 5  // no semicolon needed!
val result = if (x > 3) "big" else "small"  // if-else is an EXPRESSION!
```

> **Key Insight 🔥:** In Scala, almost EVERYTHING is an expression that returns a value. This is a superpower from functional programming!

### Semicolons
```scala
// Scala does NOT need semicolons (unlike Java/C++)
val a = 1   // ✅ Clean
val b = 2   // ✅ Clean
val c = 3; val d = 4  // ✅ Semicolon only when TWO things on same line
```

### Blocks `{}`
```scala
val result = {
  val x = 10
  val y = 20
  x + y  // Last line = return value of the block!
}
// result is 30
```

> **6th Grade Analogy:** A block is like a lunchbox — everything inside is contained, and what you take OUT is the last thing you packed.

---

## 📁 File Structure

```
MyProject/
├── src/
│   └── main/
│       └── scala/
│           └── MyApp.scala    ← Your code goes here
├── build.sbt                  ← Build configuration
└── project/
    └── build.properties       ← SBT version
```

### build.sbt (minimal)
```sbt
name := "MyFirstScalaApp"
version := "0.1.0"
scalaVersion := "3.3.1"
```

---

## 💬 Comments
```scala
// This is a single-line comment

/* This is a 
   multi-line comment */

/** This is Scaladoc — used for documentation
  * @param name the person's name
  * @return a greeting string
  */
def greet(name: String): String = s"Hello, $name!"
```

---

## 🔄 Scala vs Java — Quick Comparison

| Feature | Java | Scala |
|---------|------|-------|
| Entry point | `public static void main` | `@main def` or `object` with `main` |
| Semicolons | Required | Optional |
| Type inference | Partial | Full (`val x = 5` knows it's `Int`) |
| Null safety | Not enforced | `Option[T]` preferred |
| Immutability | `final` keyword | `val` by default |

---

## 🧪 Practice Exercises

1. Write a Scala program that prints your name 5 times
2. Create a block that calculates `(3 + 4) * 2` and stores it in `val`
3. Write a Scaladoc comment for a function that adds two numbers

---

## 🏷️ Tags
#scala #syntax #basics #feynman #beginner #jvm

## 🔗 Related Notes
- [[02-Input-Output]] — Next Step: Printing & Reading
- [[03-Variables-Types]] — What are `val` and `var`?
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
