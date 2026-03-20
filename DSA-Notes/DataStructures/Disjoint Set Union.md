# 📦 Disjoint Set Union (Union-Find)

#DataStructure #Advanced #Graph

⬅️ [[Octree]] | ➡️ [[Skip List]]

---

## 📌 Definition

**Disjoint Set Union (DSU)** or **Union-Find** is a data structure that tracks a set of elements partitioned into disjoint (non-overlapping) subsets. Supports two operations efficiently: `union` and `find`.

---

## ⚙️ How It Works

```python
class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])  # Path compression
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py:
            return False
        if self.rank[px] < self.rank[py]:
            px, py = py, px
        self.parent[py] = px  # Union by rank
        if self.rank[px] == self.rank[py]:
            self.rank[px] += 1
        return True
```

**Optimizations:**
1. **Path compression** — flatten tree during find
2. **Union by rank** — attach smaller tree under taller

---

## 🧠 When to Use

- Connected components
- Cycle detection in undirected graphs
- Minimum spanning tree (Kruskal's)
- Network connectivity
- Merging sets of accounts, friends

---

## ⏱ Complexity

| Operation | Time (with both optimizations) |
|-----------|-------------------------------|
| find | O(α(n)) ≈ O(1) |
| union | O(α(n)) ≈ O(1) |
| Space | O(n) |

*α = inverse Ackermann function, practically constant*

---

## 🧪 Problems

### Medium
- [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
- [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
- [Accounts Merge](https://leetcode.com/problems/accounts-merge/)
- [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)

### Hard
- [Minimize Malware Spread](https://leetcode.com/problems/minimize-malware-spread/)
- [Making A Large Island](https://leetcode.com/problems/making-a-large-island/)

---

## 🔗 Related Topics

- [[Graph]] — used in graph problems
- [[Minimum Spanning Tree]] — Kruskal's uses DSU
- [[Topological Sort]] — alternative for connectivity
- [[Depth First Search]] — alternative for connected components

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
