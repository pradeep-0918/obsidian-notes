# Kruskal's Algorithm

## 📌 Definition
A greedy algorithm that finds the **Minimum Spanning Tree** of a connected, weighted graph by sorting edges by weight and adding them one by one if they don't form a cycle.

## ⚙️ How It Works
1. Sort all edges by weight (ascending)
2. Initialise each vertex as its own component ([[Disjoint Set Union]])
3. For each edge (u, v, w) in sorted order:
   - If u and v are in different components → add edge to MST, union them
   - Else → skip (would form a cycle)
4. Stop when MST has V-1 edges

## 🧠 When to Use
- Finding Minimum Spanning Tree
- Sparse graphs (fewer edges)
- Network design (cheapest connections)

## 🔥 Key Properties
- Greedy: always picks the cheapest safe edge
- Requires [[Union Find]] for cycle detection
- Works on disconnected graphs (produces spanning forest)

## ⏱ Complexity
| | Time | Space |
|-|------|-------|
| | O(E log E) | O(V) |

## 🆚 Comparison
| Algorithm | Best For | Time |
|-----------|---------|------|
| Kruskal's | Sparse graphs | O(E log E) |
| [[Prim's Algorithm]] | Dense graphs | O(E log V) |

## 🧪 Problems
- [[DSA Problem Bank#Min Cost to Connect All Points]]
- [[DSA Problem Bank#Connecting Cities With Minimum Cost]]

## ❌ Mistakes
- Forgetting to sort edges first
- Not using Union Find (naive cycle detection is O(V))

## 🔗 Related Topics
[[Minimum Spanning Tree]] | [[Prim's Algorithm]] | [[Union Find]] | [[Disjoint Set Union]] | [[Greedy Algorithms]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Graph #MST #Algorithm
