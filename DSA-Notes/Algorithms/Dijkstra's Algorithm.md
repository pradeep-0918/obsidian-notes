# ⚙️ Dijkstra's Algorithm

#Algorithm #Graph #ShortestPath

⬅️ [[Topological Sort]] | ➡️ [[Bellman-Ford Algorithm]]

---

## 📌 Definition

**Dijkstra's Algorithm** finds the shortest path from a source vertex to all other vertices in a **non-negative weighted** graph.

---

## ⚙️ How It Works

```python
import heapq

def dijkstra(graph, src, n):
    dist = [float('inf')] * n
    dist[src] = 0
    heap = [(0, src)]  # (distance, node)
    
    while heap:
        d, u = heapq.heappop(heap)
        if d > dist[u]:
            continue  # outdated entry
        for v, w in graph[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                heapq.heappush(heap, (dist[v], v))
    
    return dist
```

**Key:** Uses a **min-heap** (priority queue) to always process the nearest unvisited node.

---

## 🧠 When to Use

- Shortest path with non-negative weights
- Navigation (GPS)
- Network routing

⚠️ **Does NOT work with negative edge weights** → Use [[Bellman-Ford Algorithm]]

---

## ⏱ Complexity

| Implementation | Time |
|----------------|------|
| Min-heap | O((V+E) log V) |
| Fibonacci heap | O(E + V log V) |
| Space | O(V) |

---

## 🧪 Problems

### Medium
- [Network Delay Time](https://leetcode.com/problems/network-delay-time/)
- [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)
- [Path With Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)

### Hard
- [Swim in Rising Water](https://leetcode.com/problems/swim-in-rising-water/)

---

## 🔗 Related Topics

- [[Breadth First Search]] — unweighted shortest path
- [[Bellman-Ford Algorithm]] — handles negative weights
- [[Heap]] — priority queue implementation
- [[Graph]] — applied to weighted graphs
- [[Shortest Path Algorithms]] — family of algorithms

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
