# Amortized Analysis

## 📌 Definition
A method for analysing the average per-operation cost of a sequence of operations, accounting for the fact that some operations are occasionally expensive but that cost is spread over many cheap ones.

## ⚙️ Three Methods
**1. Aggregate Method**
- Find total cost T(n) for n operations
- Amortized cost = T(n)/n

**2. Accounting Method**
- Assign "credits" to cheap operations
- Credits pay for future expensive operations
- Ensure credit never goes negative

**3. Potential Method**
- Define potential function Φ(state)
- Amortized cost = actual cost + ΔΦ

## 🔥 Classic Examples
| Structure | Operation | Worst | Amortized |
|-----------|-----------|-------|-----------|
| Dynamic Array | append | O(n) | O(1) |
| Stack with multipop | pop k | O(n) | O(1) |
| [[Disjoint Set Union]] | find | O(log n) | O(α(n)) |

## 🧠 When to Use
- Dynamic arrays (doubling strategy)
- Understanding "expensive but rare" operations
- Cache with LRU eviction

## 🔗 Related Topics
[[Big-O Notation]] | [[Time Complexity]] | [[Dynamic Array]] | [[Disjoint Set Union]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Theory #AmortizedAnalysis
