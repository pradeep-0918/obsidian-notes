# ⚙️ Depth First Search (DFS)

#Algorithm #Graph #Tree #DFS

⬅️ [[Algorithms Index]] | ➡️ [[Breadth First Search]]

---

## 📌 Definition

**DFS** explores as far as possible along each branch before backtracking. Uses a **stack** (explicit or call stack via recursion).

---

## ⚙️ How It Works

```python
# Recursive DFS on Graph
def dfs(graph, node, visited=set()):
    if node in visited:
        return
    visited.add(node)
    print(node)
    for neighbor in graph[node]:
        dfs(graph, neighbor, visited)

# Iterative DFS
def dfs_iter(graph, start):
    stack = [start]
    visited = set()
    while stack:
        node = stack.pop()
        if node in visited:
            continue
        visited.add(node)
        for neighbor in graph[node]:
            stack.append(neighbor)

# DFS on Binary Tree (all 3 traversals)
def preorder(root):   # Root → Left → Right
    if not root: return
    visit(root)
    preorder(root.left)
    preorder(root.right)

def inorder(root):    # Left → Root → Right (BST sorted!)
    if not root: return
    inorder(root.left)
    visit(root)
    inorder(root.right)

def postorder(root):  # Left → Right → Root
    if not root: return
    postorder(root.left)
    postorder(root.right)
    visit(root)
```

---

## 🧠 When to Use

- Explore all paths (backtracking)
- Topological sort
- Cycle detection
- Connected components
- Tree traversal (pre/in/post order)
- Path finding

---

## ⏱ Complexity

| Case | Time | Space |
|------|------|-------|
| Graph | O(V+E) | O(V) |
| Tree | O(n) | O(h) |

*h = tree height, V = vertices, E = edges*

---

## 🧪 Problems

### Medium
- [Number of Islands](https://leetcode.com/problems/number-of-islands/)
- [Clone Graph](https://leetcode.com/problems/clone-graph/)
- [Course Schedule](https://leetcode.com/problems/course-schedule/)
- [All Paths From Source to Target](https://leetcode.com/problems/all-paths-from-source-to-target/)
- [Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)

### Hard
- [Word Search II](https://leetcode.com/problems/word-search-ii/)
- [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)

---

## 🔗 Related Topics

- [[Breadth First Search]] — level-order, shortest path
- [[Backtracking]] — DFS with pruning
- [[Topological Sort]] — DFS-based ordering
- [[Graph]] — primary application
- [[Binary Tree]] — tree traversals
- [[Stack]] — explicit DFS implementation
- [[Recursion]] — implicit stack

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
