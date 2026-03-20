# Shortest Path Algorithms

## 📌 Definition
A family of algorithms that find the minimum cost path between vertices in a weighted graph.

## 🧠 Choosing the Right Algorithm
| Scenario | Algorithm |
|----------|-----------|
| Single source, non-negative weights | [[Dijkstra's Algorithm]] |
| Single source, negative weights | [[Bellman-Ford Algorithm]] |
| All pairs, any weights | [[Floyd-Warshall Algorithm]] |
| Unweighted graph | [[Breadth First Search]] |
| DAG (no cycles) | [[Topological Sort]] + relaxation |

## ⏱ Complexity Summary
| Algorithm | Time | Space | Neg. Weights |
|-----------|------|-------|-------------|
| BFS | O(V+E) | O(V) | ❌ |
| Dijkstra | O((V+E) log V) | O(V) | ❌ |
| Bellman-Ford | O(V·E) | O(V) | ✅ |
| Floyd-Warshall | O(V³) | O(V²) | ✅ |

## 🧪 Problems
- [[DSA Problem Bank#Network Delay Time]]
- [[DSA Problem Bank#Cheapest Flights Within K Stops]]
- [[DSA Problem Bank#Path With Minimum Effort]]
- [[DSA Problem Bank#Find the City With the Smallest Number of Neighbors]]

## 🔗 Related Topics
[[Dijkstra's Algorithm]] | [[Bellman-Ford Algorithm]] | [[Floyd-Warshall Algorithm]] | [[Breadth First Search]] | [[Weighted Graph]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Graph #ShortestPath #Algorithm
