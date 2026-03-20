# Quick Sort

## 📌 Definition
An efficient, in-place, comparison-based sorting algorithm that uses [[Divide and Conquer]] by selecting a pivot element and partitioning the array around it.

## ⚙️ How It Works
1. Choose a pivot (first, last, random, or median-of-three)
2. Partition: move elements < pivot to left, > pivot to right
3. Recursively sort left and right partitions
4. Base case: partition of size 0 or 1

## 🧠 When to Use
- General-purpose in-memory sorting
- When average-case performance matters more than worst-case
- Cache-friendly operations (good locality of reference)

## 🔥 Key Properties
- In-place (O(log n) stack space)
- Not stable in standard form
- Worst case avoidable with random pivot selection

## ⏱ Complexity
| Case | Time | Space |
|------|------|-------|
| Best | O(n log n) | O(log n) |
| Average | O(n log n) | O(log n) |
| Worst | O(n²) | O(n) |

## 🆚 Comparison
| Algorithm | Stable | In-Place | Worst |
|-----------|--------|----------|-------|
| Quick Sort | ❌ | ✅ | O(n²) |
| [[Merge Sort]] | ✅ | ❌ | O(n log n) |

## 🧪 Problems
- [[DSA Problem Bank#Sort Colors]]
- [[DSA Problem Bank#Kth Largest Element in an Array]]

## ❌ Mistakes
- Always choosing first/last as pivot on sorted arrays → O(n²)
- Not handling duplicates (use 3-way partition)

## 🔗 Related Topics
[[QuickSelect]] | [[Merge Sort]] | [[Divide and Conquer]] | [[Sorting]]

## ⬅️ Previous
[[Merge Sort]]

## ➡️ Next
[[QuickSelect]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Sorting #QuickSort #Algorithm
