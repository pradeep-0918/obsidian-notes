# Bucket Sort

## 📌 Definition
A distribution-based sorting algorithm that divides elements into a number of buckets, sorts each bucket individually, and concatenates the results.

## ⚙️ How It Works
1. Determine range of input values
2. Create k buckets covering equal sub-ranges
3. Distribute each element into appropriate bucket
4. Sort each bucket (insertion sort or recursively)
5. Concatenate all buckets in order

## 🧠 When to Use
- Uniformly distributed floating-point numbers
- Top-K frequency problems (frequency buckets)
- When O(n) sort is achievable
- Grouping elements by frequency

## 🔥 Key Properties
- Stable when using stable inner sort
- Best with uniform distribution
- Excellent for "sort by frequency" patterns

## ⏱ Complexity
| Case | Time | Space |
|------|------|-------|
| Best | O(n+k) | O(n+k) |
| Average | O(n+k) | O(n+k) |
| Worst | O(n²) | O(n+k) |

## 🧪 Problems
- [[DSA Problem Bank#Sort Characters By Frequency]]
- [[DSA Problem Bank#Top K Frequent Words]]
- [[DSA Problem Bank#Maximum Gap]]
- [[DSA Problem Bank#Top K Frequent Elements]]

## ❌ Mistakes
- Bad bucket count choice (too few → uneven; too many → overhead)
- Using when distribution is skewed

## 🔗 Related Topics
[[Sorting]] | [[Hash Map]] | [[Quick Sort]] | [[Merge Sort]]

## ⬅️ Previous
[[Sorting]]

## ➡️ Next
[[Binary Search]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #Sorting #BucketSort #Algorithm
