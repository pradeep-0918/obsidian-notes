# Unbounded Knapsack DP

## 📌 Definition
Knapsack variant where each item can be selected **unlimited times**. State: `dp[w]` = max value achievable with capacity w.

## ⚙️ How It Works
- For each capacity w, for each item: `dp[w] = max(dp[w], dp[w-wt[i]] + val[i])`
- Key difference from 0/1: iterate weights **forward** (allows item reuse)
- Inner loop over items, outer over weights (or vice versa — both work)

## 🧠 When to Use
- Coin change (unlimited coins)
- Rod cutting
- Item selection with repetition allowed

## 🧪 Problems
- [[DSA Problem Bank#Coin Change]]
- [[DSA Problem Bank#Coin Change II]]
- [[DSA Problem Bank#Perfect Squares]]
- [[DSA Problem Bank#Integer Break]]

## ❌ Mistakes
- Iterating weights in reverse (gives 0/1 knapsack behaviour instead)

## 🔗 Related Topics
[[Knapsack DP]] | [[Dynamic Programming]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #Knapsack #Algorithm
