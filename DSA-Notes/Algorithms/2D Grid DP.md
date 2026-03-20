# 2D Grid DP

## 📌 Definition
Dynamic programming on a 2D grid where state is a cell (i, j) and transitions come from adjacent cells.

## ⚙️ How It Works
- `dp[i][j]` = answer at cell (i, j)
- Transitions: from top `(i-1,j)`, left `(i,j-1)`, or diagonal `(i-1,j-1)`
- Fill row by row, left to right
- Handle obstacles by skipping or setting to 0/∞

## 🧠 When to Use
- Counting paths in a grid
- Minimum/maximum cost paths
- Obstacle avoidance
- Maximal square/rectangle

## 🧪 Problems
- [[DSA Problem Bank#Unique Paths]]
- [[DSA Problem Bank#Unique Paths II]]
- [[DSA Problem Bank#Minimum Path Sum]]
- [[DSA Problem Bank#Triangle]]
- [[DSA Problem Bank#Maximal Square]]

## ❌ Mistakes
- Not initialising boundary cells (row 0 and col 0)
- Forgetting obstacle cells block propagation

## 🔗 Related Topics
[[Dynamic Programming]] | [[Array]] | [[Depth First Search]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #Grid #Algorithm
