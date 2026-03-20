# 🌐 Graph
#DataStructure #NonLinear #Graph

⬅️ [[Max Heap]] | ➡️ [[Directed Graph]]

---

## 📌 Definition
A **graph** G = (V, E) consists of a set of **vertices** (nodes) V and **edges** E connecting them. Unlike trees, graphs can have cycles.

---

## ⚙️ Representations

### Adjacency List (most common)
```python
graph = {
    0: [1, 2],
    1: [0, 3],
    2: [0],
    3: [1]
}
```

### Adjacency Matrix
```python
# graph[u][v] = 1 if edge u→v
matrix = [[0,1,1,0],
          [1,0,0,1],
          [1,0,0,0],
          [0,1,0,0]]
```

---

## 🔥 Types
- [[Directed Graph]] — one-way edges
- [[Undirected Graph]] — two-way edges
- [[Weighted Graph]] — edges have weights

---

## 🧠 When to Use
- Model relationships (social networks, maps)
- Path finding (Dijkstra, BFS)
- Dependency resolution (topological sort)
- Connectivity (union-find)

---

## ⏱ Complexity

| Representation | Space | Edge Check | Neighbors |
|---|---|---|---|
| Adjacency List | O(V+E) | O(degree) | O(degree) |
| Adjacency Matrix | O(V²) | O(1) | O(V) |

---

## 🔥 Key Graph Algorithms
| Problem | Algorithm |
|---|---|
| Shortest path (unweighted) | [[Breadth First Search]] |
| Shortest path (weighted) | [[Dijkstra's Algorithm]] |
| All-pairs shortest path | [[Floyd-Warshall]] |
| Cycle detection | DFS / [[Disjoint Set Union]] |
| Topological ordering | [[Topological Sort]] |
| MST | [[Kruskal's Algorithm]] / [[Prim's Algorithm]] |
| Connected components | DFS / [[Disjoint Set Union]] |

---

## 🧪 Problems
- [ ] [Number of Islands](https://leetcode.com/problems/number-of-islands/) — Medium
- [ ] [Clone Graph](https://leetcode.com/problems/clone-graph/) — Medium
- [ ] [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) — Medium
- [ ] [Network Delay Time](https://leetcode.com/problems/network-delay-time/) — Medium
- [ ] [Word Ladder](https://leetcode.com/problems/word-ladder/) — Hard

---

## 🔗 Related Topics
- [[Depth First Search]]
- [[Breadth First Search]]
- [[Topological Sort]]
- [[Disjoint Set Union]]
- [[Dijkstra's Algorithm]]

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
