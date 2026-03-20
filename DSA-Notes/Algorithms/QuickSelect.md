# QuickSelect

## 📌 Definition
A selection algorithm that finds the k-th smallest (or largest) element in an unordered list in average O(n) time, using the partitioning technique from [[Quick Sort]].

## ⚙️ How It Works
1. Choose a pivot and partition the array
2. If pivot index == k, return pivot (found!)
3. If pivot index > k, recurse on left partition
4. If pivot index < k, recurse on right partition
5. Only recurse into ONE side (unlike Quick Sort)

## 🧠 When to Use
- Find k-th largest/smallest without full sort
- Median finding
- Top-K problems when O(n) is needed

## 🔥 Key Properties
- Average O(n), Worst O(n²) (avoidable with random pivot)
- In-place, modifies array
- Not stable

## ⏱ Complexity
| Case | Time |
|------|------|
| Average | O(n) |
| Worst | O(n²) |
| Space | O(1) |

## 🆚 Comparison
| Approach | Time | Use Case |
|---------|------|----------|
| QuickSelect | O(n) avg | k-th element |
| Sort + index | O(n log n) | Full sort needed |
| [[Heap]] (Top-K) | O(n log k) | Streaming data |

## 🧪 Problems
- [[DSA Problem Bank#Kth Largest Element in an Array]]
- [[DSA Problem Bank#K Closest Points to Origin]]
- [[DSA Problem Bank#Top K Frequent Elements]]

## ❌ Mistakes
- Not randomising pivot (worst case O(n²) on sorted input)
- Confusing k-th smallest vs k-th largest index

## 🔗 Related Topics
[[Quick Sort]] | [[Heap]] | [[Priority Queue]] | [[Divide and Conquer]]

## ⬅️ Previous
[[Quick Sort]]

## ➡️ Next
[[Merge Sort]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #QuickSelect #Algorithm #Selection
