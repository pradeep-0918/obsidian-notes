# 11 — BufferedReader in Java (Fast Input)

#java #bufferedreader #fastinput #io #syntax #dsa
← [[10_Builtin_Functions]] | Next → [[12_StringTokenizer]]

---

## 🧠 Feynman Explain

`Scanner` reads one character at a time — slow for big input.  
`BufferedReader` **buffers** (pre-loads) a big chunk of input at once — much faster.  
In competitive programming, when input has **10⁵+ numbers**, always use BufferedReader.

---

## ⚡ Speed Comparison

| Method | Speed | Use When |
|--------|-------|----------|
| `Scanner` | Slow | Small input, learning |
| `BufferedReader` | **Fast** | Competitive programming |
| `BufferedReader` + `StreamTokenizer` | Fastest | Extreme speed needed |

---

## 📥 Basic Setup

```java
import java.io.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        // Read a full line as String
        String line = br.readLine();

        // Read a single integer on its own line
        int n = Integer.parseInt(br.readLine().trim());

        // Read a long
        long l = Long.parseLong(br.readLine().trim());
    }
}
```

> ⚠️ Always add `throws IOException` to main, or wrap in try-catch.

---

## 🔢 Reading Multiple Values on One Line

```java
// Input: "3 5 7"
String[] parts = br.readLine().split(" ");
int a = Integer.parseInt(parts[0]);
int b = Integer.parseInt(parts[1]);
int c = Integer.parseInt(parts[2]);
```

---

## 📦 Reading an Array

```java
// Input line 1: n
// Input line 2: n integers separated by spaces

int n = Integer.parseInt(br.readLine().trim());
String[] parts = br.readLine().split(" ");
int[] arr = new int[n];
for (int i = 0; i < n; i++) {
    arr[i] = Integer.parseInt(parts[i]);
}
```

---

## 📦 Reading a 2D Matrix

```java
int n = Integer.parseInt(br.readLine().trim());
int[][] matrix = new int[n][n];
for (int i = 0; i < n; i++) {
    String[] row = br.readLine().split(" ");
    for (int j = 0; j < n; j++) {
        matrix[i][j] = Integer.parseInt(row[j]);
    }
}
```

---

## 🚀 Full Competitive Programming Template

```java
import java.io.*;
import java.util.*;

public class Main {
    static BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    static PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));

    public static void main(String[] args) throws IOException {
        int t = Integer.parseInt(br.readLine().trim()); // Number of test cases
        while (t-- > 0) {
            solve();
        }
        pw.flush();
    }

    static void solve() throws IOException {
        int n = Integer.parseInt(br.readLine().trim());
        StringTokenizer st = new StringTokenizer(br.readLine());
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = Integer.parseInt(st.nextToken());

        // ... logic ...

        pw.println(result);
    }
}
```

---

## ⚡ StreamTokenizer (Even Faster than BufferedReader for numbers)

```java
import java.io.*;

StreamTokenizer in = new StreamTokenizer(new BufferedReader(new InputStreamReader(System.in)));

in.nextToken();
int n = (int) in.nval;  // in.nval always returns double

in.nextToken();
int x = (int) in.nval;
```

> Use `StreamTokenizer` when reading **millions of integers** — it's the fastest pure-Java method.

---

## 📤 Fast Output with PrintWriter

```java
PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));

pw.println(42);          // Print with newline
pw.print("hello ");      // Print without newline
pw.printf("%.2f%n", 3.14); // Formatted

pw.flush();              // ❗ MUST call flush() at the very end!
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Setup | `new BufferedReader(new InputStreamReader(System.in))` |
| Read line | `br.readLine()` |
| Read int | `Integer.parseInt(br.readLine().trim())` |
| Read array | `br.readLine().split(" ")` then parseInt each |
| Fast output | `new PrintWriter(...)` + `pw.flush()` |
| Fastest int input | `StreamTokenizer` + `in.nval` |

---

## 🔗 Related Notes
- [[01_Input_Output]] — Scanner (simpler but slower)
- [[12_StringTokenizer]] — Tokenize each line after readLine
- [[02_Arrays]] — Fill arrays from BufferedReader input
