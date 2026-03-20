# Digit DP

## 📌 Definition
DP over digits of a number, counting integers in a range [0, N] satisfying certain digit-based properties.

## ⚙️ How It Works
- State: `dp[pos][tight][...]` where `tight` = whether current prefix matches N's prefix
- Build number digit by digit from most significant
- Implemented via DFS with memoization
- At each position, try digits 0 to (tight ? digit[pos] : 9)

## 🧠 When to Use
- Count numbers with specific digit sum
- Count numbers with no repeated digits
- Count numbers satisfying digit pattern in range [L, R]

## 🧪 Problems
- [[DSA Problem Bank#Count Numbers with Unique Digits]]
- [[DSA Problem Bank#Non-negative Integers without Consecutive Ones]]

## ❌ Mistakes
- Forgetting the `tight` constraint (over-counts)
- Not handling leading zeros separately

## 🔗 Related Topics
[[Dynamic Programming]] | [[Recursion]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #Digit #Algorithm
