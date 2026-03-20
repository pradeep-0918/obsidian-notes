# 📦 Segment Tree

#DataStructure #Tree #Advanced

⬅️ [[AVL Tree]] | ➡️ [[Fenwick Tree]]

---

## 📌 Definition

A **Segment Tree** is a binary tree used for storing information about intervals or segments. Allows efficient range queries AND point updates in O(log n).

---

## ⚙️ How It Works

```
Array:   [1, 3, 5, 7, 9, 11]
Sum tree:
              36
           /      \
          9        27
         / \      / \
        4   5   16   11
       /\        /\
      1  3      7  9
```

```python
class SegTree:
    def __init__(self, n):
        self.tree = [0] * (4 * n)
    
    def build(self, arr, node, start, end):
        if start == end:
            self.tree[node] = arr[start]
        else:
            mid = (start + end) // 2
            self.build(arr, 2*node, start, mid)
            self.build(arr, 2*node+1, mid+1, end)
            self.tree[node] = self.tree[2*node] + self.tree[2*node+1]
    
    def query(self, node, start, end, l, r):
        if r < start or end < l:
            return 0
        if l <= start and end <= r:
            return self.tree[node]
        mid = (start + end) // 2
        return self.query(2*node, start, mid, l, r) +                self.query(2*node+1, mid+1, end, l, r)
```

---

## 🧠 When to Use

- Range sum/min/max queries with updates
- Count inversions
- Interval problems with dynamic data

---

## ⏱ Complexity

| Operation | Time |
|-----------|------|
| Build | O(n) |
| Query | O(log n) |
| Update | O(log n) |
| Space | O(n) |

---

## 🧪 Problems

- [Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)
- [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)

---

## 🔗 Related Topics

- [[Fenwick Tree]] — simpler alternative for sum queries
- [[Binary Tree]] — underlying structure
- [[Divide and Conquer]] — recursive build pattern

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
