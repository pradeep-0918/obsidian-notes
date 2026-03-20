# Prim's Algorithm

## 📌 Definition
A greedy algorithm that builds a **Minimum Spanning Tree** by starting from an arbitrary vertex and greedily expanding the tree one vertex at a time, always picking the cheapest edge that connects the tree to a new vertex.

## ⚙️ How It Works
1. Start with any vertex, mark it visited
2. Add all edges from visited vertices to a min-heap
3. Extract minimum edge that connects to an unvisited vertex
4. Add that vertex and its connecting edge to MST
5. Repeat until all V vertices are in MST

## 🧠 When to Use
- Finding Minimum Spanning Tree
- Dense graphs (many edges)
- When starting from a specific node is natural

## 🔥 Key Properties
- Grows the MST one vertex at a time
- Uses a priority queue (min-heap)
- Always maintains a connected subtree

## ⏱ Complexity
| Implementation | Time |
|----------------|------|
| Binary heap | O(E log V) |
| Fibonacci heap | O(E + V log V) |
| Adjacency matrix | O(V²) |

## 🆚 Comparison
| Algorithm | Strategy | Best For |
|-----------|---------|---------|
| Prim's | Grow tree vertex-by-vertex | Dense graphs |
| [[Kruskal's Algorithm]] | Sort edges globally | Sparse graphs |

## 🧪 Problems
- [[DSA Problem Bank#Min Cost to Connect All Points]]

## ❌ Mistakes
- Not using a visited set (can re-add vertices)
- Confusing with Dijkstra (Prim tracks edge weight; Dijkstra tracks path weight)

## 🔗 Related Topics
[[Minimum Spanning Tree]] | [[Kruskal's Algorithm]] | [[Dijkstra's Algorithm]] | [[Priority Queue]] | [[Greedy Algorithms]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Graph #MST #Algorithm
