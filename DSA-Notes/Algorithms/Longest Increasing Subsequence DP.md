# Longest Increasing Subsequence DP

## 📌 Definition
Classic DP pattern finding the longest strictly increasing subsequence (LIS) in an array.

## ⚙️ How It Works
**O(n²) approach:**
- `dp[i]` = length of LIS ending at index i
- `dp[i] = max(dp[j] + 1)` for all `j < i` where `a[j] < a[i]`

**O(n log n) approach (patience sort):**
- Maintain `tails[]` array where `tails[i]` = smallest tail of all IS of length i+1
- For each element, binary search in `tails` and replace or extend

## 🧠 When to Use
- LIS directly
- Russian Doll Envelopes (sort by one dimension, LIS on other)
- Box stacking, chain problems

## 🧪 Problems
- [[DSA Problem Bank#Longest Increasing Subsequence]]
- [[DSA Problem Bank#Russian Doll Envelopes]]
- [[DSA Problem Bank#Number of Longest Increasing Subsequence]]
- [[DSA Problem Bank#Longest Divisible Subset]]

## ❌ Mistakes
- Using `<=` instead of `<` for strictly increasing
- Not understanding patience sort — it doesn't track actual subsequence

## 🔗 Related Topics
[[Dynamic Programming]] | [[Binary Search]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #LIS #Algorithm
