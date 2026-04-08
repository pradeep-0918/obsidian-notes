# ⚡ 09 — Advanced Scala

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[08-OOP-Concepts|← Previous: OOP]]**

## 🧠 Feynman Explanation
> **Advanced Scala is like learning to cook beyond recipes.** You understand WHY ingredients work together, not just HOW. Implicits, type classes, monads — these are the professional-chef techniques that make Scala code elegant and powerful.

---

## 🔮 IMPLICITS (Scala 2) / GIVEN + USING (Scala 3)

### The Problem Implicits Solve
```scala
// Without implicits: you must pass context EVERYWHERE
def processData(data: String, config: Config, logger: Logger): Result = ???

// Call site: noise everywhere
processData(myData, globalConfig, myLogger)
processData(otherData, globalConfig, myLogger)  // Repeating config, logger!
```

### Scala 2: implicit
```scala
case class Config(timeout: Int, retries: Int)

// implicit parameter — filled automatically if in scope
def fetchData(url: String)(implicit config: Config): String = {
  s"Fetching $url with timeout ${config.timeout}"
}

// Provide the implicit value
implicit val defaultConfig: Config = Config(30, 3)

// Now call WITHOUT passing config!
fetchData("https://api.example.com")   // Works!
fetchData("https://other.com")          // Same implicit config
```

### Scala 3: given / using (Cleaner!)
```scala
// 'given' replaces 'implicit val'
given defaultConfig: Config = Config(30, 3)

// 'using' replaces 'implicit' in param list
def fetchData(url: String)(using config: Config): String =
  s"Fetching $url with timeout ${config.timeout}"

fetchData("https://api.example.com")  // Uses given config
```

---

## 🎭 TYPE CLASSES — Polymorphism Without Inheritance

> **The pattern:** Define behavior for types AFTER they exist. No modification needed!

```scala
// Define the "interface" (type class)
trait Serializable[A] {
  def serialize(value: A): String
  def deserialize(s: String): A
}

// Provide instances for types
given Serializable[Int] with {
  def serialize(n: Int): String = n.toString
  def deserialize(s: String): Int = s.toInt
}

given Serializable[Boolean] with {
  def serialize(b: Boolean): String = if (b) "1" else "0"
  def deserialize(s: String): Boolean = s == "1"
}

// Generic function using the type class
def roundTrip[A](value: A)(using ser: Serializable[A]): A =
  ser.deserialize(ser.serialize(value))

roundTrip(42)     // 42
roundTrip(true)   // true
```

---

## 🌊 FUNCTORS, MONADS — The Big Concepts

### Functor — "Can be mapped over"
```scala
// Option is a Functor
Some(5).map(_ * 2)   // Some(10)
None.map(_ * 2)      // None

// List is a Functor
List(1,2,3).map(_ * 2)  // List(2, 4, 6)

// The pattern: F[A] => (A => B) => F[B]
```

### Monad — "Can be flatMapped"
```scala
// Option Monad
def divide(a: Int, b: Int): Option[Int] =
  if (b == 0) None else Some(a / b)

val result = for {
  x <- divide(10, 2)   // Some(5)
  y <- divide(x, 0)    // None!
  z <- divide(y, 3)    // Never reached
} yield z
// result = None  ← The 'None' propagated!

// List Monad
val combinations = for {
  x <- List(1, 2, 3)
  y <- List("a", "b")
} yield s"$x$y"
// List("1a", "1b", "2a", "2b", "3a", "3b")
```

> **Monad Analogy:** A monad is like a burrito. The tortilla (wrapper) keeps things organized. You can stuff more things in (flatMap), but it stays a burrito (same wrapper type). The wrapper handles the "messy details" (None propagation, empty list, etc.)!

---

## 🎯 IMPLICIT CONVERSIONS & EXTENSION METHODS

### Scala 3: Extension Methods (Clean Syntax!)
```scala
// Add methods to existing types without modifying them!
extension (s: String)
  def shout: String = s.toUpperCase + "!!!"
  def isPalindrome: Boolean = s == s.reverse
  def wordCount: Int = s.trim.split("\\s+").length

"hello".shout         // "HELLO!!!"
"racecar".isPalindrome  // true
"hello world".wordCount  // 2

// Extension on Int
extension (n: Int)
  def factorial: Long = (1 to n).map(_.toLong).product
  def isPrime: Boolean = n > 1 && (2 until n).forall(n % _ != 0)

5.factorial  // 120
7.isPrime    // true
```

---

## 🔧 FUTURE — Asynchronous Programming

