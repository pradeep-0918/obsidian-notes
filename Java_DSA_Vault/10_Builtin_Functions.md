# 10 — Built-in Utility Functions in Java

#java #builtin #math #collections #syntax #dsa
← [[09_Tree]] | Next → [[11_BufferedReader]]

---

## 🧠 Feynman Explain

Java ships with a huge toolbox.  
**Math class** = calculator.  
**Collections class** = toolbox for Lists, sorting, shuffling.  
**Arrays class** = toolbox for arrays.  
**Integer/Long/Character** = wrappers that add extra powers to primitives.

---

## ➕ Math Class

```java
Math.abs(-5);              // 5         — absolute value
Math.max(3, 7);            // 7
Math.min(3, 7);            // 3
Math.pow(2, 10);           // 1024.0    — power
Math.sqrt(16);             // 4.0       — square root
Math.cbrt(27);             // 3.0       — cube root
Math.ceil(3.2);            // 4.0       — round up
Math.floor(3.9);           // 3.0       — round down
Math.round(3.5);           // 4         — nearest int
Math.log(Math.E);          // 1.0       — natural log
Math.log10(1000);          // 3.0
Math.PI;                   // 3.14159...
Math.E;                    // 2.71828...
Math.random();             // Random double [0.0, 1.0)
(int)(Math.random() * n);  // Random int [0, n)
```

---

## 📋 Collections Class (for Lists)

```java
import java.util.Collections;

List<Integer> list = new ArrayList<>(Arrays.asList(3, 1, 4, 1, 5));

Collections.sort(list);                        // Ascending
Collections.sort(list, Collections.reverseOrder()); // Descending
Collections.sort(list, (a, b) -> b - a);       // Custom lambda

Collections.reverse(list);                     // Reverse in-place
Collections.shuffle(list);                     // Randomize

Collections.max(list);                         // Max element
Collections.min(list);                         // Min element
Collections.frequency(list, 1);               // Count of 1 = 2
Collections.swap(list, 0, 1);                 // Swap indices
Collections.fill(list, 0);                    // Fill all with 0
Collections.nCopies(3, "x");                  // ["x","x","x"]

// Binary search (list must be sorted!)
Collections.binarySearch(list, 3);            // Index or negative

// Sum using streams
int sum = list.stream().mapToInt(Integer::intValue).sum();
```

---

## 📦 Arrays Class (for arrays)

```java
import java.util.Arrays;

int[] arr = {5, 3, 1, 4, 2};

Arrays.sort(arr);                           // Ascending
Arrays.sort(arr, 1, 4);                    // Sort subarray [1..3]

Arrays.fill(arr, 0);                       // Fill with 0
Arrays.fill(arr, 1, 3, -1);              // Fill index 1..2 with -1

Arrays.copyOf(arr, 5);                     // Copy (pad or truncate to length 5)
Arrays.copyOfRange(arr, 1, 4);            // Copy index 1..3

Arrays.equals(arr1, arr2);                 // Compare content
Arrays.deepEquals(matrix1, matrix2);      // 2D compare

Arrays.toString(arr);                      // "[1, 2, 3]"
Arrays.deepToString(matrix);              // "[[1, 2], [3, 4]]"

int idx = Arrays.binarySearch(arr, 3);   // Must be sorted first
```

---

## 🔢 Integer & Long Methods

```java
Integer.MAX_VALUE;           // 2147483647
Integer.MIN_VALUE;           // -2147483648
Long.MAX_VALUE;              // 9223372036854775807
Long.MIN_VALUE;

Integer.parseInt("42");      // String → int
Long.parseLong("9999999");   // String → long
Integer.toBinaryString(10);  // "1010"
Integer.toHexString(255);    // "ff"
Integer.toOctalString(8);    // "10"
Integer.bitCount(7);         // 3 (number of 1-bits in 7 = 111)
Integer.highestOneBit(12);   // 8 (= 1000 in binary)
Integer.numberOfLeadingZeros(1);  // 31
Integer.compare(a, b);       // neg/0/pos like compareTo
Integer.sum(a, b);           // a + b (useful in streams)
Integer.max(a, b);           // Math.max equivalent for lambdas
```

---

## 🔤 Character Methods

```java
char c = 'A';
Character.isLetter(c);       // true
Character.isDigit(c);        // false
Character.isLetterOrDigit(c);
Character.isWhitespace(c);   // false
Character.isUpperCase(c);    // true
Character.isLowerCase(c);    // false
Character.toUpperCase(c);    // 'A'
Character.toLowerCase(c);    // 'a'
(int) c;                     // ASCII: 65

// ASCII Quick Reference
// 'A'-'Z' = 65-90
// 'a'-'z' = 97-122
// '0'-'9' = 48-57
```

---

## 📝 String Utility

```java
String.valueOf(42);          // "42"
String.format("%.2f", 3.14); // "3.14"
String.format("%05d", 42);   // "00042"
String.join("-", "a","b");   // "a-b"
String.join(", ", list);     // "a, b, c" from list
```

---

## 🔂 Streams (Java 8+)

```java
import java.util.stream.*;

List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);

// Filter + Map + Collect
List<Integer> evens = list.stream()
    .filter(x -> x % 2 == 0)
    .collect(Collectors.toList());  // [2, 4]

// Sum / Average / Max / Min
int sum = list.stream().mapToInt(Integer::intValue).sum();
double avg = list.stream().mapToInt(Integer::intValue).average().orElse(0);
int max = list.stream().mapToInt(x -> x).max().getAsInt();

// Count / Any / All / None
long count = list.stream().filter(x -> x > 2).count();
boolean anyBig = list.stream().anyMatch(x -> x > 4);
boolean allPos = list.stream().allMatch(x -> x > 0);

// Sort + Distinct + Limit
list.stream().sorted().distinct().limit(3).collect(Collectors.toList());

// Collect to Map
Map<Integer, Integer> squareMap = list.stream()
    .collect(Collectors.toMap(x -> x, x -> x * x));
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Max of two | `Math.max(a, b)` |
| Power | `Math.pow(base, exp)` |
| Sort list | `Collections.sort(list)` |
| Reverse list | `Collections.reverse(list)` |
| Max in list | `Collections.max(list)` |
| Sort array | `Arrays.sort(arr)` |
| Parse int | `Integer.parseInt(s)` |
| Is digit | `Character.isDigit(c)` |
| Stream sum | `.stream().mapToInt(x->x).sum()` |

---

## 🔗 Related Notes
- [[02_Arrays]] — Arrays.sort, Arrays.fill
- [[03_Strings]] — String.format, String.join
- [[04_Hashing]] — Collections on Map/Set
