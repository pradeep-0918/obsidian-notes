# 📦 KD Tree

#DataStructure #Tree #Spatial

⬅️ [[Suffix Tree]] | ➡️ [[Quad Tree]]

---

## 📌 Definition

A **KD Tree** (K-Dimensional Tree) is a space-partitioning data structure for organizing k-dimensional points. Used for nearest neighbor search.

---

## ⚙️ How It Works

- Recursively partition space by alternating dimensions
- Each level splits by a different dimension

```
2D points: [(3,6), (17,15), (13,15), (6,12)]
Split by x → Split by y → ...
```

---

## ⏱ Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Build | O(n log n) | O(n log n) |
| Nearest neighbor | O(log n) | O(n) |
| Range search | O(√n + k) | O(n) |

---

## 🧠 When to Use

- Nearest neighbor queries
- Range queries on spatial data
- Geographic information systems

---

## 🔗 Related Topics

- [[Quad Tree]] — 2D space partitioning
- [[Binary Tree]] — base concept

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
