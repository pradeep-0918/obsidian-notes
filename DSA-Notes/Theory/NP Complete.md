# NP-Complete

## 📌 Definition
A problem is **NP-Complete** if it is:
1. In **NP** (a solution can be verified in polynomial time)
2. **NP-Hard** (every problem in NP can be reduced to it in polynomial time)

NP-Complete problems are the "hardest" problems in NP.

## 🔥 Classic NP-Complete Problems
| Problem | Description |
|---------|-------------|
| 3-SAT | Satisfiability of 3-clause boolean formulas |
| Travelling Salesman (decision) | Is there a tour of cost ≤ k? |
| Subset Sum | Does any subset sum to target T? |
| Graph Coloring | Can graph be coloured with k colours? |
| Hamiltonian Path | Does a path visiting all vertices once exist? |
| Clique | Does a clique of size k exist? |

## 🧠 Implications
- No polynomial-time algorithm known (if P ≠ NP)
- Approach: approximation algorithms, heuristics, or exact exponential algorithms for small inputs
- Bitmask DP often used for small-n exact solutions

## 🔗 Related Topics
[[P vs NP]] | [[NP Hard]] | [[Bitmask DP]] | [[Backtracking]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Theory #Complexity #NPComplete
