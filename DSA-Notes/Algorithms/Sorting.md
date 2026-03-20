# Sorting

## 📌 Definition
The process of arranging elements in a defined order (ascending or descending). Sorting is fundamental to computer science and underpins many other algorithms.

## ⚙️ How It Works
Different algorithms use different strategies:
- **Comparison-based**: Compare pairs of elements (Merge Sort, Quick Sort, Heap Sort)
- **Non-comparison**: Exploit structure of data (Counting Sort, Radix Sort, [[Bucket Sort]])
- **In-place**: Sort without extra memory
- **Stable**: Preserve relative order of equal elements

## 🧠 When to Use
- Prerequisite for Binary Search
- Grouping and deduplication
- Interval problems (sort by start/end)
- Greedy algorithms (sort first, then greedy)

## 🔥 Key Properties
- Lower bound for comparison sort: O(n log n)
- Non-comparison sorts can achieve O(n)
- Stability matters when secondary sort key needed

## ⏱ Complexity
| Algorithm | Best | Average | Worst | Stable |
|-----------|------|---------|-------|--------|
| [[Merge Sort]] | O(n log n) | O(n log n) | O(n log n) | ✅ |
| [[Quick Sort]] | O(n log n) | O(n log n) | O(n²) | ❌ |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | ❌ |
| Insertion Sort | O(n) | O(n²) | O(n²) | ✅ |
| [[Bucket Sort]] | O(n+k) | O(n+k) | O(n²) | ✅ |

## 🧪 Problems
- [[DSA Problem Bank#Sort Colors]]
- [[DSA Problem Bank#Sort List]]
- [[DSA Problem Bank#Merge Intervals]]
- [[DSA Problem Bank#Kth Largest Element in an Array]]

## ❌ Mistakes
- Using unstable sort when stability required
- Not considering data distribution (use counting sort for small ranges)

## 🔗 Related Topics
[[Merge Sort]] | [[Quick Sort]] | [[Bucket Sort]] | [[Binary Search]] | [[Greedy Algorithms]]

## ⬅️ Previous
[[Divide and Conquer]]

## ➡️ Next
[[Merge Sort]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Sorting #Algorithm
