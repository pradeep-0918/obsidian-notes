# Eulerian Circuit

## 📌 Definition
An Eulerian circuit is a closed walk that visits **every edge** in a graph exactly once and returns to the starting vertex. An Eulerian path visits every edge once but may start and end at different vertices.

## ⚙️ How It Works
**Hierholzer's Algorithm:**
1. Start from any vertex with non-zero degree
2. Follow edges, removing them, until returning to start (forms a cycle)
3. Find any vertex in current cycle with remaining unused edges
4. Splice in a new sub-cycle from that vertex
5. Repeat until all edges used

**Existence conditions:**
- Undirected: Eulerian circuit exists iff all vertices have even degree and graph is connected
- Directed: Eulerian circuit exists iff every vertex has equal in-degree and out-degree

## 🧠 When to Use
- Chinese Postman Problem (traverse all streets)
- Route planning that covers all roads
- Reconstructing a sequence from overlapping parts (genome assembly)
- Validating circuit board designs

## ⏱ Complexity
| | Time | Space |
|-|------|-------|
| | O(V+E) | O(V+E) |

## 🧪 Problems
- [[DSA Problem Bank#Reconstruct Itinerary]]
- [[DSA Problem Bank#Valid Arrangement of Pairs]]

## ❌ Mistakes
- Confusing Eulerian (every edge once) with Hamiltonian (every vertex once)
- Not checking existence conditions before applying algorithm

## 🔗 Related Topics
[[Graph]] | [[Directed Graph]] | [[Depth First Search]] | [[Topological Sort]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Graph #EulerianCircuit #Algorithm
