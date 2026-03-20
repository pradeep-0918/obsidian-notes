# 📦 AVL Tree

#DataStructure #Tree #Balanced

⬅️ [[Binary Search Tree]] | ➡️ [[Red-Black Tree]]

---

## 📌 Definition

An **AVL Tree** is a self-balancing [[Binary Search Tree]] where the height difference (balance factor) between left and right subtrees is at most 1.

---

## ⚙️ Balance Factor

```
balance_factor = height(left) - height(right)
Valid values: -1, 0, 1
```

**Rotations to rebalance:**
- Left Rotation
- Right Rotation
- Left-Right Rotation
- Right-Left Rotation

---

## ⏱ Complexity

| Operation | Time |
|-----------|------|
| Search | O(log n) guaranteed |
| Insert | O(log n) |
| Delete | O(log n) |

---

## 🔗 Related Topics

- [[Binary Search Tree]] — base structure
- [[Red-Black Tree]] — alternative balanced BST

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
