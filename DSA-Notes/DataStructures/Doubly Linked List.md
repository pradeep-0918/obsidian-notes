# 📦 Doubly Linked List

#DataStructure #Linear #LinkedList

⬅️ [[Singly Linked List]] | ➡️ [[Circular Linked List]]

---

## 📌 Definition

A **Doubly Linked List** has nodes with **two** pointers: `prev` and `next`, allowing traversal in both directions.

```
null ← [1|←→] ↔ [2|←→] ↔ [3|←→] → null
```

---

## ⚙️ Implementation

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.prev = None
        self.next = None
```

---

## 🧠 When to Use

- LRU Cache (O(1) delete from middle)
- Browser history (forward/backward)
- Undo/Redo

---

## 🧪 Problems

- [LRU Cache](https://leetcode.com/problems/lru-cache/)
- [Design Browser History](https://leetcode.com/problems/design-browser-history/)
- [Flatten a Multilevel Doubly Linked List](https://leetcode.com/problems/flatten-a-multilevel-doubly-linked-list/)

---

## 🔗 Related Topics

- [[Singly Linked List]] — simpler variant
- [[Hash Map]] — combined with DLL for LRU Cache

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
