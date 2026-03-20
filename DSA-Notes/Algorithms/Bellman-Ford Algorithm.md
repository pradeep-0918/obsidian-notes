# Bellman-Ford Algorithm

## 📌 Definition
A single-source shortest path algorithm that works with **negative edge weights** and can detect negative weight cycles, unlike [[Dijkstra's Algorithm]].

## ⚙️ How It Works
1. Initialise distance to source = 0, all others = ∞
2. Repeat V-1 times: relax all edges (u,v,w): if dist[u]+w < dist[v], update dist[v]
3. Run one more pass: if any edge still relaxes, a negative cycle exists

## 🧠 When to Use
- Graphs with negative edge weights
- Detecting negative weight cycles
- Arbitrage detection in currency exchange

## 🔥 Key Properties
- Works on directed and undirected graphs
- Detects negative cycles
- Slower than Dijkstra but more versatile

## ⏱ Complexity
| | Time | Space |
|-|------|-------|
| | O(V·E) | O(V) |

## 🆚 Comparison
| Algorithm | Negative Weights | Negative Cycles | Time |
|-----------|-----------------|----------------|------|
| [[Dijkstra's Algorithm]] | ❌ | ❌ | O((V+E) log V) |
| Bellman-Ford | ✅ | Detects | O(V·E) |
| [[Floyd-Warshall Algorithm]] | ✅ | Detects | O(V³) |

## 🧪 Problems
- [[DSA Problem Bank#Cheapest Flights Within K Stops]]
- [[DSA Problem Bank#Network Delay Time]]
- [[DSA Problem Bank#Find Negative Cycle]]

## ❌ Mistakes
- Running only V-1 iterations (need V-th pass for cycle detection)
- Using on undirected graphs with negative edges (all edges become negative cycles)

## 🔗 Related Topics
[[Dijkstra's Algorithm]] | [[Floyd-Warshall Algorithm]] | [[Shortest Path Algorithms]] | [[Weighted Graph]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Graph #ShortestPath #Algorithm
