# 🌳 Trees — Binary Trees

**← [[01_Graph_BFS_DFS]] | Next: [[03_Stack]] →**

---

## 1. 💡 INTUITION FIRST

> A tree is a **graph with no cycles** starting from one root node.
> Each node has at most **left** and **right** children.
>
> **Golden Rule:** Almost every tree problem = **recursion**.
> Think: *"What should I do at THIS node, and trust recursion handles the rest."*

---

## 2. 🔑 PATTERNS

| Pattern | Problems |
|---|---|
| Inorder / Preorder / Postorder | Traversal, BST validation |
| Height/Depth recursion | Max depth, balanced tree |
| Level Order (BFS on tree) | Level averages, zigzag |
| Path problems | Path sum, root to leaf |
| LCA | Lowest Common Ancestor |

**Recursive Formula for most tree problems:**
```
f(node) = combine( f(node.left), f(node.right) )
```

---

## 3. 🧩 PROBLEMS IN THIS NOTE

| # | Problem | Pattern | LeetCode |
|---|---|---|---|
| T1 | Maximum Depth | Recursion (postorder) | #104 |
| T2 | Level Order Traversal | BFS | #102 |
| T3 | Path Sum | DFS Recursion | #112 |

---

## 4. 📝 NODE STRUCTURE

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right
```

---
---

# 🌲 T1 — Maximum Depth of Binary Tree (LeetCode #104)

## ❓ Problem Statement

Given the root of a binary tree, return its **maximum depth** (number of nodes along the longest path from root to leaf).

**Input:**
```
        3
       / \
      9  20
         / \
        15   7
```
**Output:** `3`

---

## ✅ ANSWER

The maximum depth is **3** (path: 3 → 20 → 15 or 3 → 20 → 7).

---

## 🧠 FEYNMAN TECHNIQUE

> Imagine you're measuring the depth of a swimming pool, but the pool has branches (like a tree).
>
> You drop a measuring chain down the **left side** of the pool. It hits bottom and tells you the depth. You pull it back and drop it down the **right side**. It tells you that depth too.
>
> You take the **maximum of the two**, and add **1 for the current level** you're standing on.
>
> You keep doing this recursively — each node asks its left and right children for their depths, takes the max, and adds 1 for itself.
>
> **Base case:** When you reach a `null` (no water, no depth) → return 0.

---

## 🔄 STEP-BY-STEP DRY RUN

```
Tree:      3
          / \
         9  20
            / \
           15   7

maxDepth(3)
  ├── maxDepth(9)
  │     ├── maxDepth(null) → 0
  │     └── maxDepth(null) → 0
  │     └── return 1 + max(0,0) = 1
  └── maxDepth(20)
        ├── maxDepth(15)
        │     └── return 1 + max(0,0) = 1
        └── maxDepth(7)
              └── return 1 + max(0,0) = 1
        └── return 1 + max(1,1) = 2
  └── return 1 + max(1, 2) = 3  ✅
```

---

## ☕ JAVA — Full Code

```java
public class MaxDepth {

    static class TreeNode {
        int val;
        TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    public static int maxDepth(TreeNode root) {
        // Base case: empty node has depth 0
        if (root == null) return 0;

        // Recursively get depth of left and right subtrees
        int leftDepth = maxDepth(root.left);
        int rightDepth = maxDepth(root.right);

        // Current node adds 1 level to the deeper subtree
        return 1 + Math.max(leftDepth, rightDepth);
    }

