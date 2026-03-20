# ⚙️ Dynamic Programming

#Algorithm #DP #DynamicProgramming

⬅️ [[Greedy Algorithms]] | ➡️ [[Backtracking]]

---

## 📌 Definition

**Dynamic Programming** solves complex problems by breaking them into overlapping subproblems and storing results to avoid redundant computation.

---

## ⚙️ Two Approaches

### 1. Top-Down (Memoization)
```python
from functools import lru_cache

def fib(n):
    @lru_cache(None)
    def dp(i):
        if i <= 1: return i
        return dp(i-1) + dp(i-2)
    return dp(n)
```

### 2. Bottom-Up (Tabulation)
```python
def fib(n):
    if n <= 1: return n
    dp = [0] * (n+1)
    dp[1] = 1
    for i in range(2, n+1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

---

## 🧠 When to Use

**Two conditions MUST hold:**
1. **Optimal Substructure** — optimal solution uses optimal solutions to subproblems
2. **Overlapping Subproblems** — same subproblems solved repeatedly

**Recognition signals:**
- "Maximum/minimum" + "number of ways"
- Counting distinct paths/sequences
- Optimization over subsequence/subset
- String similarity (LCS, edit distance)

---

## 🔥 DP Pattern Templates

### 1D DP (Sequences)
```
dp[i] = f(dp[i-1], dp[i-2], ...)
```
Problems: Climbing Stairs, House Robber, Fibonacci

### 2D DP (Grids, Strings)
```
dp[i][j] = f(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
```
Problems: LCS, Edit Distance, Unique Paths

### Knapsack
```
dp[i][w] = max(dp[i-1][w], dp[i-1][w-weight[i]] + value[i])
```

### Interval DP
```
dp[i][j] = min over k of dp[i][k] + dp[k+1][j] + cost
```

---

## ⏱ Complexity

Varies by problem:
- 1D DP: O(n) time, O(n) or O(1) space
- 2D DP: O(n·m) time, O(n·m) space
- Knapsack: O(n·W) time, O(W) space optimized

---

## 🧪 Problems

### Easy
- [Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
- [Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/)

### Medium
- [House Robber](https://leetcode.com/problems/house-robber/)
- [Coin Change](https://leetcode.com/problems/coin-change/)
- [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)
- [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
- [Word Break](https://leetcode.com/problems/word-break/)
- [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

### Hard
- [Edit Distance](https://leetcode.com/problems/edit-distance/)
- [Burst Balloons](https://leetcode.com/problems/burst-balloons/)
- [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/)

---

## 🔗 Related Topics

- [[Knapsack DP]] — 0/1 knapsack variant
- [[Unbounded Knapsack DP]] — unlimited items
- [[String DP]] — LCS, edit distance
- [[2D Grid DP]] — grid path problems
- [[Bitmask DP]] — subset DP
- [[Memoization]] — top-down DP
- [[Recursion]] — base of top-down

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
