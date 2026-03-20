# 📦 Min Heap

#DataStructure #Tree #Heap

⬅️ [[Heap]] | ➡️ [[Max Heap]]

---

## 📌 Definition

A **Min Heap** is a [[Heap]] where the root is the **minimum** element. Every parent is ≤ its children.

---

## ⚙️ Python Usage

```python
import heapq

heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 1)
heapq.heappush(heap, 3)
min_val = heapq.heappop(heap)  # returns 1
```

---

## 🧠 When to Use

- Find K smallest elements
- Dijkstra's algorithm
- Merge K sorted lists

---

## 🔗 Related Topics

- [[Heap]] — parent concept
- [[Max Heap]] — root is maximum
- [[Priority Queue]] — min heap based

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
