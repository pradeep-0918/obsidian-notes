# NP-Hard

## 📌 Definition
A problem is **NP-Hard** if every problem in NP can be reduced to it in polynomial time. NP-Hard problems are **at least as hard** as the hardest problems in NP.

## 🔥 Key Distinction
| Class | In NP? | At least as hard as NP? |
|-------|--------|------------------------|
| [[NP Complete]] | ✅ Yes | ✅ Yes |
| NP-Hard | ❌ Not necessarily | ✅ Yes |

## 🔥 Classic NP-Hard Problems
| Problem | Notes |
|---------|-------|
| Travelling Salesman (optimisation) | Find minimum tour (not just decision) |
| Halting Problem | Undecidable — not even in NP |
| Graph Coloring (optimisation) | Find minimum colours needed |
| Longest Path | Find longest simple path in graph |

## 🧠 Practical Approach
- Use approximation algorithms with guaranteed ratio
- Use heuristics (genetic algorithms, simulated annealing)
- Use exact algorithms for small inputs ([[Bitmask DP]], [[Backtracking]])

## 🔗 Related Topics
[[NP Complete]] | [[P vs NP]] | [[Bitmask DP]] | [[Backtracking]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Theory #Complexity #NPHard
