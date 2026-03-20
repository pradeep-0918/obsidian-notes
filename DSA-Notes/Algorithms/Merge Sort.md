# Merge Sort

## 📌 Definition
A stable, comparison-based sorting algorithm that uses [[Divide and Conquer]] to recursively split arrays into halves, sort each half, and merge them back in sorted order.

## ⚙️ How It Works
1. Divide array into two halves
2. Recursively sort each half
3. Merge the two sorted halves using a two-pointer technique
4. Base case: array of size 0 or 1 is already sorted

## 🧠 When to Use
- Stable sort required
- Sorting linked lists (no random access needed)
- External sorting (large data sets on disk)
- Counting inversions

## 🔥 Key Properties
- Stable sort (equal elements maintain relative order)
- Not in-place (requires O(n) extra space)
- Guaranteed O(n log n) — no worst-case degradation

## ⏱ Complexity
| Case | Time | Space |
|------|------|-------|
| Best | O(n log n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n log n) | O(n) |

## 🆚 Comparison
| Algorithm | Stable | In-Place | Average |
|-----------|--------|----------|---------|
| Merge Sort | ✅ | ❌ | O(n log n) |
| Quick Sort | ❌ | ✅ | O(n log n) |
| Heap Sort | ❌ | ✅ | O(n log n) |

## 🧪 Problems
- [[DSA Problem Bank#Sort List]]
- [[DSA Problem Bank#Reverse Pairs]]
- [[DSA Problem Bank#Count of Smaller Numbers After Self]]

## ❌ Mistakes
- Not handling odd-length arrays in split
- Forgetting to copy merged result back to original array
- Using merge sort when in-place sort is required

## 🔗 Related Topics
[[Divide and Conquer]] | [[Quick Sort]] | [[Sorting]] | [[Recursion]]

## ⬅️ Previous
[[Sorting]]

## ➡️ Next
[[Quick Sort]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Sorting #MergeSort #Algorithm
