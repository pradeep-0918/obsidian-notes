# 📋 OOP Cheatsheet

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[08-OOP-Concepts|← Full OOP Notes]]**

## 🏷️ Tags
#scala #oop #cheatsheet #class #trait #pattern-matching

---

## 🔑 Class vs Case Class vs Object

| | `class` | `case class` | `object` |
|-|---------|-------------|--------|
| Instances | Many (with `new`) | Many (no `new`) | ONE forever |
| `equals` | Reference | Structural | N/A |
| `copy` | ❌ | ✅ | N/A |
| Pattern match | Manual | ✅ Auto | ✅ |
| Use for | Behavior + state | Data containers | Singleton/companion |

---

## 🎭 Trait Rules
```
- Like interface but CAN have code
- Mix multiple traits (class A extends B with C with D)
- No constructor params (Scala 2)
- sealed trait = all subclasses in same file
```

## 🏗️ Sealed Hierarchy Pattern
```scala
sealed trait Shape
case class Circle(r: Double) extends Shape
case class Rect(w: Double, h: Double) extends Shape
case object Point extends Shape

// Exhaustive pattern match
def area(s: Shape): Double = s match {
  case Circle(r) => Math.PI * r * r
  case Rect(w, h) => w * h
  case Point => 0.0
}
```

## 🔒 Access Levels
```
public    → default (no keyword)
protected → this class + subclasses
private   → this class only
private[pkg] → within package
```

## 🔮 Generics Quick
```scala
class Box[T](val value: T)           // Any type
class Box[T <: Animal](val v: T)     // T = Animal or subtype
class Box[T >: Cat](val v: T)        // T = Cat or supertype
class List[+T]                        // Covariant
trait Printer[-T]                     // Contravariant
```

---

## 🔗 Related Notes
[[08-OOP-Concepts]] | [[09-Advanced-Scala]] | [[🗺️ SCALA MASTER MAP]]
