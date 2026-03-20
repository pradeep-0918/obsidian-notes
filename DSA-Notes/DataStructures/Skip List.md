# 📦 Skip List

#DataStructure #Advanced #Probabilistic

⬅️ [[Disjoint Set Union]] | ➡️ [[Rope]]

---

## 📌 Definition

A **Skip List** is a probabilistic data structure that allows O(log n) average-case search, insert, and delete — like a balanced BST but implemented with linked lists and "express lanes".

---

## ⚙️ How It Works

```
Level 3: 1 ──────────────→ 9
Level 2: 1 ──→ 4 ──────→ 9
Level 1: 1 → 2 → 4 → 6 → 9
```

Each element is promoted to higher levels with probability p (usually 1/2).

---

## ⏱ Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Space | O(n log n) | O(n log n) |

---

## 🧠 When to Use

- When you need sorted set with concurrent access
- Redis Sorted Sets (implemented as skip list)

---

## 🔗 Related Topics

- [[Binary Search Tree]] — similar performance guarantee
- [[Linked List]] — base structure

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
