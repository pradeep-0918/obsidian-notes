# ⚙️ Union Find

#Algorithm #Graph #UnionFind

⬅️ [[Topological Sort]] | ➡️ [[Minimum Spanning Tree]]

---

## 📌 Definition

**Union Find** (Disjoint Set Union) is an algorithm for grouping elements into sets and merging them. See [[Disjoint Set Union]] for the data structure details.

---

## ⚙️ Template

```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
        self.count = n  # number of components
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        px, py = self.find(x), self.find(y)
        if px == py: return False
        if self.rank[px] < self.rank[py]: px, py = py, px
        self.parent[py] = px
        if self.rank[px] == self.rank[py]: self.rank[px] += 1
        self.count -= 1
        return True
    
    def connected(self, x, y):
        return self.find(x) == self.find(y)
```

---

## 🧠 When to Use

- Count connected components
- Detect cycles in undirected graph
- Kruskal's MST
- Merging groups (accounts, pixels)

---

## 🧪 Problems

### Medium
- [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
- [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
- [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

### Hard
- [Minimize Malware Spread](https://leetcode.com/problems/minimize-malware-spread/)

---

## 🔗 Related Topics

- [[Disjoint Set Union]] — the data structure
- [[Graph]] — applied to graphs
- [[Minimum Spanning Tree]] — Kruskal's uses this
- [[Depth First Search]] — alternative for components

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
