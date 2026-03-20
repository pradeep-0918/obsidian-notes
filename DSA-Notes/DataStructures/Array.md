# 📦 Array
#DataStructure #Linear #Array

⬅️ [[Data Structures Index]] | ➡️ [[Dynamic Array]]

---

## 📌 Definition
An array is a **contiguous block of memory** storing elements of the same type, accessible via an integer index. It is the most fundamental data structure.

---

## ⚙️ How It Works
- Elements stored at consecutive memory addresses
- Index `i` maps to address: `base + i * element_size`
- Random access in O(1) due to direct address computation

```
Index:  [0]  [1]  [2]  [3]  [4]
Value:  [12] [45] [7]  [89] [3]
Addr:   100  104  108  112  116
```

---

## 🧠 When to Use
- Need O(1) random access by index
- Size is known in advance
- Cache-friendly iteration is important
- Store and process ordered data

---

## 🔥 Key Properties
- **Fixed size** (unlike [[Dynamic Array]])
- **Cache-friendly** — elements in contiguous memory
- **Index-based** — O(1) access
- **Homogeneous** — all elements same type

---

## ⏱ Complexity

| Operation | Time | Space |
|---|---|---|
| Access | O(1) | — |
| Search | O(n) | — |
| Insert (end) | O(1) | — |
| Insert (middle) | O(n) | — |
| Delete | O(n) | — |
| **Space** | — | O(n) |

---

## 🆚 Comparison

| Feature | Array | [[Linked List]] | [[Hash Map]] |
|---|---|---|---|
| Access | O(1) | O(n) | O(1) avg |
| Insert (mid) | O(n) | O(1) | O(1) avg |
| Cache | ✅ | ❌ | ❌ |
| Fixed size | ✅ | ❌ | ❌ |

---

## 🧪 Problems
- [ ] [Move Zeroes](https://leetcode.com/problems/move-zeroes/) — Easy
- [ ] [Majority Element](https://leetcode.com/problems/majority-element/) — Easy
- [ ] [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) — Easy
- [ ] [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/) — Easy
- [ ] [Rotate Array](https://leetcode.com/problems/rotate-array/) — Medium
- [ ] [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/) — Medium
- [ ] [First Missing Positive](https://leetcode.com/problems/first-missing-positive/) — Hard
- [ ] [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) — Medium
- [ ] [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) — Hard
- [ ] [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/) — Hard

---

## ❌ Common Mistakes
- Off-by-one errors in index loops
- Not handling empty arrays
- Forgetting to handle single-element arrays
- Using array when data size is dynamic (use [[Dynamic Array]])

---

## 🔗 Related Topics
- [[Dynamic Array]] — Resizable version
- [[Two Pointers]] — Classic array technique
- [[Sliding Window]] — Window over array
- [[Prefix Sum]] — Cumulative sums
- [[Binary Search]] — Search on sorted array
- [[Sorting Algorithms]] — Ordering array elements
- [[Kadane's Algorithm]] — Maximum subarray

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
