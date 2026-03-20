# ⚙️ Topological Sort

#Algorithm #Graph #TopologicalSort

⬅️ [[Breadth First Search]] | ➡️ [[Dijkstra's Algorithm]]

---

## 📌 Definition

**Topological Sort** orders vertices in a DAG (Directed Acyclic Graph) such that for every edge u→v, u comes before v. Used for dependency resolution.

---

## ⚙️ How It Works

### Kahn's Algorithm (BFS-based)
```python
from collections import deque

def topo_sort(n, edges):
    graph = [[] for _ in range(n)]
    in_degree = [0] * n
    
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1
    
    queue = deque([i for i in range(n) if in_degree[i] == 0])
    order = []
    
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    
    return order if len(order) == n else []  # Empty = cycle detected
```

### DFS-based
```python
def topo_dfs(graph, n):
    visited = [0] * n  # 0=unvisited, 1=visiting, 2=done
    result = []
    
    def dfs(node):
        if visited[node] == 1: return False  # cycle
        if visited[node] == 2: return True   # already done
        visited[node] = 1
        for neighbor in graph[node]:
            if not dfs(neighbor): return False
        visited[node] = 2
        result.append(node)
        return True
    
    for i in range(n):
        if not dfs(i): return []
    return result[::-1]
```

---

## 🧠 When to Use

- Course prerequisites / task scheduling
- Build system dependencies
- Package managers
- Any "do X before Y" problem

---

## ⏱ Complexity

| Metric | Value |
|--------|-------|
| Time | O(V+E) |
| Space | O(V+E) |

---

## 🧪 Problems

### Medium
- [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
- [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)
- [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

### Hard
- [Sort Items by Groups](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)

---

## 🔗 Related Topics

- [[Graph]] — applied to DAG
- [[Depth First Search]] — DFS-based topo sort
- [[Breadth First Search]] — Kahn's algorithm
- [[Disjoint Set Union]] — alternative for connectivity

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