    public static void main(String[] args) {
        // ── INPUT: Build the tree ──
        //       3
        //      / \
        //     9  20
        //        / \
        //       15   7
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(9);
        root.right = new TreeNode(20);
        root.right.left = new TreeNode(15);
        root.right.right = new TreeNode(7);

        // ── OUTPUT ──
        System.out.println("Maximum Depth: " + maxDepth(root));
        // Output: Maximum Depth: 3
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def maxDepth(root: TreeNode) -> int:
    # Base case
    if root is None:
        return 0

    left_depth = maxDepth(root.left)
    right_depth = maxDepth(root.right)

    return 1 + max(left_depth, right_depth)


# ── INPUT: Build the tree ──
root = TreeNode(3)
root.left = TreeNode(9)
root.right = TreeNode(20)
root.right.left = TreeNode(15)
root.right.right = TreeNode(7)

# ── OUTPUT ──
print("Maximum Depth:", maxDepth(root))
# Output: Maximum Depth: 3
```

---

## 📖 STORY TO REMEMBER — "The Treasure Diver"

> **Deep-sea diver Dan needs to find the deepest part of a coral reef.**
>
> At each branching point of the reef, Dan sends a **clone of himself** left and right. Each clone does the same — splits and explores deeper.
>
> When a clone hits a dead end (null), it radios back: **"Depth 0 here!"**
>
> Each branching point listens to both clones, takes the **deeper report**, adds **1 for its own level**, and radios that number up.
>
> Dan at the top gets the final answer.
>
> **Memory hook:** `"Split → Dive → Pick Deeper → Add 1 → Return Up"`

---
---

# 📊 T2 — Binary Tree Level Order Traversal (LeetCode #102)

## ❓ Problem Statement

Given the root of a binary tree, return the **level order traversal** of its nodes' values (i.e., from left to right, level by level).

**Input:**
```
    3
   / \
  9  20
     / \
    15   7
```
**Output:** `[[3], [9, 20], [15, 7]]`

---

## ✅ ANSWER

**`[[3], [9, 20], [15, 7]]`** — each inner list is one level of the tree.

---

## 🧠 FEYNMAN TECHNIQUE

> Picture a **fire drill at a school with 3 floors**.
>
> Floor 1 (root): 1 teacher evacuates. You note their names.
> Floor 2: 2 teachers evacuate. Note their names.
> Floor 3: 4 teachers evacuate. Note their names.
>
> You process **one entire floor at a time**. How do you know when a floor ends? You know how many people were on that floor BEFORE evacuating it (that's `queue.size()`).
>
> **The trick:** Capture the queue size BEFORE your inner loop. That size = exactly how many nodes are on the current level.

---

## 🔄 STEP-BY-STEP DRY RUN

```
Queue: [3]   → Level 1 size=1 → poll 3, add 9,20 → level=[3]
Queue: [9,20] → Level 2 size=2 → poll 9(no children), poll 20(add 15,7) → level=[9,20]
Queue: [15,7] → Level 3 size=2 → poll 15, poll 7 → level=[15,7]
Queue: []    → Done
Result: [[3],[9,20],[15,7]] ✅
```

---

## ☕ JAVA — Full Code

```java
import java.util.*;

public class LevelOrder {

    static class TreeNode {
        int val;
        TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    public static List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            int size = queue.size(); // CRITICAL: capture size BEFORE loop
            List<Integer> level = new ArrayList<>();

            for (int i = 0; i < size; i++) {
                TreeNode node = queue.poll();
                level.add(node.val);

                if (node.left != null)  queue.offer(node.left);
                if (node.right != null) queue.offer(node.right);
            }
            result.add(level);
        }
        return result;
    }

    public static void main(String[] args) {
        // ── INPUT ──
        TreeNode root = new TreeNode(3);
        root.left = new TreeNode(9);
        root.right = new TreeNode(20);
        root.right.left = new TreeNode(15);
        root.right.right = new TreeNode(7);

        // ── OUTPUT ──
        System.out.println("Level Order: " + levelOrder(root));
        // Output: Level Order: [[3], [9, 20], [15, 7]]
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
from collections import deque
from typing import List, Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def levelOrder(root: Optional[TreeNode]) -> List[List[int]]:
    result = []
    if not root:
        return result

    queue = deque([root])

    while queue:
        level_size = len(queue)   # capture size BEFORE loop
        level = []

        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)

            if node.left:  queue.append(node.left)
            if node.right: queue.append(node.right)

        result.append(level)

    return result


# ── INPUT ──
root = TreeNode(3)
root.left = TreeNode(9)
root.right = TreeNode(20)
root.right.left = TreeNode(15)
root.right.right = TreeNode(7)

# ── OUTPUT ──
print("Level Order:", levelOrder(root))
# Output: Level Order: [[3], [9, 20], [15, 7]]
```

---

## 📖 STORY TO REMEMBER — "The School Photo Day"

> **It's school photo day. Students are lined up by class (level).**
>
> The photographer calls out each class one at a time. Before snapping the photo, she counts **exactly how many kids** are in that line right now (= `queue.size()`). She photographs exactly that many kids, then calls the next class (their children join the end of the line).
>
> If she doesn't count first, she'll photograph kids from two different classes in one shot!
>
> **Memory hook:** `"Count the class → Photo → Add kids to line → Next class"`
> = capture size → process level → add children → repeat.

---
---

# 🎯 T3 — Path Sum (LeetCode #112)

## ❓ Problem Statement

Given root and `targetSum`, return `true` if the tree has a **root-to-leaf path** where all values sum to `targetSum`.

**Input:** root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
**Output:** `true` (path: 5 → 4 → 11 → 2)

---

## ✅ ANSWER

**`true`** — the path 5 → 4 → 11 → 2 sums to 22.

---

## 🧠 FEYNMAN TECHNIQUE

> You're hiking a forked trail, trying to collect exactly 22 berries before reaching a dead end (leaf).
>
> At each fork, you take one berry from the current bush. You now need `22 - bush_value` more berries from the rest of the trail.
>
> You explore both forks recursively. If you reach a dead end (leaf, no children) and you've collected exactly 22 → you win!
>
> **Key insight:** At each node, **subtract** the node's value from target. At a leaf, check if remainder is 0.

---

## 🔄 STEP-BY-STEP DRY RUN

```
Target = 22, Tree:
        5
       / \
      4   8
     /   / \
    11  13   4
   /  \       \
  7    2        1

hasPathSum(5, 22):
  hasPathSum(4, 17):
    hasPathSum(11, 13):
      hasPathSum(7, 2):  → leaf, 7 ≠ 2 → false
      hasPathSum(2, 2):  → leaf, 2 == 2 → TRUE ✅
```

---

## ☕ JAVA — Full Code

```java
public class PathSum {

    static class TreeNode {
        int val;
        TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    public static boolean hasPathSum(TreeNode root, int targetSum) {
        // No node → no path
        if (root == null) return false;

        // Leaf node: check if remaining sum equals node's value
        if (root.left == null && root.right == null) {
            return root.val == targetSum;
        }

        // Recurse with reduced target
        int remaining = targetSum - root.val;
        return hasPathSum(root.left, remaining) || hasPathSum(root.right, remaining);
    }

    public static void main(String[] args) {
        // ── INPUT ──
        //        5
        //       / \
        //      4   8
        //     /   / \
        //    11  13   4
        //   /  \       \
        //  7    2        1
        TreeNode root = new TreeNode(5);
        root.left = new TreeNode(4);
        root.right = new TreeNode(8);
        root.left.left = new TreeNode(11);
        root.left.left.left = new TreeNode(7);
        root.left.left.right = new TreeNode(2);
        root.right.left = new TreeNode(13);
        root.right.right = new TreeNode(4);
        root.right.right.right = new TreeNode(1);

        int targetSum = 22;

        // ── OUTPUT ──
        System.out.println("Has Path Sum " + targetSum + ": " + hasPathSum(root, targetSum));
        // Output: Has Path Sum 22: true
    }
}
```

---

## 🐍 PYTHON — Full Code

```python
from typing import Optional

class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

def hasPathSum(root: Optional[TreeNode], targetSum: int) -> bool:
    if not root:
        return False

    # Leaf node check
    if not root.left and not root.right:
        return root.val == targetSum

    remaining = targetSum - root.val
    return hasPathSum(root.left, remaining) or hasPathSum(root.right, remaining)


# ── INPUT ──
root = TreeNode(5)
root.left = TreeNode(4)
root.right = TreeNode(8)
root.left.left = TreeNode(11)
root.left.left.left = TreeNode(7)
root.left.left.right = TreeNode(2)
root.right.left = TreeNode(13)
root.right.right = TreeNode(4)
root.right.right.right = TreeNode(1)

# ── OUTPUT ──
print("Has Path Sum 22:", hasPathSum(root, 22))
# Output: Has Path Sum 22: True
```

---

## 📖 STORY TO REMEMBER — "The Berry Trail"

> **Hiker Harry needs to collect EXACTLY 22 berries on a forked trail.**
>
> At each bush, Harry picks all the berries there and mentally notes how many more he needs.
>
> When he reaches a **dead end (leaf)**, he checks: "Do I have exactly enough?" If yes — success! If no — this trail fails.
>
> He tries both forks at every junction. If **either fork works** → return true.
>
> **Memory hook:** `"Pick berries → Need less → Dead end check → Either fork wins"`
> = subtract value → recurse both → leaf check.

---

## ⚠️ COMMON MISTAKES (ALL TREE PROBLEMS)

- ❌ **Forgetting null check** at the top of every recursive function
- ❌ **Wrong leaf check** — a leaf is when BOTH left AND right are null
- ❌ **Not capturing queue.size()** before inner for-loop in level order
- ❌ **Assuming BST** properties when problem says "binary tree"
- ❌ **Returning wrong type** from recursion (int vs boolean)

---

## 📋 LEETCODE PRACTICE

| # | Problem | Level | Pattern |
|---|---|---|---|
| 104 | Maximum Depth of Binary Tree | Easy | Recursion |
| 226 | Invert Binary Tree | Easy | Recursion |
| 102 | Binary Tree Level Order Traversal | Medium | BFS |
| 112 | Path Sum | Easy | DFS Recursion |
| 236 | Lowest Common Ancestor | Medium | Recursion |

---

**← [[01_Graph_BFS_DFS]] | Next: [[03_Stack]] →**

*Tags: #trees #binary-tree #bfs #dfs #recursion #java #python #dsa*