```scala
import scala.concurrent.Future
import scala.concurrent.ExecutionContext.Implicits.global

// Create a Future (runs in background)
val futureResult: Future[Int] = Future {
  Thread.sleep(1000)  // Simulate slow operation
  42
}

// Transform asynchronously
val doubled = futureResult.map(_ * 2)  // Future[Int] with 84

// Combine Futures
val f1 = Future(10)
val f2 = Future(20)
val sum = for {
  x <- f1
  y <- f2
} yield x + y  // Future[Int] with 30

// Handle results
sum.onComplete {
  case scala.util.Success(v) => println(s"Result: $v")
  case scala.util.Failure(e) => println(s"Error: $e")
}

// Or recover from failure
val safe = sum.recover {
  case e: Exception => -1  // Default on failure
}
```

---

## ⚙️ LAZY EVALUATION

```scala
// lazy val — computed on first access, then cached
lazy val heavyComputation: Int = {
  println("Computing...")
  Thread.sleep(2000)
  42
}

// LazyList (infinite sequences!)
val naturals: LazyList[Int] = LazyList.from(1)
val first100 = naturals.take(100).toList  // Only computes what you need!

// Fibonacci with LazyList
val fibs: LazyList[BigInt] = {
  def loop(a: BigInt, b: BigInt): LazyList[BigInt] =
    a #:: loop(b, a + b)  // #:: is LazyList cons
  loop(0, 1)
}
fibs.take(10).toList  // List(0, 1, 1, 2, 3, 5, 8, 13, 21, 34)
```

---

## 🏷️ TYPE BOUNDS & VARIANCE

```scala
// Upper bound: T must be Animal or subtype
class Cage[T <: Animal](val animal: T)

// Lower bound: T must be Animal or supertype
def addToList[T >: Animal](list: List[T], animal: Animal): List[T] =
  animal :: list

// Covariance: if Cat <: Animal, then List[Cat] <: List[Animal]
// (read-only structures)
class ReadOnlyBox[+T](val value: T)  // + = covariant

// Contravariance: if Cat <: Animal, then Writer[Animal] <: Writer[Cat]
// (write-only structures)
trait Printer[-T] { def print(value: T): Unit }  // - = contravariant
```

---

## 🔄 PATTERN MATCHING ADVANCED

```scala
// Extractors — custom unapply
object Email {
  def unapply(s: String): Option[(String, String)] = {
    val parts = s.split("@")
    if (parts.length == 2) Some((parts(0), parts(1)))
    else None
  }
}

"user@example.com" match {
  case Email(user, domain) => println(s"User: $user, Domain: $domain")
  case _ => println("Not an email")
}

// Tuple patterns
val pair = (1, "hello", true)
pair match {
  case (n, s, true) => println(s"Number $n, String $s, and it's true!")
  case (n, _, false) => println(s"Number $n, and it's false")
}
```

---

## 📦 EITHER — Better Error Handling

```scala
// Either[Left(Error), Right(Success)]
def parseAge(s: String): Either[String, Int] =
  s.toIntOption match {
    case Some(n) if n >= 0 => Right(n)
    case Some(n)            => Left(s"Age must be positive, got $n")
    case None               => Left(s"'$s' is not a valid number")
  }

parseAge("21")   // Right(21)
parseAge("-5")   // Left("Age must be positive, got -5")
parseAge("abc")  // Left("'abc' is not a valid number")

// Chain operations
val result = for {
  age  <- parseAge("21")
  name <- Right("Pradeep"): Either[String, String]
} yield s"$name is $age years old"
// Right("Pradeep is 21 years old")
```

---

## 🔮 IMPLICIT/GIVEN Contexts (Context Bounds)

```scala
// Ordering — sort any type that has an Ordering
def sortPair[A: Ordering](a: A, b: A): (A, A) =
  if (implicitly[Ordering[A]].lteq(a, b)) (a, b) else (b, a)

sortPair(3, 1)         // (1, 3)
sortPair("z", "a")    // ("a", "z")
```

---

## 🧩 STRUCTURAL TYPING

```scala
// Duck typing — works with anything that has the right methods
def close(closeable: { def close(): Unit }): Unit = closeable.close()

// Works with File, InputStream, Connection, anything!
```

---

## 🏷️ Tags
#scala #advanced #implicits #type-classes #futures #monads #extension-methods #either #lazy #variance

## 🔗 Related Notes
- [[08-OOP-Concepts]] — Foundation for advanced OOP
- [[06-Functions]] — Functions power these patterns
- [[10-Data-Engineering]] — Futures used in Spark
- [[🗺️ SCALA MASTER MAP]] — Return to Hub
