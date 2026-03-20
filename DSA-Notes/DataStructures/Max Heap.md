# 📦 Max Heap

#DataStructure #Tree #Heap

⬅️ [[Min Heap]] | ➡️ [[Graph]]

---

## 📌 Definition

A **Max Heap** is a [[Heap]] where the root is the **maximum** element. Every parent is ≥ its children.

---

## ⚙️ Python Usage

```python
import heapq

heap = []
# Python only has min-heap, negate for max
heapq.heappush(heap, -5)
heapq.heappush(heap, -1)
max_val = -heapq.heappop(heap)  # returns 5
```

---

## 🧠 When to Use

- Find K largest elements
- Sliding Window Median (combined with min heap)
- Two Heap pattern

---

## 🔗 Related Topics

- [[Heap]] — parent concept
- [[Min Heap]] — root is minimum

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
