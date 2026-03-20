# ⛏️ Heap
#DataStructure #NonLinear #Heap

⬅️ [[Trie]] | ➡️ [[Min Heap]]

---

## 📌 Definition
A **heap** is a complete binary tree satisfying the **heap property**:
- **Max Heap**: parent ≥ all children
- **Min Heap**: parent ≤ all children

---

## ⚙️ How It Works
Stored as an array (implicit binary tree):
- Root = index 1 (or 0)
- Left child of i = 2i (or 2i+1)
- Right child of i = 2i+1 (or 2i+2)
- Parent of i = i//2

```
Max Heap:
         100
        /   \
       19    36
      / \   / \
     17  3 25  1

Array: [100, 19, 36, 17, 3, 25, 1]
```

---

## 🧠 When to Use
- Priority queues
- Scheduling (always process highest/lowest priority)
- Top K problems
- Median finding (two heaps)
- Dijkstra's algorithm

---

## ⏱ Complexity

| Operation | Time |
|---|---|
| Insert | O(log n) |
| Extract max/min | O(log n) |
| Peek max/min | O(1) |
| Build from array | O(n) |
| Search | O(n) |
| Space | O(n) |

---

## 🔥 Heapify
- **Sift up** (used in insert): bubble new element up
- **Sift down** (used in extract): bubble root down

**Build heap in O(n)**: Apply sift-down from n//2 to 1

---

## 🔗 Related Topics
- [[Min Heap]]
- [[Max Heap]]
- [[Priority Queue]]
- [[Dijkstra's Algorithm]]
- [[Top K Elements]]
- [[Two Heaps Pattern]]

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
