# 📦 Binary Tree

#DataStructure #NonLinear #Tree

⬅️ [[Tree]] | ➡️ [[Binary Search Tree]]

---

## 📌 Definition

A **Binary Tree** is a tree where each node has **at most 2 children**: left and right.

```
        1
       / \
      2   3
     / \   \
    4   5   6
```

---

## ⚙️ How It Works

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

**Traversal types:**
- **Preorder**: Root → Left → Right
- **Inorder**: Left → Root → Right (sorted for BST)
- **Postorder**: Left → Right → Root
- **Level order**: BFS level by level

---

## 🧠 When to Use

- Hierarchical data
- Expression trees
- Base for BST, Heap, Segment Tree

---

## 🔥 Key Properties

- **Height** h: best O(log n), worst O(n) (skewed)
- **Complete** — all levels full except possibly last
- **Perfect** — all internal nodes have 2 children, all leaves at same level
- **Balanced** — height = O(log n)

---

## ⏱ Complexity

| Operation | Balanced | Unbalanced |
|-----------|---------|------------|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Space | O(n) | O(n) |

---

## 🧪 Problems

### Easy
- [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)
- [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
- [Same Tree](https://leetcode.com/problems/same-tree/)
- [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)
- [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

### Medium
- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [Path Sum III](https://leetcode.com/problems/path-sum-iii/)
- [Lowest Common Ancestor](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [Construct Tree from Preorder and Inorder](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

### Hard
- [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
- [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)

---

## 🔗 Related Topics

- [[Binary Search Tree]] — ordered binary tree
- [[AVL Tree]] — self-balancing BST
- [[Heap]] — complete binary tree
- [[Segment Tree]] — range query tree
- [[Depth First Search]] — tree traversal
- [[Breadth First Search]] — level order traversal

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
