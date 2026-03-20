# 📦 Priority Queue

#DataStructure #Linear #Queue

⬅️ [[Deque]] | ➡️ [[Monotonic Stack]]

---

## 📌 Definition

A **Priority Queue** serves elements in priority order (highest or lowest priority first), not FIFO order. Typically implemented with a [[Heap]].

---

## ⚙️ Implementation

```python
import heapq

# Min Priority Queue (default in Python)
pq = []
heapq.heappush(pq, (priority, item))
priority, item = heapq.heappop(pq)

# Max Priority Queue (negate)
heapq.heappush(pq, (-priority, item))
```

---

## 🧠 When to Use

- Top K elements
- Dijkstra's algorithm
- Huffman coding
- Task scheduling by priority

---

## 🧪 Problems

- [Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
- [Task Scheduler](https://leetcode.com/problems/task-scheduler/)
- [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

---

## 🔗 Related Topics

- [[Heap]] — underlying implementation
- [[Min Heap]] / [[Max Heap]] — variants
- [[Greedy Algorithms]] — priority-based greedy
- [[Dijkstra's Algorithm]] — uses PQ

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
