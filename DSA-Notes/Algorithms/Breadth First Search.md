# ⚙️ Breadth First Search (BFS)

#Algorithm #Graph #Tree #BFS

⬅️ [[Depth First Search]] | ➡️ [[Topological Sort]]

---

## 📌 Definition

**BFS** explores all neighbors at the current depth before moving to the next level. Uses a **queue**. Finds **shortest path** in unweighted graphs.

---

## ⚙️ How It Works

```python
from collections import deque

def bfs(graph, start):
    queue = deque([start])
    visited = {start}
    
    while queue:
        node = queue.popleft()
        print(node)
        
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)

# BFS for shortest path with distance tracking
def bfs_distance(graph, start, end):
    queue = deque([(start, 0)])
    visited = {start}
    
    while queue:
        node, dist = queue.popleft()
        if node == end:
            return dist
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append((neighbor, dist+1))
    return -1

# Level-order BFS (tree)
def level_order(root):
    if not root: return []
    queue = deque([root])
    result = []
    while queue:
        level = []
        for _ in range(len(queue)):   # process one level at a time
            node = queue.popleft()
            level.append(node.val)
            if node.left: queue.append(node.left)
            if node.right: queue.append(node.right)
        result.append(level)
    return result
```

---

## 🧠 When to Use

- Shortest path in unweighted graph
- Level-order tree traversal
- Multi-source BFS (multiple starting points)
- State space search (min steps)

---

## ⏱ Complexity

| Case | Time | Space |
|------|------|-------|
| Graph | O(V+E) | O(V) |
| Tree | O(n) | O(w) |

*w = max width of tree*

---

## 🧪 Problems

### Medium
- [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
- [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)
- [01 Matrix](https://leetcode.com/problems/01-matrix/)
- [Open the Lock](https://leetcode.com/problems/open-the-lock/)

### Hard
- [Word Ladder](https://leetcode.com/problems/word-ladder/)
- [Bus Routes](https://leetcode.com/problems/bus-routes/)
- [Shortest Path in Grid with Obstacles](https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/)

---

## 🔗 Related Topics

- [[Depth First Search]] — explores deep, not wide
- [[Queue]] — core data structure
- [[Dijkstra's Algorithm]] — weighted BFS
- [[Graph]] — primary application
- [[Binary Tree]] — level-order traversal

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
