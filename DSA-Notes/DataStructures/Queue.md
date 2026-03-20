# 🚶 Queue
#DataStructure #Linear #Queue

⬅️ [[Stack]] | ➡️ [[Deque]]

---

## 📌 Definition
A **queue** is a linear data structure following **FIFO** (First In, First Out) principle. Think of a line at a store.

---

## ⚙️ How It Works
```
ENQUEUE → [1][2][3][4] → DEQUEUE
           back      front
```
- `enqueue(x)` — add to back
- `dequeue()` — remove from front
- `peek()` — view front element

---

## 🧠 When to Use
- BFS traversal
- Level order tree traversal
- Task scheduling / job queues
- Producer-consumer problems

---

## 🔥 Key Properties
- FIFO ordering
- O(1) enqueue and dequeue

---

## ⏱ Complexity

| Operation | Time |
|---|---|
| Enqueue | O(1) |
| Dequeue | O(1) |
| Peek | O(1) |
| Search | O(n) |
| Space | O(n) |

---

## 🆚 Types
- [[Simple Queue]] — Basic FIFO
- [[Circular Queue]] — Fixed-size ring buffer
- [[Deque]] — Double-ended
- [[Priority Queue]] — Ordered by priority

---

## 🧪 Problems
- [ ] [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/) — Easy
- [ ] [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/) — Medium
- [ ] [Open the Lock](https://leetcode.com/problems/open-the-lock/) — Medium

---

## 🔗 Related Topics
- [[Breadth First Search]] — Uses queue
- [[Monotonic Queue]]
- [[Priority Queue]]
- [[Deque]]

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
