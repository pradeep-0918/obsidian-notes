# 12 — StringTokenizer in Java

#java #stringtokenizer #fastinput #syntax #dsa
← [[11_BufferedReader]] | Back → [[Notes/00_INDEX]]

---

## 🧠 Feynman Explain

`StringTokenizer` is like scissors that cut a sentence into words.  
Much **faster** than `String.split()` because it doesn't use **regex** internally.  
In competitive programming, pair it with [[11_BufferedReader]] for maximum speed.

---

## ⚡ Speed Comparison

| Method | Speed | Notes |
|--------|-------|-------|
| `String.split(" ")` | Moderate | Uses regex — extra overhead |
| `StringTokenizer` | **Fast** | No regex, pure char scanning |
| `StreamTokenizer` | Fastest | For raw numbers only |

---

## 📦 Basic Usage

```java
import java.util.StringTokenizer;

String line = "10 20 30 40 50";
StringTokenizer st = new StringTokenizer(line);

while (st.hasMoreTokens()) {
    System.out.println(st.nextToken());  // "10", "20", "30"...
}
```

---

## 🔢 Reading Numbers from One Line

```java
StringTokenizer st = new StringTokenizer(br.readLine());

int a = Integer.parseInt(st.nextToken());
int b = Integer.parseInt(st.nextToken());
int c = Integer.parseInt(st.nextToken());
```

---

## 📦 Reading Array using StringTokenizer

```java
int n = Integer.parseInt(br.readLine().trim());
int[] arr = new int[n];
StringTokenizer st = new StringTokenizer(br.readLine());
for (int i = 0; i < n; i++) {
    arr[i] = Integer.parseInt(st.nextToken());
}
```

---

## 🔀 Custom Delimiters

```java
// Default delimiter: space, tab, newline, carriage return
StringTokenizer st1 = new StringTokenizer("a b c");

// Custom delimiter: comma
StringTokenizer st2 = new StringTokenizer("a,b,c", ",");

// Multiple delimiters: comma or semicolon
StringTokenizer st3 = new StringTokenizer("a,b;c", ",;");

// Delimiter: space + colon
StringTokenizer st4 = new StringTokenizer("a: b: c", ": ");
```

---

## 🔍 Key Methods

```java
StringTokenizer st = new StringTokenizer("hello world java");

st.hasMoreTokens();      // true — are there more tokens?
st.nextToken();          // "hello" — returns next token
st.countTokens();        // 3 — remaining tokens count (before any nextToken())
st.hasMoreElements();    // Same as hasMoreTokens()
st.nextElement();        // Same as nextToken() but returns Object
```

---

## 🚀 Full Fast Input Template

```java
import java.io.*;
import java.util.*;

public class Main {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));

        int t = Integer.parseInt(br.readLine().trim());
        while (t-- > 0) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int n = Integer.parseInt(st.nextToken());
            int k = Integer.parseInt(st.nextToken());

            st = new StringTokenizer(br.readLine());
            int[] arr = new int[n];
            for (int i = 0; i < n; i++) arr[i] = Integer.parseInt(st.nextToken());

            // logic...
            pw.println(answer);
        }
        pw.flush();
    }
}
```

---

## ⚠️ StringTokenizer vs split()

```java
// split() — uses regex, slower, returns array
String[] parts = "a b c".split(" ");

// StringTokenizer — no regex, faster, lazy evaluation
StringTokenizer st = new StringTokenizer("a b c");

// When to use split():
// - Need array index access
// - Need regex patterns
// - Input is small

// When to use StringTokenizer:
// - Competitive programming
// - Reading large input fast
// - Simple whitespace/delimiter splitting
```

---

## 🔢 Reading Long values

```java
StringTokenizer st = new StringTokenizer(br.readLine());
long a = Long.parseLong(st.nextToken());
long b = Long.parseLong(st.nextToken());
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Create | `new StringTokenizer(line)` |
| Custom delim | `new StringTokenizer(line, ",")` |
| Has more | `st.hasMoreTokens()` |
| Get token | `st.nextToken()` |
| Parse int | `Integer.parseInt(st.nextToken())` |
| Parse long | `Long.parseLong(st.nextToken())` |
| Token count | `st.countTokens()` |

---

## 🔗 Related Notes
- [[11_BufferedReader]] — Must use with BufferedReader for full benefit
- [[01_Input_Output]] — Scanner alternative
- [[03_Strings]] — String methods like split()
