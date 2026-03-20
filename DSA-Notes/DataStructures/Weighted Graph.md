# Weighted Graph

## 📌 Definition
A [[Graph]] where each edge carries a numerical weight representing cost, distance, or capacity.

## ⚙️ How It Works
- Each edge (u, v) has an associated weight w
- Adjacency list stores (neighbor, weight) pairs
- Shortest paths consider cumulative weight
- Minimum spanning tree minimizes total edge weight

## 🧠 When to Use
- Maps / navigation (road distances)
- Network flow (bandwidth)
- Minimum Spanning Tree problems
- Shortest path with costs

## 🔥 Key Properties
- Negative weights: use Bellman-Ford, not Dijkstra
- Non-negative weights: Dijkstra's Algorithm optimal
- All-pairs shortest path: Floyd-Warshall

## ⏱ Complexity
| Representation | Space | Edge Lookup |
|---------------|-------|-------------|
| Adjacency List | O(V+E) | O(degree) |
| Adjacency Matrix | O(V²) | O(1) |

## 🆚 Comparison
| Type | Edge Direction | Weight | Algorithm |
|------|---------------|--------|-----------|
| Directed | Yes | Optional | Topological Sort, DFS |
| Undirected | No | Optional | BFS, Union Find |
| Weighted | Optional | Yes | Dijkstra, Bellman-Ford |

## 🧪 Problems
- [[DSA Problem Bank#Network Delay Time]]
- [[DSA Problem Bank#Cheapest Flights Within K Stops]]
- [[DSA Problem Bank#Min Cost to Connect All Points]]

## ❌ Mistakes
- Forgetting to check visited nodes (infinite loops)
- Not handling disconnected graphs
- Confusing directed and undirected edge representations in code

## 🔗 Related Topics
[[Graph]] | [[Depth First Search]] | [[Breadth First Search]] | [[Topological Sort]] | [[Dijkstra's Algorithm]] | [[Union Find]]

## ⬅️ Previous
[[Graph]]

## ➡️ Next
[[Disjoint Set Union]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Graph #DataStructure
