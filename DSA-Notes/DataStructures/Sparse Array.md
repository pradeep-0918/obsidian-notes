# 📦 Sparse Array

#DataStructure #Linear #Advanced

⬅️ [[Dynamic Array]] | ➡️ [[String]]

---

## 📌 Definition

A **Sparse Array** is an array where most elements have the same default value (usually 0). Instead of storing all values, only non-default values are stored, typically in a hash map or list of (index, value) pairs.

---

## ⚙️ How It Works

```
Dense:  [0, 0, 0, 42, 0, 0, 0, 7, 0, 0]  → wastes 8 slots
Sparse: {3: 42, 7: 7}  → only non-zeros stored
```

---

## 🧠 When to Use

- Matrix with mostly zeros (e.g., adjacency matrix for sparse graph)
- Memory is constrained
- Very large index spaces (NLP embeddings, graph problems)

---

## 🔥 Key Properties

- Memory efficient when density < ~20%
- Access is O(1) for hash-based, O(log n) for sorted list
- Common in scientific computing, ML (scipy sparse matrices)

---

## ⏱ Complexity

| Operation | Hash Map Impl | Sorted List |
|-----------|--------------|-------------|
| Access | O(1) avg | O(log n) |
| Insert | O(1) avg | O(n) |
| Space | O(k) | O(k) |

*k = number of non-zero elements*

---

## 🆚 Comparison

| Feature | Dense Array | Sparse Array |
|---------|------------|-------------|
| Memory | O(n) always | O(k) non-zeros |
| Access | O(1) | O(1) hash |
| Iteration | O(n) | O(k) |
| Use case | Dense data | Mostly-zero data |

---

## 🔗 Related Topics

- [[Array]] — dense version
- [[Hash Map]] — common implementation
- [[Graph]] — sparse adjacency matrices

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
