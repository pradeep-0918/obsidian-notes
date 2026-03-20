# Probability DP

## 📌 Definition
DP computing expected values or probabilities of outcomes over sequential random events.

## ⚙️ How It Works
- State: `dp[i][j]` = probability or expected value at step i with parameter j
- Transitions weight sub-outcomes by their probabilities
- Base case: certain/known initial state
- Use floating point arithmetic carefully

## 🧠 When to Use
- Dice roll counting problems
- Random walk expected values
- Game probability calculations

## 🧪 Problems
- [[DSA Problem Bank#New 21 Game]]
- [[DSA Problem Bank#Knight Probability in Chessboard]]
- [[DSA Problem Bank#Soup Servings]]

## ❌ Mistakes
- Floating-point precision errors (use epsilon comparisons)
- Off-by-one in probability normalisation

## 🔗 Related Topics
[[Dynamic Programming]] | [[Math]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #Probability #Algorithm
