# 💾 Space Complexity

#Theory #Complexity

⬅️ [[Time Complexity]] | ➡️ [[DSA-Notes/Theory/Amortized Analysis]]

---

## 📌 Definition

**Space Complexity** measures the memory used by an algorithm as input size n grows.

---

## 📊 Common Space Complexities

| Complexity | Example |
|------------|---------|
| O(1) | Two pointers, in-place sort |
| O(log n) | DFS recursive stack on balanced tree |
| O(n) | Storing all elements, BFS queue |
| O(n²) | 2D DP table, adjacency matrix |
| O(n·m) | Trie with n words of length m |

---

## 🔥 Space Optimization Tips

- **Tabulation → Rolling array**: Replace O(n) dp with O(1)
- **Iterative DFS**: Replace O(h) call stack with explicit stack
- **In-place algorithms**: Sort/reverse without extra space
- **Bit manipulation**: Pack multiple booleans into int

---

## ❌ Common Mistakes

- Forgetting **call stack** space in recursion (O(h) for tree DFS)
- Not counting **auxiliary** space (temp arrays in merge sort)
- Assuming in-place = O(1) (recursive in-place still uses O(log n) stack)

---

## 🔗 Related Topics

- [[Time Complexity]]
- [[Big-O Notation]]
- [[Dynamic Programming]] — space optimization

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
