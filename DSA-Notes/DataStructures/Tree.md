# 🌳 Tree
#DataStructure #NonLinear #Tree

⬅️ [[Priority Queue]] | ➡️ [[Binary Tree]]

---

## 📌 Definition
A **tree** is a hierarchical, non-linear data structure with a root node and subtrees of children, with no cycles.

---

## ⚙️ Terminology
- **Root** — topmost node (no parent)
- **Leaf** — node with no children
- **Height** — longest path from root to leaf
- **Depth** — distance from root to node
- **Subtree** — tree rooted at any node

---

## 🔥 Types of Trees
- [[Binary Tree]] — max 2 children
- [[Binary Search Tree]] — ordered binary tree
- [[AVL Tree]] — self-balancing BST
- [[Red-Black Tree]] — balanced BST (used in std::map)
- [[Segment Tree]] — range queries
- [[Fenwick Tree]] — prefix sums
- [[Trie]] — prefix tree for strings
- [[N-ary Tree]] — up to N children

---

## 🧠 When to Use
- Hierarchical data (file systems, org charts)
- Ordered data with fast search (BST)
- Range queries (Segment Tree)
- Prefix matching (Trie)

---

## 🔥 Traversals
```
        1
       / \
      2   3
     / \
    4   5
```
- **Preorder** (root, left, right): 1,2,4,5,3
- **Inorder** (left, root, right): 4,2,5,1,3
- **Postorder** (left, right, root): 4,5,2,3,1
- **Level order** (BFS): 1,2,3,4,5

---

## 🧪 Problems
- [ ] [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/) — Easy
- [ ] [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) — Easy
- [ ] [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) — Medium

---

## 🔗 Related Topics
- [[Binary Tree]]
- [[Depth First Search]]
- [[Breadth First Search]]
- [[Dynamic Programming]] — Tree DP

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
