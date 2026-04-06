# 01 — Input & Output in Java

#java #input #output #syntax
← [[Notes/00_INDEX]] | Next → [[02_Arrays]]

---

## 🧠 Feynman Explain

Think of `Scanner` as your **ear** — it listens for what you type.  
Think of `System.out` as your **mouth** — it prints things out.  
For **speed**, ditch Scanner. Use [[11_BufferedReader]].

---

## 📥 Standard Input — Scanner

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);

int n       = sc.nextInt();        // Read integer
long l      = sc.nextLong();       // Read long
double d    = sc.nextDouble();     // Read double
String s    = sc.next();           // Read one word (no spaces)
String line = sc.nextLine();       // Read full line

sc.close();
```

> ⚠️ **Gotcha:** After `nextInt()`, always call `sc.nextLine()` once to consume the leftover `\n` before reading a line.

---

## 📤 Standard Output — System.out

```java
System.out.print("Hello");          // No newline
System.out.println("Hello");        // With newline
System.out.printf("%.2f%n", 3.14); // Formatted output
System.out.printf("%d %s%n", n, s); // Integer + String

// Fast bulk output — wrap in StringBuilder
StringBuilder sb = new StringBuilder();
sb.append(ans).append("\n");
System.out.print(sb);
```

---

## 🔢 Reading Multiple Values on One Line

```java
// Input: "3 5 7"
int a = sc.nextInt();
int b = sc.nextInt();
int c = sc.nextInt();

// OR using split
String[] parts = sc.nextLine().split(" ");
int x = Integer.parseInt(parts[0]);
int y = Integer.parseInt(parts[1]);
```

---

## 🏁 Reading N Numbers into Array

```java
int n = sc.nextInt();
int[] arr = new int[n];
for (int i = 0; i < n; i++) {
    arr[i] = sc.nextInt();
}
```

---

## ⚡ Speed Tip — PrintWriter (Fast Output)

```java
import java.io.*;
PrintWriter pw = new PrintWriter(new BufferedWriter(new OutputStreamWriter(System.out)));
pw.println(answer);
pw.flush();  // MUST flush at the end!
```

---

## 🔗 Related Notes
- [[11_BufferedReader]] — Fastest input method
- [[12_StringTokenizer]] — Tokenize split input fast
- [[02_Arrays]] — Store the input

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Read int | `sc.nextInt()` |
| Read String word | `sc.next()` |
| Read full line | `sc.nextLine()` |
| Print with newline | `System.out.println()` |
| Formatted print | `System.out.printf()` |
| Fast output | `PrintWriter` + `flush()` |
