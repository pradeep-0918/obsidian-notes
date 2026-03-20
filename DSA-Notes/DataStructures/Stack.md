# 📚 Stack
#DataStructure #Linear #Stack

⬅️ [[Circular Linked List]] | ➡️ [[Queue]]

---

## 📌 Definition
A **stack** is a linear data structure following **LIFO** (Last In, First Out) principle. Think of a stack of plates.

---

## ⚙️ How It Works
```
PUSH →  [5]   ← top
        [3]
        [1]
        [7]   ← bottom
```
- `push(x)` — add to top
- `pop()` — remove from top
- `peek()` — view top without removing

---

## 🧠 When to Use
- Undo/redo functionality
- Function call stack (recursion)
- Balanced parentheses
- Expression evaluation
- DFS (iterative)
- Monotonic stack problems

---

## 🔥 Key Properties
- LIFO ordering
- O(1) push, pop, peek
- Can be implemented with array or linked list

---

## ⏱ Complexity

| Operation | Time |
|---|---|
| Push | O(1) |
| Pop | O(1) |
| Peek | O(1) |
| Search | O(n) |
| Space | O(n) |

---

## 🔥 Interview Patterns
1. **Parentheses matching** — push open, pop on close
2. **Monotonic stack** — next greater element
3. **Expression evaluation** — RPN, calculators
4. **Function call tracking** — recursion simulation

---

## 🧪 Problems
- [ ] [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) — Easy
- [ ] [Min Stack](https://leetcode.com/problems/min-stack/) — Medium
- [ ] [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/) — Medium
- [ ] [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/) — Medium
- [ ] [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/) — Medium
- [ ] [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) — Hard
- [ ] [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/) — Hard

---

## ❌ Common Mistakes
- Not checking for empty stack before pop/peek
- Confusing stack with queue

---

## 🔗 Related Topics
- [[Monotonic Stack]]
- [[Depth First Search]] — Uses stack internally
- [[Backtracking]]
- [[Dynamic Array]] — Common implementation

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
