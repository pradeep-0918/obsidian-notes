# Divide and Conquer

## 📌 Definition
An algorithm design paradigm that solves a problem by recursively breaking it into smaller subproblems, solving each independently, and combining the results.

## ⚙️ How It Works
Three steps:
1. **Divide**: Split problem into smaller subproblems of the same type
2. **Conquer**: Solve subproblems recursively (base case: solve directly)
3. **Combine**: Merge subproblem solutions into the overall solution

## 🧠 When to Use
- Sorting (Merge Sort, Quick Sort)
- Searching (Binary Search)
- Matrix multiplication (Strassen)
- Closest pair of points
- Tree construction from traversals

## 🔥 Key Properties
- Subproblems are independent (unlike DP where they overlap)
- Recursion tree analysis helps find complexity
- Often parallelizable

## ⏱ Complexity
Use Master Theorem: T(n) = aT(n/b) + f(n)
- a=2, b=2, f(n)=O(n) → O(n log n) (Merge Sort)
- a=1, b=2, f(n)=O(1) → O(log n) (Binary Search)

## 🆚 Comparison
| Paradigm | Subproblems | Overlap | Example |
|----------|------------|---------|---------|
| Divide & Conquer | Independent | No | Merge Sort |
| [[Dynamic Programming]] | Overlap | Yes | Fibonacci |
| [[Greedy Algorithms]] | No recursion | — | Interval scheduling |

## 🧪 Problems
- [[DSA Problem Bank#Convert Sorted List to Binary Search Tree]]
- [[DSA Problem Bank#Construct Quad Tree]]
- [[DSA Problem Bank#Maximum Binary Tree]]
- [[DSA Problem Bank#Median of Two Sorted Arrays]]

## ❌ Mistakes
- Using D&C when subproblems overlap (use DP instead)
- Incorrect combine step
- Not identifying the right subproblem structure

## 🔗 Related Topics
[[Recursion]] | [[Dynamic Programming]] | [[Merge Sort]] | [[Quick Sort]] | [[Binary Search]]

## ⬅️ Previous
[[Recursion]]

## ➡️ Next
[[Sorting]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #DivideAndConquer #Algorithm
