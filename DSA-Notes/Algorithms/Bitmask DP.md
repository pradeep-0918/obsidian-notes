# Bitmask DP

## 📌 Definition
DP where state includes a bitmask representing a subset of elements. Enables exponential-state problems to be solved in O(2^n · n).

## ⚙️ How It Works
- State: `dp[mask][i]` = answer when visited set is `mask` and currently at node/item i
- Transition: extend mask by setting next unvisited bit: `next_mask = mask | (1 << j)`
- Base: `dp[1<<start][start] = 0`
- Answer: check all full masks `(1<<n)-1`

## 🧠 When to Use
- Travelling Salesman Problem (TSP)
- Assignment problems
- Set covering with minimum cost
- Problems with up to 20 elements

## 🧪 Problems
- [[DSA Problem Bank#Partition to K Equal Sum Subsets]]
- [[DSA Problem Bank#Smallest Sufficient Team]]
- [[DSA Problem Bank#Minimum Number of Work Sessions to Finish the Tasks]]

## ❌ Mistakes
- Bit indexing errors (off-by-one in mask shifts)
- Not precomputing compatible masks

## 🔗 Related Topics
[[Bit Manipulation]] | [[Dynamic Programming]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #Bitmask #Algorithm
