# Amortized Analysis

## 📌 Definition
A method of analysing the time complexity of a sequence of operations where occasional expensive operations are "paid for" by cheap ones — giving a more accurate average per-operation cost.

## ⚙️ How It Works
Three methods:
1. **Aggregate**: Total cost of n operations / n = amortized cost per op
2. **Accounting**: Assign "credits" to cheap ops to prepay for expensive ops
3. **Potential**: Use a potential function Φ to capture stored work

## 🔥 Classic Examples
| Data Structure | Operation | Amortized | Worst Case |
|----------------|-----------|-----------|------------|
| Dynamic Array | Push | O(1) | O(n) |
| Stack with multipop | Pop k | O(1) | O(n) |
| [[Disjoint Set Union]] | Find | O(α(n)) | O(log n) |
| Splay Tree | Any | O(log n) | O(n) |

## 🧠 When to Use
- Analysing dynamic arrays, stacks, queues with batch operations
- Understanding why O(n) operations don't always mean O(n) per step

## 🔗 Related Topics
[[Big-O Notation]] | [[Time Complexity]] | [[Disjoint Set Union]] | [[Dynamic Array]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #Theory #AmortizedAnalysis
