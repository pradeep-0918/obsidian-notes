# Undirected Graph

## 📌 Definition
A [[Graph]] where edges have no direction — if A connects to B, then B connects to A.

## ⚙️ How It Works
- Edges represented as unordered pairs {u, v}
- Degree: number of edges connected to a node
- Adjacency list: node u lists v, and v lists u
- Connected component: maximal set of reachable nodes

## 🧠 When to Use
- Social networks (mutual friendships)
- Road maps (two-way streets)
- Network connectivity problems
- Bipartite matching

## 🔥 Key Properties
- Connected graph: path exists between every pair
- Tree = connected undirected acyclic graph
- Euler path/circuit applicable

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
- [[DSA Problem Bank#Number of Islands]]
- [[DSA Problem Bank#Number of Provinces]]
- [[DSA Problem Bank#Is Graph Bipartite?]]

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
