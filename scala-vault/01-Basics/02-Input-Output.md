# 📥 02 — Input & Output

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[01-Basic-Syntax|← Previous: Basic Syntax]]**

## 🧠 Feynman Explanation
> **Think of I/O like a conversation.** Output = you talking (printing to screen). Input = someone replying (user typing). Scala gives you clean, powerful ways to do both.

---

## 📤 OUTPUT — Printing to Screen

### The 3 Print Methods
```scala
println("Hello!")        // Prints + adds new line  ← Use this 90% of the time
print("Hello")           // Prints WITHOUT new line
printf("Age: %d\n", 25) // C-style formatting (rare in Scala)
```

**Visual Difference:**
```
println("A")  →  A
println("B")  →  B

print("A")    →  AB   (same line!)
print("B")
```

### String Interpolation (The Scala Superpower!)
```scala
val name = "Pradeep"
val age = 21

// s-interpolation (most common)
println(s"Hello, $name! You are $age years old.")

// f-interpolation (with formatting)
val pi = 3.14159
println(f"Pi is approximately $pi%.2f")  // Pi is approximately 3.14

// raw-interpolation (no escape sequences)
println(raw"Line1\nLine2")  // Prints literally: Line1\nLine2

// Expressions inside interpolation
println(s"Next year you'll be ${age + 1}")
println(s"Name length: ${name.length}")
```

> **6th Grade Analogy:** String interpolation is like Mad Libs — you have a sentence with blanks, and Scala fills in the blanks with your variable values!

---

## 📐 Formatting Output

### Using `format`
```scala
val price = 1234.567
println("Price: %.2f".format(price))    // Price: 1234.57
println("%-10s|%5d".format("Item", 42)) // Item      |   42
```

### Format Specifiers
| Specifier | Type | Example |
|-----------|------|---------|
| `%d` | Integer | `42` |
| `%f` | Float/Double | `3.14` |
| `%.2f` | Float 2 decimal | `3.14` |
| `%s` | String | `"hello"` |
| `%b` | Boolean | `true` |
| `%10s` | Right-padded | `"    hello"` |
| `%-10s` | Left-padded | `"hello    "` |

---

## 📥 INPUT — Reading from User

### Reading Console Input
```scala
import scala.io.StdIn

val name = StdIn.readLine("Enter your name: ")
println(s"Hello, $name!")

val age = StdIn.readInt()          // Reads integer
val price = StdIn.readDouble()     // Reads double
val flag = StdIn.readBoolean()     // Reads true/false
```

### Safe Input with Try/Catch
```scala
import scala.io.StdIn
import scala.util.Try

val input = StdIn.readLine("Enter a number: ")
val number = Try(input.toInt).getOrElse(0)  // 0 if invalid input
println(s"You entered: $number")
```

> **Why Try?** Users might type "abc" when you expect a number. `Try` catches the crash gracefully. Think of it as a safety net under a trapeze artist!

---

## 📄 File I/O

### Reading a File
```scala
import scala.io.Source

// Read all lines
val lines = Source.fromFile("data.txt").getLines().toList
lines.foreach(println)

// Read entire file as string
val content = Source.fromFile("data.txt").mkString
println(content)

// With auto-close (best practice)
val source = Source.fromFile("data.txt")
try {
  source.getLines().foreach(println)
} finally {
  source.close()
}
```

### Writing a File
```scala
import java.io.PrintWriter

val writer = new PrintWriter("output.txt")
writer.println("Line 1")
writer.println("Line 2")
writer.close()
```

### Using `java.nio` (Modern Way)
```scala
import java.nio.file.{Files, Paths}
import java.nio.charset.StandardCharsets

// Write
Files.write(Paths.get("output.txt"), 
  "Hello File!".getBytes(StandardCharsets.UTF_8))

// Read
val content = new String(Files.readAllBytes(Paths.get("data.txt")))
```

---

## 🌐 Reading from URL/Web
```scala
import scala.io.Source

val url = "https://api.example.com/data"
val content = Source.fromURL(url).mkString
println(content)
```

---

## 📊 Real-World Pattern: CSV Reading
```scala
import scala.io.Source

val csvFile = Source.fromFile("students.csv")
val data = csvFile.getLines()
  .drop(1)  // Skip header
  .map(line => line.split(","))
  .map(cols => (cols(0), cols(1).toInt))  // (name, age)
  .toList

data.foreach { case (name, age) =>
  println(s"$name is $age years old")
}
```

---

## 🧪 Practice Exercises

1. Write a program that asks for user's name and age, then prints: "Hi [name], in 10 years you'll be [age+10]!"
2. Read a text file and count the number of lines
3. Create a program that reads numbers until user types "stop", then prints the sum

---

## 🏷️ Tags
#scala #io #input #output #console #file-io #string-interpolation #beginner

## 🔗 Related Notes
- [[01-Basic-Syntax]] — The foundation
- [[03-Variables-Types]] — What types can you read/write?
- [[05-Strings-Math]] — String manipulation
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
