# 06 — Stack in Java

#java #stack #lifo #syntax #dsa
← [[05_Queue]] | Next → [[07_LinkedList]]

---

## 🧠 Feynman Explain

A **Stack** is a pile of plates — you can only add or remove from the **top**.  
**LIFO = Last In, First Out.**  
Use it for: undo operations, bracket matching, DFS, monotonic patterns.

---

## 📦 Stack — Two Ways

### ✅ Preferred: ArrayDeque (Faster)

```java
import java.util.Deque;
import java.util.ArrayDeque;

Deque<Integer> stack = new ArrayDeque<>();

stack.push(10);         // Push to top
stack.push(20);
stack.push(30);

stack.peek();           // View top = 30 (no remove)
stack.pop();            // Remove & return top = 30
stack.isEmpty();        // false
stack.size();           // 2
```

### ⚠️ Legacy: Stack class (Thread-safe but slower)

```java
import java.util.Stack;

Stack<Integer> stack = new Stack<>();
stack.push(10);
stack.peek();           // View top
stack.pop();            // Remove top
stack.isEmpty();
stack.search(10);       // Position from top (1-indexed), -1 if absent
```

> 🏎️ **Always prefer `ArrayDeque` over `Stack`** in interviews unless thread-safety is explicitly needed.

---

## 🔄 Classic Stack Patterns

### 1. Valid Parentheses

```java
Deque<Character> stack = new ArrayDeque<>();
for (char c : s.toCharArray()) {
    if (c == '(' || c == '{' || c == '[') {
        stack.push(c);
    } else {
        if (stack.isEmpty()) return false;
        char top = stack.pop();
        if (c == ')' && top != '(') return false;
        if (c == '}' && top != '{') return false;
        if (c == ']' && top != '[') return false;
    }
}
return stack.isEmpty();
```

---

### 2. Monotonic Stack — Next Greater Element

```java
int[] result = new int[n];
Arrays.fill(result, -1);
Deque<Integer> stack = new ArrayDeque<>();  // Stores indices

for (int i = 0; i < n; i++) {
    while (!stack.isEmpty() && nums[stack.peek()] < nums[i]) {
        result[stack.pop()] = nums[i];
    }
    stack.push(i);
}
```

---

### 3. DFS using Stack (Iterative)

```java
Deque<Integer> stack = new ArrayDeque<>();
boolean[] visited = new boolean[n];

stack.push(start);
while (!stack.isEmpty()) {
    int node = stack.pop();
    if (visited[node]) continue;
    visited[node] = true;
    for (int neighbor : adj.get(node)) {
        if (!visited[neighbor]) stack.push(neighbor);
    }
}
```

---

### 4. Evaluate Reverse Polish Notation

```java
Deque<Integer> stack = new ArrayDeque<>();
for (String token : tokens) {
    if ("+-*/".contains(token)) {
        int b = stack.pop(), a = stack.pop();
        switch (token) {
            case "+": stack.push(a + b); break;
            case "-": stack.push(a - b); break;
            case "*": stack.push(a * b); break;
            case "/": stack.push(a / b); break;
        }
    } else {
        stack.push(Integer.parseInt(token));
    }
}
return stack.pop();
```

---

### 5. Min Stack

```java
Deque<Integer> stack = new ArrayDeque<>();
Deque<Integer> minStack = new ArrayDeque<>();

void push(int val) {
    stack.push(val);
    int curMin = minStack.isEmpty() ? val : Math.min(val, minStack.peek());
    minStack.push(curMin);
}
int getMin() { return minStack.peek(); }
int pop()    { minStack.pop(); return stack.pop(); }
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Create stack | `new ArrayDeque<>()` |
| Push | `stack.push(x)` |
| Pop | `stack.pop()` |
| Peek top | `stack.peek()` |
| Is empty | `stack.isEmpty()` |
| Size | `stack.size()` |

---

## 🔗 Related Notes
- [[05_Queue]] — FIFO counterpart; Deque covers both
- [[08_Graph]] — DFS uses Stack
- [[09_Tree]] — Iterative DFS uses Stack
