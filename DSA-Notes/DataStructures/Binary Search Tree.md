# 📦 Binary Search Tree (BST)

#DataStructure #Tree #BST

⬅️ [[Binary Tree]] | ➡️ [[AVL Tree]]

---

## 📌 Definition

A **BST** is a [[Binary Tree]] with the property: for every node, **all left subtree values < node < all right subtree values**.

---

## ⚙️ Key Operations

```python
def search(root, val):
    if not root or root.val == val:
        return root
    if val < root.val:
        return search(root.left, val)
    return search(root.right, val)

def insert(root, val):
    if not root:
        return TreeNode(val)
    if val < root.val:
        root.left = insert(root.left, val)
    else:
        root.right = insert(root.right, val)
    return root
```

**Inorder traversal of BST = sorted order!**

---

## ⏱ Complexity

| Operation | Balanced | Skewed |
|-----------|---------|--------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

---

## 🧪 Problems

### Easy
- [Validate BST (Easy)](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)
- [Convert Sorted Array to BST](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

### Medium
- [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
- [Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)
- [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)

---

## 🔗 Related Topics

- [[Binary Tree]] — parent concept
- [[AVL Tree]] — self-balancing BST
- [[Trie]] — string-based tree

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
