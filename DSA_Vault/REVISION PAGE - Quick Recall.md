# 🔁 REVISION PAGE — Quick Recall Everything
> *The night-before-interview page | [[🏠 HOME - DSA Master Index]] | [[⚡ TRICKS & PATTERNS CHEATSHEET]]*

---

## 🎯 ONE-LINE SUMMARIES (Read in 3 minutes)

### 🔵 Two Pointers
| Problem | One-Line Key Idea |
|---------|-------------------|
| Two Sum II | Sorted → Left & right pointers, move based on sum vs target |
| Sort Colors | Dutch Flag: lo/mid/hi — swap 0s left, 2s right, 1s middle |
| 3Sum | Sort + fix i + two pointer on [i+1, n-1], skip duplicates |
| Rearrange Alternately | Encode two values in one cell using max_elem trick |
| Min Swaps to Sort | Count cycles: swaps = Σ(cycle_length - 1) |

### 🟢 Hashing & Prefix Sum
| Problem | One-Line Key Idea |
|---------|-------------------|
| Subarray Sum = K | `seen[prefix - k]` counts valid subarrays |
| Top K Frequent | Bucket sort by frequency → O(n) |
| Majority Element (n/2) | Boyer-Moore: cancel out votes, survivor wins |
| Majority Element (n/3) | Two-candidate Boyer-Moore, verify at end |
| Longest Consecutive | HashSet + only start from sequence beginning |

### 🟡 Binary Search
| Problem | One-Line Key Idea |
|---------|-------------------|
| Find Peak Element | If arr[mid] > arr[mid+1]: go left, else go right |
| Search Rotated Array | One half always sorted → check which, then narrow |
| First & Last Position | Two binary searches: keep going left / keep going right |
| Min in Rotated II | Duplicates? If arr[mid]==arr[right], shrink right by 1 |
| Split Array Largest Sum | Binary search on answer: can we split with max ≤ X? |
| Maximum Gap | Bucket sort with size = (max-min)/(n-1), max gap between buckets |

### 🔴 Sliding Window & Stack
| Problem | One-Line Key Idea |
|---------|-------------------|
| Sliding Window Max | Monotonic decreasing deque: front = current max |
| Largest Rectangle | Monotonic increasing stack: pop when shorter bar found |
| Trapping Rain Water | Two pointers: water = min(left_max, right_max) - height |

### 🟣 Advanced
| Problem | One-Line Key Idea |
|---------|-------------------|
| First Missing Positive | Put each number at its correct index, find mismatch |
| Median Two Sorted | Binary search partition: max_left ≤ min_right |
| Count Inversions | Merge sort: count right-before-left elements during merge |
| Reverse Pairs | Merge sort: count pairs before merging (two pointer) |
| Kadane's | curr = max(num, curr+num); max_sum = max(max_sum, curr) |
| Stock Buy Sell | Track min seen so far, update max profit each step |

---

## 📊 Algorithm Speed Comparison

```
O(1)        → Direct index, HashMap lookup
O(log n)    → Binary Search
O(n)        → Two Pointers, Sliding Window, Kadane's, Boyer-Moore
O(n log n)  → Sort-based, Merge Sort, Heap
O(n²)       → Brute force pairs (avoid!)
O(n³)       → Brute force triplets (avoid!)
```

---

## 🔥 Most Common Interview Patterns (Ranked)

```
Rank 1: 🏆 PREFIX SUM + HASHMAP
  → Subarray Sum K, Longest Subarray with sum K

Rank 2: 🥈 TWO POINTERS
  → Two Sum, 3Sum, Container with Most Water

Rank 3: 🥉 SLIDING WINDOW  
  → Max in window, Minimum window substring

Rank 4: BINARY SEARCH ON ANSWER
  → Split array, Minimize max, etc.

Rank 5: MONOTONIC STACK
  → Next greater, Histogram, Rain water

Rank 6: MODIFIED MERGE SORT
  → Inversions, Reverse pairs
```

---

## ⚠️ CRITICAL GOTCHAS (Don't forget these!)

```
1. Binary Search: Use mid = left + (right-left)//2, NOT (left+right)//2

2. Two Pointers for 3Sum: Skip duplicates AFTER finding a valid triplet
   while left < right and nums[left] == nums[left+1]: left += 1

3. Dutch National Flag: DON'T increment mid after swapping with high!
   (New element at mid is unprocessed)

4. Prefix Sum init: Always start with {0: 1} in hashmap!
   (Handles subarrays starting from index 0)

5. Monotonic Deque: Remove from FRONT if out of window,
   Remove from BACK if smaller than current

6. Kadane's: Initialize with nums[0], not 0
   (Handles all-negative arrays)

7. First Missing Positive: Answer is always in [1, n+1]
   (Use array itself as hash table)

8. Boyer-Moore: Always VERIFY the candidate for Majority Element II
   (Two candidates need verification pass)
```

---

## 🧪 Quick Complexity Audit Before Submitting

```
Before submitting, ask:
❓ Is there a HashSet/HashMap that could reduce O(n²) to O(n)?
❓ Is the array sorted? Could two pointers replace nested loops?
❓ Am I recalculating subarray sums? → Use prefix sum!
❓ Can I binary search on the answer instead of brute force?
❓ Is the "maximum in window" problem? → Monotonic deque!
```

---

## 📚 Study Resources

| Resource | Best For |
|----------|----------|
| LeetCode | Practice + discussion |
| NeetCode.io | Video explanations |
| TakeUForward (Striver) | Structured Indian interviews |
| CLRS (Introduction to Algorithms) | Deep theory |
| Competitive Programming 3 | Advanced tricks |
| Programming Pearls (Bentley) | Problem-solving intuition |
| Cracking the Coding Interview | Interview prep |

---

## 🔗 All Notes

- [[01 - Two Pointers]]
- [[02 - Hashing & Prefix Sum]]
- [[03 - Binary Search]]
- [[04 - Sliding Window & Stack]]
- [[05 - Advanced Array Problems]]
- [[⚡ TRICKS & PATTERNS CHEATSHEET]]
- [[🏠 HOME - DSA Master Index]]

---
*Tags: #Revision #Summary #Interview #DSA #CheatSheet #QuickRecall*
