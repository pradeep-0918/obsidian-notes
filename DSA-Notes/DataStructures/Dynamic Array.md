# 📦 Dynamic Array

#DataStructure #Linear #Array

⬅️ [[Array]] | ➡️ [[Sparse Array]]

---

## 📌 Definition

A **Dynamic Array** (e.g., Python list, Java ArrayList, C++ vector) is an array that resizes automatically when it runs out of capacity. It achieves amortized O(1) append by doubling in size.

---

## ⚙️ How It Works

1. Start with capacity = some default (e.g., 4)
2. When full, allocate 2× capacity, copy elements
3. Amortized cost of n appends = O(n) total → O(1) per append

```
Capacity: 4   → [_, _, _, _]
Add 1-4:      → [1, 2, 3, 4]
Add 5: RESIZE → [1, 2, 3, 4, 5, _, _, _]  (cap=8)
```

---

## 🧠 When to Use

- Need array with unknown/growing size
- Frequent append to end
- Random access still needed

---

## 🔥 Key Properties

- **Amortized O(1)** append
- Growth factor: usually 2× (doubles capacity)
- Underlying static array + pointer to last element
- Python `list`, Java `ArrayList`, C++ `vector`

---

## ⏱ Complexity

| Operation | Average | Worst |
|-----------|---------|-------|
| Access | O(1) | O(1) |
| Append | O(1) amortized | O(n) resize |
| Insert middle | O(n) | O(n) |
| Delete middle | O(n) | O(n) |
| Space | O(n) | O(n) |

---

## 🆚 Comparison

| Feature | Static Array | Dynamic Array | [[Linked List]] |
|---------|-------------|--------------|-------------|
| Size | Fixed | Grows | Grows |
| Append | O(1) | O(1) amort | O(1) |
| Random Access | O(1) | O(1) | O(n) |
| Memory | Tight | Extra capacity | Extra pointers |

---

## 🧪 Problems

Most [[Array]] problems apply here. Dynamic arrays underlie most array-based patterns.

---

## ❌ Common Mistakes

- Forgetting resize is O(n) worst case
- Unnecessary copies when passing to functions

---

## 🔗 Related Topics

- [[Array]] — static version
- [[DSA-Notes/Theory/Amortized Analysis]] — why append is O(1)
- [[Stack]] — commonly built on dynamic array

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
