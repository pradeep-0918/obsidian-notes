# 📋 Collections Cheatsheet

> **[[🗺️ SCALA MASTER MAP|← Back to Master Map]]** | **[[07-Collections|← Full Collections Notes]]**

## 🏷️ Tags
#scala #collections #cheatsheet #list #map #set

---

## 🗂️ Choose Your Collection

```
Need order + duplicates?   → List / Vector
Need unique elements?      → Set
Need key-value lookup?     → Map
Need fixed size + Java?    → Array
Need to grow/shrink fast?  → ArrayBuffer (mutable)
Need infinite sequence?    → LazyList
```

## 📋 List Operations Matrix

| Operation | Code | Result |
|-----------|------|--------|
| Prepend | `0 :: List(1,2,3)` | `List(0,1,2,3)` |
| Append | `List(1,2,3) :+ 4` | `List(1,2,3,4)` |
| Concat | `List(1,2) ++ List(3,4)` | `List(1,2,3,4)` |
| Map | `.map(_ * 2)` | transformed |
| Filter | `.filter(_ > 2)` | filtered |
| Reduce | `.reduce(_ + _)` | single value |
| Fold | `.foldLeft(0)(_ + _)` | with start |
| flatMap | `.flatMap(x => List(x,x))` | flattened |
| Group | `.groupBy(_ % 2)` | Map |
| Sort | `.sortBy(_.length)` | sorted |
| Zip | `.zip(otherList)` | List[(A,B)] |

## 🔥 The Big 5 HOF

```scala
val nums = List(1,2,3,4,5,6,7,8,9,10)

nums.map(_ * 2)              // All × 2
nums.filter(_ % 2 == 0)     // Only evens: 2,4,6,8,10
nums.reduce(_ + _)           // Sum: 55
nums.foldLeft(0)(_ + _)      // Sum with start: 55
nums.flatMap(x=>List(x,-x)) // Flatten: 1,-1,2,-2,...
```

## 🗺️ Map Quick Ops

```scala
val m = Map("a"->1, "b"->2)
m("a")              // 1 (unsafe!)
m.get("a")          // Some(1)
m.getOrElse("z", 0) // 0
m + ("c" -> 3)      // Add
m - "a"             // Remove
m.keys / m.values   // Keys/values
m.map{case(k,v)=>k->v*2} // Transform
```

---

## 🔗 Related Notes
[[07-Collections]] | [[06-Functions]] | [[🗺️ SCALA MASTER MAP]]
