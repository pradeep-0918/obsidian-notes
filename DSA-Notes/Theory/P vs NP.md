# P vs NP

## 📌 Definition
One of the most important unsolved problems in computer science: whether every problem whose solution can be **verified** in polynomial time (**NP**) can also be **solved** in polynomial time (**P**).

## 🧠 Key Concepts

**P (Polynomial Time)**
- Problems solvable in O(n^k) time for some constant k
- Examples: sorting, shortest path, spanning tree

**NP (Nondeterministic Polynomial)**
- Problems where a given solution can be **verified** in polynomial time
- Examples: 3-SAT, Travelling Salesman (decision), Subset Sum

**The Question: P = NP?**
- If yes: every problem we can check quickly, we can also solve quickly
- If no (widely believed): some problems are inherently hard to solve
- Unsolved since 1971 (Cook-Levin theorem)

## 🔥 Why It Matters
- Cryptography relies on P ≠ NP (hard to break, easy to verify)
- [[NP Complete]] problems: if any one is in P, all NP problems are in P
- Affects algorithm design strategy

## 🔗 Related Topics
[[NP Complete]] | [[NP Hard]] | [[Big-O Notation]] | [[Time Complexity]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Theory #Complexity #PvsNP
