# 🔄 Recursion
#Algorithm #Paradigm

⬅️ [[Kadane's Algorithm]] | ➡️ [[Backtracking]]

---

## 📌 Definition
**Recursion** is a technique where a function calls itself to solve smaller instances of the same problem. Requires a **base case** to terminate.

---

## ⚙️ Structure
```python
def solve(problem):
    # 1. Base case
    if is_simple(problem):
        return simple_solution(problem)
    
    # 2. Recursive case
    smaller = reduce(problem)
    sub_result = solve(smaller)
    
    # 3. Combine
    return combine(sub_result, problem)
```

---

## 🔥 Classic Examples

### Fibonacci
```python
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)  # O(2^n) — memoize!
```

### Factorial
```python
def factorial(n):
    if n == 0: return 1
    return n * factorial(n-1)
```

### Tree Traversal
```python
def inorder(root):
    if not root: return
    inorder(root.left)
    visit(root)
    inorder(root.right)
```

---

## 🧠 When to Use
- Tree/graph traversal
- Divide & conquer problems
- Backtracking
- Problems with recursive substructure

---

## ⏱ Analysis
Use the **Master Theorem** for divide-and-conquer:
T(n) = aT(n/b) + f(n)

| Case | When | Result |
|---|---|---|
| 1 | f(n) = O(n^(log_b(a) - ε)) | T(n) = Θ(n^log_b(a)) |
| 2 | f(n) = Θ(n^log_b(a)) | T(n) = Θ(n^log_b(a) log n) |
| 3 | f(n) = Ω(n^(log_b(a) + ε)) | T(n) = Θ(f(n)) |

---

## ❌ Common Mistakes
- Missing base case → stack overflow
- Not reducing problem → infinite recursion
- Exponential time without memoization

---

## 🧪 Problems
- [ ] [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/) — Easy
- [ ] [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/) — Easy
- [ ] [Pow(x, n)](https://leetcode.com/problems/powx-n/) — Medium
- [ ] [Decode String](https://leetcode.com/problems/decode-string/) — Medium

---

## 🔗 Related Topics
- [[Backtracking]]
- [[Divide and Conquer]]
- [[Dynamic Programming]]
- [[Tree]] — Most tree problems use recursion

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
