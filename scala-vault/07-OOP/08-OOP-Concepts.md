# 🏛️ 08 — OOP Concepts ⭐⭐⭐

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[07-Collections|← Previous: Collections]]**

## 🧠 Feynman Explanation
> **OOP is like a blueprint system for the real world.** A `class` is a blueprint (like an architect's drawing). An `object` is the actual building made from that blueprint. Scala's OOP is special — it COMBINES OOP with functional programming, giving you the best of both worlds!

---

## 🏗️ CLASS — The Blueprint

```scala
// Basic class definition
class Person(val name: String, var age: Int) {
  
  // Fields (from constructor params with val/var)
  // name = val → immutable field
  // age = var → mutable field
  
  // Additional field
  val id: Int = Person.nextId()
  
  // Method
  def greet(): String = s"Hi, I'm $name and I'm $age years old."
  
  // Method with params
  def isOlderThan(other: Person): Boolean = age > other.age
  
  // Overriding toString
  override def toString: String = s"Person($name, $age)"
}

// Creating instances
val p1 = new Person("Pradeep", 21)
val p2 = new Person("Kumar", 25)

println(p1.name)        // "Pradeep"
println(p1.age)         // 21
p1.age = 22             // ✅ var can change
// p1.name = "Other"    // ❌ val cannot change
println(p1.greet())     // "Hi, I'm Pradeep and I'm 22 years old."
println(p1.isOlderThan(p2))  // false
```

---

## 🔧 Constructor Variants

```scala
class Animal(val name: String, val sound: String) {
  
  // Auxiliary constructor — calls primary with 'this'
  def this(name: String) = this(name, "...")
  
  // Body code runs at construction time
  println(s"$name was created!")
  
  def speak(): Unit = println(s"$name says: $sound")
}

val dog = new Animal("Rex", "Woof")  // "Rex was created!"
val cat = new Animal("Whiskers")      // Uses auxiliary constructor
```

---

## ✨ CASE CLASS — Scala's Secret Weapon

> Case classes are **data containers** that auto-generate boilerplate. Use them for 90% of your data modeling!

```scala
case class Student(name: String, age: Int, gpa: Double)

// What you get FOR FREE:
val s1 = Student("Pradeep", 21, 9.1)  // No 'new' needed!
val s2 = Student("Pradeep", 21, 9.1)

// 1. equals — structural equality
s1 == s2          // ✅ true! (compared field by field)

// 2. hashCode — consistent with equals
s1.hashCode == s2.hashCode  // true

// 3. toString — readable output
println(s1)       // Student(Pradeep,21,9.1)

// 4. copy — create modified copies
val s3 = s1.copy(gpa = 9.5)
// Student(Pradeep,21,9.5)

// 5. Pattern matching! (unapply is generated)
s1 match {
  case Student("Pradeep", age, gpa) if gpa > 9.0 =>
    println(s"Top student! Age $age, GPA $gpa")
  case Student(name, _, _) =>
    println(s"Regular student: $name")
}
```

### Regular Class vs Case Class
| Feature | Class | Case Class |
|---------|-------|-----------|
| `new` keyword | Required | Optional |
| `equals` | Reference (same object) | Structural (same data) |
| `toString` | Memory address | Field values |
| `copy` | Manual | Auto-generated |
| Pattern matching | Needs `unapply` | Built-in |
| Mutability | Your choice | Immutable by default |

---

## 🎭 OBJECT — The Singleton

```scala
// object = ONE INSTANCE ONLY (Singleton pattern built-in!)
object DatabaseConfig {
  val host = "localhost"
  val port = 5432
  val db = "mydb"
  
  def connectionString: String = s"jdbc:postgresql://$host:$port/$db"
}

// Use directly — no instantiation!
println(DatabaseConfig.host)
println(DatabaseConfig.connectionString)
```

### Companion Object — The Factory Pattern
```scala
class Circle(val radius: Double) {
  def area: Double = Math.PI * radius * radius
}

// Companion: SAME NAME, same file as the class
object Circle {
  // Factory method — no 'new' needed by the caller
  def apply(radius: Double): Circle = new Circle(radius)
  
  // Another factory
  def unit: Circle = new Circle(1.0)
  
  // Constants
  val Pi = Math.PI
}

// Usage — looks like a function call!
val c = Circle(5.0)     // Calls Circle.apply(5.0)
val unit = Circle.unit
```

---

## 🧬 INHERITANCE — Extending Classes

```scala
// Base class
abstract class Shape {
  val color: String           // Abstract field — subclass must provide
  def area: Double            // Abstract method — subclass must implement
  def perimeter: Double       // Abstract method
  
  // Concrete method — shared by all shapes
  def describe(): String = s"$color ${this.getClass.getSimpleName}, area=${area:.2f}"
}

// Concrete class — extends Shape
class Rectangle(val width: Double, val height: Double, val color: String) 
    extends Shape {
  
  override def area: Double = width * height
  override def perimeter: Double = 2 * (width + height)
}

class Circle(val radius: Double, val color: String) extends Shape {
  override def area: Double = Math.PI * radius * radius
  override def perimeter: Double = 2 * Math.PI * radius
}

// Using polymorphism
val shapes: List[Shape] = List(
  new Rectangle(5, 3, "Red"),
  new Circle(4, "Blue"),
  new Rectangle(2, 2, "Green")
)

shapes.foreach(s => println(s.describe()))
val totalArea = shapes.map(_.area).sum
```

---

## 🎨 TRAIT — Like Interface on Steroids

> Traits = Java interfaces + can have code. Scala's multiple inheritance mechanism!

```scala
// Trait with abstract and concrete members
trait Flyable {
  def maxAltitude: Int      // Abstract
  def fly(): String = s"Flying up to $maxAltitude meters!"  // Concrete
}

trait Swimmable {
  def maxDepth: Int
  def swim(): String = s"Diving to $maxDepth meters!"
}

trait Runnable {
  def speed: Int
  def run(): String = s"Running at $speed km/h"
}

// Mix in multiple traits!
class Duck(val name: String) extends Flyable with Swimmable with Runnable {
  override val maxAltitude = 100
  override val maxDepth = 5
  override val speed = 10
}

val donald = new Duck("Donald")
println(donald.fly())   // "Flying up to 100 meters!"
println(donald.swim())  // "Diving to 5 meters!"
println(donald.run())   // "Running at 10 km/h"
```

### Trait vs Abstract Class
| Feature | Trait | Abstract Class |
|---------|-------|---------------|
| Multiple inheritance | ✅ Mix many | ❌ Single only |
| Constructor params | Scala 3 only | ✅ Yes |
| State | ✅ Can have fields | ✅ Can have fields |
| Java interop | ✅ | ✅ |
| **Prefer when** | Behavior mix-in | Strong "is-a" relationship |

---

## 🛡️ Access Modifiers

```scala
class BankAccount(private var balance: Double, val owner: String) {
  
  // public — default in Scala (everyone can access)
  def getOwner: String = owner
  
  // private — only THIS class
  private def secretCode: Int = 1234
  
  // protected — this class + subclasses
  protected def internalBalance: Double = balance
  
  // private[packagename] — visible within package
  private[banking] def transferInternal(amount: Double): Unit = {
    balance += amount
  }
  
  def deposit(amount: Double): Unit = {
    if (amount > 0) balance += amount
  }
  
  def withdraw(amount: Double): Boolean = {
    if (amount <= balance) {
      balance -= amount
      true
    } else false
  }
}
```

---

## 🔒 Sealed — Control Your Hierarchy

```scala
// sealed = all subclasses must be in SAME FILE
// This allows exhaustive pattern matching!
sealed trait Result
case class Success(value: Int) extends Result
case class Failure(error: String) extends Result
case object Pending extends Result

def handleResult(r: Result): String = r match {
  case Success(v) => s"Got: $v"
  case Failure(e) => s"Error: $e"
  case Pending    => "Still waiting..."
  // Compiler warns if you miss any case!
}
```

---

## 🎪 Case Object

```scala
// case object — like case class but no fields, singleton
sealed trait Direction
case object North extends Direction
case object South extends Direction
case object East extends Direction
case object West extends Direction

val dir: Direction = North
dir match {
  case North => println("Going North!")
  case South => println("Going South!")
  case East  => println("Going East!")
  case West  => println("Going West!")
}
```

---

## 🔮 Type Parameters (Generics)

```scala
// Generic class
class Box[T](val value: T) {
  def map[U](f: T => U): Box[U] = new Box(f(value))
  override def toString: String = s"Box($value)"
}

val intBox = new Box(42)
val strBox = new Box("Hello")
val doubled = intBox.map(_ * 2)  // Box(84)

// Generic method
def swap[A, B](pair: (A, B)): (B, A) = (pair._2, pair._1)
swap((1, "hello"))  // ("hello", 1)

// Upper bound — T must be a subtype of Comparable
def max[T <: Comparable[T]](a: T, b: T): T =
  if (a.compareTo(b) >= 0) a else b

// Lower bound — T must be a supertype of String
def addString[T >: String](list: List[T], s: String): List[T] = s :: list
```

---

## 🎨 Mixin Composition Pattern

```scala
trait Logger {
  def log(msg: String): Unit = println(s"[LOG] $msg")
}

trait Validator {
  def validate(data: String): Boolean = data.nonEmpty
}

trait Formatter {
  def format(data: String): String = data.trim.toLowerCase
}

// Mix them in at usage time!
class UserService extends Logger with Validator with Formatter {
  def process(input: String): Option[String] = {
    log(s"Processing: $input")
    val formatted = format(input)
    if (validate(formatted)) Some(formatted) else None
  }
}
```

---

## 🏆 OOP Design Patterns in Scala

### Singleton (built-in with object)
```scala
object AppConfig {
  val version = "1.0.0"
  lazy val settings = loadSettings()
  private def loadSettings() = Map("debug" -> "true")
}
```

### Factory Method (companion object)
```scala
class Connection private (val host: String, val port: Int)
object Connection {
  def apply(host: String, port: Int): Connection = {
    require(port > 0 && port < 65536, "Invalid port")
    new Connection(host, port)
  }
}
```

### Builder Pattern
```scala
case class Query(
  table: String,
  conditions: List[String] = Nil,
  limit: Option[Int] = None
) {
  def where(cond: String): Query = copy(conditions = conditions :+ cond)
  def limit(n: Int): Query = copy(limit = Some(n))
  def build: String = {
    val where = if (conditions.nonEmpty) s" WHERE ${conditions.mkString(" AND ")}" else ""
    val lim = limit.map(n => s" LIMIT $n").getOrElse("")
    s"SELECT * FROM $table$where$lim"
  }
}

val q = Query("users")
  .where("age > 18")
  .where("active = true")
  .limit(10)
  .build
// "SELECT * FROM users WHERE age > 18 AND active = true LIMIT 10"
```

---

## 📊 OOP Quick Reference

```
CLASS         → Blueprint with state and behavior
CASE CLASS    → Immutable data container (auto equals/copy/toString)
OBJECT        → Singleton (one instance, no 'new')
COMPANION     → Static-like methods for a class
ABSTRACT CLASS→ Blueprint with abstract methods
TRAIT         → Mix-in behavior (multiple inheritance)
SEALED TRAIT  → Controlled hierarchy (for pattern matching)
CASE OBJECT   → Singleton + pattern matching support
```

---

## 🧪 Practice Exercises

1. Model a `BankAccount` with `deposit`, `withdraw`, `balance`. Use companion object for creation.
2. Create a sealed hierarchy: `Animal` → `Cat`, `Dog`, `Bird`. Write a `makeSound` function using pattern matching.
3. Implement the Builder pattern for an `HttpRequest` (method, url, headers, body)
4. Create a generic `Stack[T]` with `push`, `pop`, `peek` using a `List[T]` internally

---

## 🏷️ Tags
#scala #oop #class #case-class #object #trait #inheritance #sealed #generics #design-patterns #advanced

## 🔗 Related Notes
- [[06-Functions]] — Methods vs functions
- [[07-Collections]] — Built on OOP
- [[09-Advanced-Scala]] — Type classes, implicits
- [[10-Data-Engineering]] — Spark uses Scala OOP
- [[OOP-Cheatsheet]] — Quick reference
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
