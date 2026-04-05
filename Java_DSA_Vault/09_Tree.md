# 09 — Tree & Binary Tree in Java

#java #tree #binarytree #bst #syntax #dsa
← [[08_Graph]] | Next → [[10_Builtin_Functions]]

---

## 🧠 Feynman Explain

A **Tree** is an upside-down family tree — one root, branches going down.  
A **Binary Tree** = each node has at most **2 children** (left and right).  
A **BST** (Binary Search Tree) = left child < node < right child (keeps things sorted).

---

## 🔧 TreeNode Definition

```java
class TreeNode {
    int val;
    TreeNode left, right;

    TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}
```

---

## 🏗️ Build a Tree

```java
TreeNode root = new TreeNode(1);
root.left = new TreeNode(2);
root.right = new TreeNode(3);
root.left.left = new TreeNode(4);
root.left.right = new TreeNode(5);

//        1
//       / \
//      2   3
//     / \
//    4   5
```

---

## 🔍 DFS Traversals (Recursive)

```java
// In-order: Left → Root → Right  (gives sorted for BST)
void inorder(TreeNode root) {
    if (root == null) return;
    inorder(root.left);
    System.out.print(root.val + " ");
    inorder(root.right);
}

// Pre-order: Root → Left → Right  (good for copy/serialize)
void preorder(TreeNode root) {
    if (root == null) return;
    System.out.print(root.val + " ");
    preorder(root.left);
    preorder(root.right);
}

// Post-order: Left → Right → Root  (good for delete)
void postorder(TreeNode root) {
    if (root == null) return;
    postorder(root.left);
    postorder(root.right);
    System.out.print(root.val + " ");
}
```

---

## 🔍 BFS — Level Order Traversal

```java
List<List<Integer>> levelOrder(TreeNode root) {
    List<List<Integer>> result = new ArrayList<>();
    if (root == null) return result;

    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    while (!q.isEmpty()) {
        int size = q.size();  // CRITICAL: snapshot current level size
        List<Integer> level = new ArrayList<>();
        for (int i = 0; i < size; i++) {
            TreeNode node = q.poll();
            level.add(node.val);
            if (node.left != null) q.offer(node.left);
            if (node.right != null) q.offer(node.right);
        }
        result.add(level);
    }
    return result;
}
```

---

## 📏 Common Tree Properties

```java
// Height / Max Depth
int height(TreeNode root) {
    if (root == null) return 0;
    return 1 + Math.max(height(root.left), height(root.right));
}

// Diameter (longest path between any two nodes)
int diameter = 0;
int diameterHelper(TreeNode root) {
    if (root == null) return 0;
    int left = diameterHelper(root.left);
    int right = diameterHelper(root.right);
    diameter = Math.max(diameter, left + right);
    return 1 + Math.max(left, right);
}

// Count Nodes
int countNodes(TreeNode root) {
    if (root == null) return 0;
    return 1 + countNodes(root.left) + countNodes(root.right);
}

// Sum of all nodes
int sumNodes(TreeNode root) {
    if (root == null) return 0;
    return root.val + sumNodes(root.left) + sumNodes(root.right);
}

// Check if Balanced
boolean isBalanced(TreeNode root) {
    return checkHeight(root) != -1;
}
int checkHeight(TreeNode root) {
    if (root == null) return 0;
    int l = checkHeight(root.left); if (l == -1) return -1;
    int r = checkHeight(root.right); if (r == -1) return -1;
    if (Math.abs(l - r) > 1) return -1;
    return 1 + Math.max(l, r);
}
```

---

## 🌲 BST Operations

```java
// Search
TreeNode search(TreeNode root, int val) {
    if (root == null || root.val == val) return root;
    if (val < root.val) return search(root.left, val);
    return search(root.right, val);
}

// Insert
TreeNode insert(TreeNode root, int val) {
    if (root == null) return new TreeNode(val);
    if (val < root.val) root.left = insert(root.left, val);
    else if (val > root.val) root.right = insert(root.right, val);
    return root;
}

// Find min node in BST
TreeNode minNode(TreeNode root) {
    while (root.left != null) root = root.left;
    return root;
}

// Validate BST
boolean isValidBST(TreeNode root) {
    return validate(root, Long.MIN_VALUE, Long.MAX_VALUE);
}
boolean validate(TreeNode node, long min, long max) {
    if (node == null) return true;
    if (node.val <= min || node.val >= max) return false;
    return validate(node.left, min, node.val) && validate(node.right, node.val, max);
}
```

---

## 🔄 Path Patterns

```java
// Path Sum (root-to-leaf)
boolean hasPathSum(TreeNode root, int targetSum) {
    if (root == null) return false;
    if (root.left == null && root.right == null) return root.val == targetSum;
    return hasPathSum(root.left, targetSum - root.val) ||
           hasPathSum(root.right, targetSum - root.val);
}

// Max Path Sum (through any nodes)
int maxSum = Integer.MIN_VALUE;
int maxPathSumHelper(TreeNode root) {
    if (root == null) return 0;
    int left  = Math.max(0, maxPathSumHelper(root.left));
    int right = Math.max(0, maxPathSumHelper(root.right));
    maxSum = Math.max(maxSum, left + right + root.val);
    return root.val + Math.max(left, right);
}
```

---

## 🃏 Shortcut Cheatsheet

| Task | Code |
|------|------|
| Inorder | L → Root → R |
| Preorder | Root → L → R |
| Postorder | L → R → Root |
| Level order | BFS with queue; snapshot size |
| Height | `1 + max(h(left), h(right))` |
| BST search | Compare, go left or right |
| BST insert | Recurse to null, return new node |
| Validate BST | Pass min/max range down |

---

## 🔗 Related Notes
- [[05_Queue]] — BFS level order uses Queue
- [[06_Stack]] — Iterative DFS uses Stack
- [[08_Graph]] — Trees are acyclic connected graphs
