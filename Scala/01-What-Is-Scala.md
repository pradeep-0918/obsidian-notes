# 01 — What Is Scala? 🦁
#scala #beginner #feynman

← [[00-MOC-Scala-Master]] | Next → [[02-Variables-And-Types]]

---

## 🧒 Feynman Explain (Like You're 6th Grade)

> Imagine you have a **super-calculator** that can also **think**.
> Normal calculators just add/subtract.
> Scala is like a calculator that can **remember**, **decide**, and **repeat** — all by itself!

Scala = **S**calable **La**nguage → it grows with you, from simple to super complex!

---

## 🌍 Why Learn Scala?

| Reason | Simple Meaning |
|---|---|
| ⚡ Fast | Runs on JVM — like Java but cleaner |
| 🧮 Math-friendly | Perfect for number crunching |
| 🤖 Used by Twitter, Netflix, Spark | Real companies use it! |
| 🧠 Teaches you to think like a pro | Functional + OOP combined |

---

## 🛠️ Setup (Install Scala in 2 Steps)

### Option A — Online (Zero Install!) ✅ Easiest
Go to 👉 **[scastie.scala-lang.org](https://scastie.scala-lang.org)**
Type code → Click Run → Done!

### Option B — Local Install
```bash
# Step 1: Install coursier
curl -fL https://github.com/coursier/launchers/raw/master/cs-x86_64-pc-linux.gz | gzip -d > cs

# Step 2: Install Scala
./cs install scala scalac
```

---

## 💻 Your First Scala Program

```scala
// Hello World — The Classic Start
object Hello extends App {
  println("Hello, Scala World! 🦁")
}
```

### What each word means:
| Word | Meaning (Simple) |
|---|---|
| `object` | A single thing (like a box) |
| `Hello` | Name of our box |
| `extends App` | "This box can RUN!" |
| `println(...)` | Print something on screen |
| `//` | Comment — Scala ignores this |

---

## ⚡ TRICK — Scala Worksheet (No main needed!)

```scala
// In a .worksheet.sc file or Scastie:
val x = 10
val y = 20
println(x + y)  // Output: 30
// No object, no App needed! Just write and run!
```
> 🔥 **Use worksheets for quick calculations — saves 5 seconds every time!**

---

## 🧒 Feynman Summary

> Scala = A smart calculator language.
> You tell it what to do → It does it FAST.
> We'll use it to solve math problems like a wizard! 🧙

---

## 🔗 See Also
- Next step: [[02-Variables-And-Types]]
- Jump to math tricks: [[03-Basic-Math-Operations]]
- Full cheatsheet: [[09-Speed-Cheatsheet]]
