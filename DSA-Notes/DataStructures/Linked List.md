# 📦 Linked List

#DataStructure #Linear #LinkedList

⬅️ [[String]] | ➡️ [[Singly Linked List]]

---

## 📌 Definition

A **Linked List** is a linear data structure where elements (nodes) are connected via pointers. Unlike arrays, elements are NOT stored in contiguous memory.

```
[Head] → [Node1|→] → [Node2|→] → [Node3|null]
```

---

## ⚙️ How It Works

Each node contains:
1. **Data** — the stored value
2. **Next** — pointer to next node (and Prev for doubly linked)

---

## 🧠 When to Use

- Frequent insert/delete at front or middle
- Unknown size at compile time
- Implementing stacks, queues
- LRU Cache (doubly linked + hash map)

---

## 🔥 Key Properties

- **Dynamic size** — no reallocation needed
- **No random access** — must traverse from head
- Variants: [[Singly Linked List]], [[Doubly Linked List]], [[Circular Linked List]]

---

## ⏱ Complexity

| Operation | Time |
|-----------|------|
| Access by index | O(n) |
| Search | O(n) |
| Insert at head | O(1) |
| Insert at tail | O(1) with tail ptr |
| Insert at middle | O(n) find + O(1) insert |
| Delete at head | O(1) |
| Space | O(n) |

---

## 🆚 Comparison

| Feature | Array | Linked List |
|---------|-------|------------|
| Random Access | O(1) | O(n) |
| Insert Front | O(n) | O(1) |
| Insert Back | O(1) | O(1) |
| Memory | Contiguous | Scattered |
| Cache | Friendly | Unfriendly |

---

## 🧪 Problems

### Easy
- [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
- [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)
- [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
- [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/)

### Medium
- [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
- [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
- [Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)
- [LRU Cache](https://leetcode.com/problems/lru-cache/)

### Hard
- [Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)
- [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

---

## ❌ Common Mistakes

- Losing head pointer (always save reference)
- Null pointer dereference
- Forgetting to update tail pointer
- Off-by-one in reversal (check boundary nodes)

---

## 🔗 Related Topics

- [[Singly Linked List]] — basic variant
- [[Doubly Linked List]] — bidirectional
- [[Circular Linked List]] — tail connects to head
- [[Stack]] — can be implemented with linked list
- [[Queue]] — can be implemented with linked list
- [[Fast and Slow Pointers]] — Floyd's cycle detection
- [[Two Pointers]] — find middle, kth from end

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
