# K-Way Merge

## 📌 Definition
A technique for merging K sorted arrays (or lists) into a single sorted output using a **min-heap** to efficiently determine which element comes next.

## ⚙️ How It Works
1. Insert the first element of each of the K arrays into a min-heap
2. Extract the minimum element; add it to the output
3. Insert the next element from the same array the minimum came from
4. Repeat until the heap is empty

## 🧠 When to Use
- Merging K sorted arrays/lists
- External sorting (merging sorted chunks from disk)
- Smallest range covering elements from K lists
- K sorted streams

## 🔥 Key Properties
- Heap always holds exactly one element per active list
- Naturally handles lists of different lengths
- Generalization of 2-way merge from Merge Sort

## ⏱ Complexity
| | Time | Space |
|-|------|-------|
| | O(n log k) | O(k) |
| n = total elements, k = number of lists |

## 🆚 Comparison
| Approach | Time | Notes |
|---------|------|-------|
| Naive (merge two at a time) | O(n·k) | Simple but slow |
| K-Way Merge (heap) | O(n log k) | Optimal |

## 🧪 Problems
- [[DSA Problem Bank#Merge K Sorted Lists]]
- [[DSA Problem Bank#Kth Smallest Element in a Sorted Matrix]]
- [[DSA Problem Bank#Smallest Range Covering Elements from K Lists]]
- [[DSA Problem Bank#Find K Pairs with Smallest Sums]]

## ❌ Mistakes
- Not tracking which list each heap element came from
- Not handling empty lists

## 🔗 Related Topics
[[Priority Queue]] | [[Min Heap]] | [[Merge Sort]] | [[Linked List]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #KWayMerge #Heap #Algorithm
