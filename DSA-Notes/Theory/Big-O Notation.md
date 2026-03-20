# 📐 Big-O Notation

#Theory #Complexity #BigO

⬅️ [[Theory Index]] | ➡️ [[Time Complexity]]

---

## 📌 Definition

**Big-O Notation** describes the upper bound of an algorithm's growth rate. It characterizes performance in the **worst case** as input size n → ∞.

---

## ⚙️ Common Complexities (Best → Worst)

| Notation | Name | Example |
|----------|------|---------|
| O(1) | Constant | Array access, hash lookup |
| O(log n) | Logarithmic | Binary search, BST ops |
| O(n) | Linear | Linear scan, BFS/DFS |
| O(n log n) | Linearithmic | Merge sort, heap sort |
| O(n²) | Quadratic | Bubble sort, brute force pairs |
| O(n³) | Cubic | Floyd-Warshall |
| O(2ⁿ) | Exponential | Subsets, backtracking |
| O(n!) | Factorial | Permutations, TSP brute force |

---

## 🔥 Rules

1. **Drop constants**: O(2n) → O(n)
2. **Drop lower terms**: O(n² + n) → O(n²)
3. **Different variables**: O(n + m) stays as is
4. **Nested loops**: multiply, not add

---

## 🔗 Related Topics

- [[Time Complexity]]
- [[Space Complexity]]
- [[DSA-Notes/Theory/Amortized Analysis]]

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
