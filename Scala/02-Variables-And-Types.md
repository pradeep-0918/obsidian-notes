# 02 — Variables & Types 📦
#scala #beginner #feynman

← [[01-What-Is-Scala]] | Next → [[03-Basic-Math-Operations]]

---

## 🧒 Feynman Explain

> Think of a **variable** like a labeled box 📦
> - You put something IN the box
> - You give the box a NAME
> - Later, you can use the NAME to get what's inside!

> **Type** = what KIND of thing is in the box
> (Is it a number? A word? True/False?)

---

## 📦 val vs var — The #1 Rule!

```scala
val pi = 3.14        // 🔒 LOCKED — can't change (like a sealed box)
var score = 0        // 🔓 OPEN — can change later

score = 10           // ✅ Works! var can change
pi = 3.15            // ❌ ERROR! val is sealed forever
```

### 🧒 Simple Way to Remember:
| Keyword | Think of it as | Can Change? |
|---|---|---|
| `val` | **Val**ult (bank vault 🏦) | ❌ NO |
| `var` | **Var**iable (varies = changes) | ✅ YES |

> ⚡ **TRICK:** Always use `val` first. Switch to `var` ONLY if you need to change it.
> This is the PRO way — it prevents bugs!

---

## 🔢 Data Types — The 6 You Must Know

```scala
val age: Int = 12              // Whole numbers (-2B to 2B)
val height: Double = 5.7       // Decimal numbers
val name: String = "Arjun"     // Text (always in " ")
val isStudent: Boolean = true  // Only true or false
val grade: Char = 'A'          // Single character (in ' ')
val bigNum: Long = 9999999999L // Huge whole numbers (add L)
```

### 📊 Type Size Chart

| Type | Size | Example | Use When |
|---|---|---|---|
| `Int` | 32-bit | `42` | Regular counting |
| `Long` | 64-bit | `9999999999L` | Very big numbers |
| `Double` | 64-bit | `3.14` | Decimals (precise) |
| `Float` | 32-bit | `3.14f` | Decimals (less memory) |
| `Boolean` | 1-bit | `true/false` | Yes/No conditions |
| `String` | varies | `"hello"` | Words & sentences |
| `Char` | 16-bit | `'A'` | Single letter |

---

## ⚡ TRICK — Type Inference (Let Scala Guess!)

```scala
// Instead of writing the type, let Scala figure it out!
val x = 42          // Scala KNOWS it's Int
val pi = 3.14       // Scala KNOWS it's Double
val name = "Ravi"   // Scala KNOWS it's String

// You DON'T need to write:
val x: Int = 42     // This works but is LONGER
```
> 🔥 **Time saved: 3-5 chars per variable. In 100 lines = saves 30 seconds!**

---

## ⚡ TRICK — Multiple Assignment

```scala
// Slow way:
val a = 1
val b = 2
val c = 3

// Fast way — tuple destructuring:
val (a, b, c) = (1, 2, 3)   // All in ONE line! 🔥
```

---

## ⚡ TRICK — Lazy Evaluation

```scala
lazy val heavyCalc = {
  // This runs ONLY when first used, not when declared!
  (1 to 1000000).sum
}
// heavyCalc is computed only when you actually use it
println(heavyCalc)  // Calculated HERE, not above
```

---

## 🔄 Type Conversion

```scala
// String to Int
val numStr = "42"
val num = numStr.toInt       // "42" → 42

// Int to String
val n = 100
val s = n.toString           // 100 → "100"

// Int to Double
val i = 5
val d = i.toDouble           // 5 → 5.0

// Double to Int (drops decimal!)
val pi = 3.99
val whole = pi.toInt         // 3.99 → 3  ⚠️ Truncates!
```

> ⚡ **TRICK:** `.toInt`, `.toDouble`, `.toString` — memorize these 3!

---

## 🧒 Feynman Summary

> Variable = Named box 📦
> `val` = Sealed box (never changes)
> `var` = Open box (can change)
> Type = What kind of thing is in the box

---

## 🔗 See Also
- Previous: [[01-What-Is-Scala]]
- Next: [[03-Basic-Math-Operations]]
- Cheatsheet: [[09-Speed-Cheatsheet]]
