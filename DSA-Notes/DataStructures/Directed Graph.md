# Directed Graph

## 📌 Definition
A [[Graph]] where edges have a direction, going from one vertex to another. Also called a digraph.

## ⚙️ How It Works
- Edges represented as ordered pairs (u, v)
- In-degree: edges pointing TO a node; Out-degree: edges FROM a node
- Represented via adjacency list/matrix with direction
- Can contain cycles (detect with DFS + coloring)

## 🧠 When to Use
- Dependencies (build systems, course prerequisites)
- Social networks (followers, not mutual)
- State machines
- Task scheduling (Topological Sort)

## 🔥 Key Properties
- Acyclic directed graph = DAG (used in Topological Sort)
- Strongly Connected Component: every node reachable from every other
- Transpose graph: reverse all edges

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
- [[DSA Problem Bank#Course Schedule II]]
- [[DSA Problem Bank#Find Eventual Safe States]]
- [[DSA Problem Bank#All Paths From Source to Target]]

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
