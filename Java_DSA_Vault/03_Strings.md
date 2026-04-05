# 03 — Strings in Java

#java #string #syntax #dsa
← [[02_Arrays]] | Next → [[04_Hashing]]

---

## 🧠 Feynman Explain

A **String** in Java is **immutable** — once created, you can't change it.  
Every "change" creates a **new String** in memory.  
For repeated edits, use **StringBuilder** (mutable, fast).

---

## 📦 Declaration

```java
String s = "hello";
String s = new String("hello");         // Avoid — creates extra object
String s = new String(charArray);       // From char array
```

---

## 🔍 Core String Methods

```java
String s = "Hello World";

// Length
s.length();                    // 11

// Access
s.charAt(0);                   // 'H'
s.indexOf('o');                // 4  (first occurrence)
s.lastIndexOf('o');            // 7  (last occurrence)
s.indexOf("World");            // 6

// Substrings
s.substring(6);                // "World"
s.substring(0, 5);             // "Hello" (end exclusive)

// Comparison
s.equals("Hello World");       // true  (use this, NOT ==)
s.equalsIgnoreCase("hello world"); // true
s.compareTo("Hello");          // lexicographic compare
s.startsWith("Hello");         // true
s.endsWith("World");           // true
s.contains("lo");              // true

// Modification (returns NEW string)
s.toLowerCase();               // "hello world"
s.toUpperCase();               // "HELLO WORLD"
s.trim();                      // removes leading/trailing spaces
s.strip();                     // Unicode-aware trim (Java 11+)
s.replace('l', 'r');           // "Herro Worrd"
s.replace("World", "Java");    // "Hello Java"
s.replaceAll("\\s+", "_");     // regex replace

// Split
String[] parts = s.split(" ");   // ["Hello", "World"]
String[] parts = s.split(",", 3); // Limit to 3 parts

// Join & Concat
String joined = String.join("-", "a", "b", "c");  // "a-b-c"
String joined = String.join(", ", arr);            // Join array
s.concat(" !");                // "Hello World !"

// Check
s.isEmpty();                   // false (length == 0)
s.isBlank();                   // false (only whitespace) Java 11+

// Conversion
s.toCharArray();               // char[]
String.valueOf(42);            // "42"
String.valueOf(3.14);          // "3.14"
Integer.toString(42);          // "42"
```

---

## ⚡ StringBuilder — Mutable String

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" World");
sb.insert(5, ",");             // "Hello, World"
sb.delete(5, 6);               // Remove index 5..5
sb.deleteCharAt(0);            // Remove char at index
sb.reverse();                  // Reverse the whole builder
sb.replace(0, 5, "Hi");       // Replace range
sb.setCharAt(0, 'h');         // Modify in place
sb.toString();                 // Convert back to String
sb.length();                   // Current length
sb.charAt(i);                  // Access char
```

> 🏎️ **Rule:** Use `StringBuilder` inside loops. `String + String` in a loop is **O(n²)**.

---

## 🔄 Common Patterns

```java
// Palindrome check
String rev = new StringBuilder(s).reverse().toString();
boolean isPalin = s.equals(rev);

// Count character frequency
int[] freq = new int[26];
for (char c : s.toCharArray()) freq[c - 'a']++;

// String to int
int n = Integer.parseInt("42");
long l = Long.parseLong("1234567890");
double d = Double.parseDouble("3.14");

// int to String
String s = String.valueOf(42);
String s = Integer.toString(42);
String s = "" + 42;           // Quick but avoid in loops

// Remove non-alphanumeric
s = s.replaceAll("[^a-zA-Z0-9]", "");

// Char operations
char c = 'A';
Character.isLetter(c);        // true
Character.isDigit(c);         // false
Character.isUpperCase(c);     // true
Character.toLowerCase(c);     // 'a'
(int) c                       // ASCII = 65
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Length | `s.length()` |
| Char at i | `s.charAt(i)` |
| Substring | `s.substring(l, r)` |
| Compare | `s.equals(t)` |
| Find | `s.indexOf("x")` |
| Split | `s.split(" ")` |
| Replace | `s.replace("a","b")` |
| To array | `s.toCharArray()` |
| Fast build | `new StringBuilder()` |
| Reverse | `sb.reverse()` |

---

## 🔗 Related Notes
- [[02_Arrays]] — char[] and Arrays.sort
- [[12_StringTokenizer]] — Fast string splitting
- [[04_Hashing]] — Map char frequencies
