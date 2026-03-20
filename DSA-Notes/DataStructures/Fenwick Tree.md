# 📦 Fenwick Tree (Binary Indexed Tree)

#DataStructure #Tree #Advanced

⬅️ [[Segment Tree]] | ➡️ [[Trie]]

---

## 📌 Definition

A **Fenwick Tree** (BIT) is a data structure providing efficient methods for range sum queries and point updates. Simpler than Segment Tree but less flexible.

---

## ⚙️ How It Works

Uses the lowest set bit (LSB) trick:
```python
class BIT:
    def __init__(self, n):
        self.n = n
        self.tree = [0] * (n + 1)
    
    def update(self, i, delta):  # 1-indexed
        while i <= self.n:
            self.tree[i] += delta
            i += i & (-i)  # move to parent
    
    def query(self, i):  # prefix sum [1..i]
        s = 0
        while i > 0:
            s += self.tree[i]
            i -= i & (-i)  # move to responsible range
        return s
    
    def range_query(self, l, r):
        return self.query(r) - self.query(l - 1)
```

---

## 🧠 When to Use

- Prefix sum with updates
- Counting inversions
- Rank queries

---

## ⏱ Complexity

| Operation | Time |
|-----------|------|
| Build | O(n log n) |
| Update | O(log n) |
| Query | O(log n) |
| Space | O(n) |

---

## 🧪 Problems

- [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)
- [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

---

## 🔗 Related Topics

- [[Segment Tree]] — more powerful alternative
- [[Prefix Sum]] — simpler, no updates

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
