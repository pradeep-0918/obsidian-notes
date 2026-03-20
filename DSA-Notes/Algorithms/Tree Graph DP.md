# Tree Graph DP

## 📌 Definition
DP computed on a tree structure where the state of a node depends on states of its children (bottom-up) or parent (re-rooting).

## ⚙️ How It Works
**Bottom-up (post-order DFS):**
- Compute children first, then combine at parent
- `dp[node] = f(dp[child1], dp[child2], ...)`

**Re-rooting technique:**
- First pass: bottom-up to compute subtree answers
- Second pass: top-down to propagate parent information

## 🧠 When to Use
- Tree diameter, max path sum
- Subtree size, independent set on tree
- House Robber on tree

## 🧪 Problems
- [[DSA Problem Bank#Binary Tree Maximum Path Sum]]
- [[DSA Problem Bank#House Robber III]]
- [[DSA Problem Bank#Diameter of Binary Tree]]
- [[DSA Problem Bank#Distribute Coins in Binary Tree]]

## ❌ Mistakes
- Combining max values from the same subtree twice (double counting)
- Forgetting that max path through a node uses at most two children

## 🔗 Related Topics
[[Dynamic Programming]] | [[Depth First Search]] | [[Tree]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #Tree #Algorithm
