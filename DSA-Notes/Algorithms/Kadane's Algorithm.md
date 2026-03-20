# ⚙️ Kadane's Algorithm

#Algorithm #Array #DP

⬅️ [[Prefix Sum]] | ➡️ [[Binary Search]]

---

## 📌 Definition

**Kadane's Algorithm** finds the maximum sum contiguous subarray in O(n) time.

---

## ⚙️ How It Works

```python
def max_subarray(arr):
    max_sum = arr[0]
    curr_sum = arr[0]
    
    for i in range(1, len(arr)):
        curr_sum = max(arr[i], curr_sum + arr[i])
        max_sum = max(max_sum, curr_sum)
    
    return max_sum
```

**Intuition:** At each position, decide: "extend previous subarray OR start fresh here?"

---

## 🧠 When to Use

- Maximum subarray sum
- Circular subarray (use both max and min kadane)
- Maximum product subarray (track min too)

---

## ⏱ Complexity

| Metric | Value |
|--------|-------|
| Time | O(n) |
| Space | O(1) |

---

## 🧪 Problems

### Medium
- [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)
- [Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)
- [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)
- [Best Sightseeing Pair](https://leetcode.com/problems/best-sightseeing-pair/)

---

## 🔗 Related Topics

- [[Dynamic Programming]] — Kadane's is DP
- [[Array]] — applied to arrays
- [[Prefix Sum]] — alternative approach

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
