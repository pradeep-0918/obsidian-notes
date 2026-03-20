# 📦 Singly Linked List

#DataStructure #Linear #LinkedList

⬅️ [[Linked List]] | ➡️ [[Doubly Linked List]]

---

## 📌 Definition

A **Singly Linked List** is a [[Linked List]] where each node has a single pointer — only to the **next** node.

```
[Head|→] → [1|→] → [2|→] → [3|null]
```

---

## ⚙️ Implementation

```python
class Node:
    def __init__(self, val):
        self.val = val
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def insert_front(self, val):
        node = Node(val)
        node.next = self.head
        self.head = node
```

---

## ⏱ Complexity

| Operation | Time |
|-----------|------|
| Access | O(n) |
| Insert front | O(1) |
| Insert back | O(n) or O(1) with tail |
| Delete | O(n) |

---

## 🔗 Related Topics

- [[Linked List]] — parent concept
- [[Doubly Linked List]] — bidirectional
- [[Stack]] — implemented with SLL

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
