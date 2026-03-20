# 📦 Red-Black Tree

#DataStructure #Tree #Balanced

⬅️ [[AVL Tree]] | ➡️ [[Segment Tree]]

---

## 📌 Definition

A **Red-Black Tree** is a self-balancing BST where nodes are colored red or black, maintaining balance through color properties.

---

## 🔥 Properties

1. Every node is red or black
2. Root is black
3. All leaves (NIL) are black
4. Red nodes have black children
5. All paths from any node to leaves have same number of black nodes

---

## 🆚 AVL vs Red-Black

| Feature | AVL Tree | Red-Black Tree |
|---------|---------|----------------|
| Balance | Stricter | Looser |
| Rotations | More (insert) | Fewer |
| Search | Faster | Slightly slower |
| Insert/Delete | Slower | Faster |
| Use case | Read-heavy | Write-heavy |

Used in: Java TreeMap, C++ std::map, Linux kernel

---

## 🔗 Related Topics

- [[AVL Tree]] — stricter balancing
- [[Binary Search Tree]] — base
- [[Hash Map]] — Java's TreeMap is RBT

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
