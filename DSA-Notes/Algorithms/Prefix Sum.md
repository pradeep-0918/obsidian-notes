# ⚙️ Prefix Sum

#Algorithm #Pattern #PrefixSum

⬅️ [[Sliding Window]] | ➡️ [[Kadane's Algorithm]]

---

## 📌 Definition

**Prefix Sum** precomputes cumulative sums so any range sum [i, j] is answerable in O(1): `prefix[j+1] - prefix[i]`.

---

## ⚙️ How It Works

```python
def build_prefix(arr):
    prefix = [0] * (len(arr) + 1)
    for i, x in enumerate(arr):
        prefix[i+1] = prefix[i] + x
    return prefix

def range_sum(prefix, l, r):  # inclusive
    return prefix[r+1] - prefix[l]
```

### Prefix Sum + Hash for Subarray Sum = K
```python
def subarray_sum(arr, k):
    count = 0
    prefix = 0
    seen = {0: 1}
    for x in arr:
        prefix += x
        count += seen.get(prefix - k, 0)
        seen[prefix] = seen.get(prefix, 0) + 1
    return count
```

---

## 🧠 When to Use

- Range sum queries on static array
- Subarray sum = K
- Subarray sum divisible by K (modulo trick)
- 2D prefix sums for matrix queries

---

## ⏱ Complexity

| Operation | Time |
|-----------|------|
| Build prefix | O(n) |
| Range query | O(1) |
| Space | O(n) |

---

## 🧪 Problems

### Easy
- [Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

### Medium
- [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)
- [Contiguous Array](https://leetcode.com/problems/contiguous-array/)
- [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

---

## 🔗 Related Topics

- [[Array]] — applied to arrays
- [[Hash Map]] — prefix + hash for subarray sum
- [[Segment Tree]] — dynamic prefix sums
- [[Fenwick Tree]] — dynamic prefix sums

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
