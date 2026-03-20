# Knapsack DP

## 📌 Definition
0/1 Knapsack pattern: for each item, decide to include or exclude. State: `dp[i][w]` = max value using first i items with weight capacity w.

## ⚙️ How It Works
- Include: `dp[i][w] = dp[i-1][w-wt[i]] + val[i]`
- Exclude: `dp[i][w] = dp[i-1][w]`
- Take the max of both choices
- Space optimisation: use 1D array, iterate weights in reverse

## 🧠 When to Use
- Weight constraints with 0/1 item selection
- Subset sum variants
- Partition problems

## 🧪 Problems
- [[DSA Problem Bank#Partition Equal Subset Sum]]
- [[DSA Problem Bank#Target Sum]]
- [[DSA Problem Bank#Last Stone Weight II]]

## ❌ Mistakes
- Forgetting to iterate weights in reverse for 1D optimisation
- Mixing up 0/1 vs unbounded knapsack loop order

## 🔗 Related Topics
[[Unbounded Knapsack DP]] | [[Dynamic Programming]] | [[Backtracking]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #Knapsack #Algorithm
