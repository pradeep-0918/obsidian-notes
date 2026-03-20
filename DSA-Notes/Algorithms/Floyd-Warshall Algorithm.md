# Floyd-Warshall Algorithm

## 📌 Definition
An all-pairs shortest path algorithm that finds shortest paths between **every pair** of vertices in a weighted graph, including those with negative edges (but not negative cycles).

## ⚙️ How It Works
- Uses dynamic programming with a 2D distance matrix `dist[i][j]`
- For each intermediate vertex k, update: `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`
- Three nested loops over all vertices

## 🧠 When to Use
- All-pairs shortest path needed
- Small dense graphs (V ≤ 500)
- Detecting negative cycles (dist[i][i] < 0)
- Transitive closure of a graph

## 🔥 Key Properties
- Works with negative edges (not negative cycles)
- O(V³) — only practical for small V
- Simple implementation

## ⏱ Complexity
| | Time | Space |
|-|------|-------|
| | O(V³) | O(V²) |

## 🆚 Comparison
| Algorithm | Pairs | Neg. Weights | Time |
|-----------|-------|-------------|------|
| [[Dijkstra's Algorithm]] | Single source | ❌ | O((V+E) log V) |
| [[Bellman-Ford Algorithm]] | Single source | ✅ | O(V·E) |
| Floyd-Warshall | All pairs | ✅ | O(V³) |

## 🧪 Problems
- [[DSA Problem Bank#Find the City With the Smallest Number of Neighbors]]
- [[DSA Problem Bank#Network Delay Time]]

## ❌ Mistakes
- Using on large sparse graphs (use Dijkstra from each source instead)
- Not initialising diagonal to 0 and off-diagonal to ∞

## 🔗 Related Topics
[[Bellman-Ford Algorithm]] | [[Dijkstra's Algorithm]] | [[Shortest Path Algorithms]] | [[Dynamic Programming]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Graph #ShortestPath #Algorithm
