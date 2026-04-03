# 🟡 Binary Search — Complete Guide
> *Back to [[🏠 HOME - DSA Master Index]] | Prev: [[02 - Hashing & Prefix Sum]] | Next: [[04 - Sliding Window & Stack]]*

---

## 🎓 What IS Binary Search? (Feynman Explanation)

> You're looking for a word in a dictionary. Do you check page 1, then 2, then 3...?
> **No!** You open the MIDDLE. If your word comes before → look in left half. After → look in right half.
> Each step you eliminate **HALF** the book. 1000 pages → 10 steps. That's Binary Search!

```
Linear Search:  O(n)  → 1,000,000 elements → 1,000,000 steps
Binary Search:  O(log n) → 1,000,000 elements → only 20 steps! 🚀
```

---

## 🔑 The Binary Search Template (MASTER THIS)

```python
# Template 1: Basic (find exact value)
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:          # ← note: <=
        mid = left + (right - left) // 2  # ← avoids integer overflow
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1

# Template 2: Find LEFTMOST occurrence
def lower_bound(arr, target):
    left, right = 0, len(arr)    # ← right = len(arr) !
    
    while left < right:          # ← note: strictly <
        mid = left + (right - left) // 2
        if arr[mid] < target:
            left = mid + 1
        else:
            right = mid          # ← NOT mid-1
    
    return left

# Template 3: Find RIGHTMOST occurrence
def upper_bound(arr, target):
    left, right = 0, len(arr)
    
    while left < right:
        mid = left + (right - left) // 2
        if arr[mid] <= target:   # ← note: <=
            left = mid + 1
        else:
            right = mid
    
    return left - 1
```

### ⚠️ Common Mistakes
```
❌ mid = (left + right) / 2  → Integer overflow for large values!
✅ mid = left + (right - left) // 2  → Safe always

❌ while left < right with right = len-1 → Can miss last element
✅ Match template carefully (see above)
```

---

## 🔵 Problem 1: Find Peak Element
> 🔗 https://leetcode.com/problems/find-peak-element/

### 🧒 Explain Like I'm 6
Find a mountain top — a number bigger than both its neighbors. Binary search by checking which side is going "uphill"!

### 🎯 Key Insight
```
If arr[mid] > arr[mid+1]:
    Peak is in LEFT half (including mid)
Else:
    Peak is in RIGHT half (excluding mid)
```

```python
def findPeakElement(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        if nums[mid] > nums[mid + 1]:
            right = mid        # peak is to the left (including mid)
        else:
            left = mid + 1    # peak is to the right
    
    return left  # left == right → peak found
```

### ⚡ Time: O(log n), Space: O(1)

---

## 🟢 Problem 2: Search in Rotated Sorted Array ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/search-in-rotated-sorted-array/

### 🧒 Explain Like I'm 6
Imagine a sorted list `[1,2,3,4,5,6,7]` rotated to `[4,5,6,7,1,2,3]`. One half is ALWAYS sorted!

### 🎯 The Trick: One Half is Always Sorted
```python
def search(nums, target):
    left, right = 0, len(nums) - 1
    
    while left <= right:
        mid = left + (right - left) // 2
        
        if nums[mid] == target:
            return mid
        
        # Check which half is sorted
        if nums[left] <= nums[mid]:  # LEFT half is sorted
            if nums[left] <= target < nums[mid]:
                right = mid - 1      # target in left half
            else:
                left = mid + 1       # target in right half
        else:                        # RIGHT half is sorted
            if nums[mid] < target <= nums[right]:
                left = mid + 1       # target in right half
            else:
                right = mid - 1      # target in left half
    
    return -1
```

### 🧠 Visual
```
[4, 5, 6, 7, 0, 1, 2], target = 0

mid=7 (index 3)
nums[left]=4 <= nums[mid]=7 → left half [4,5,6,7] sorted
target=0, NOT in [4,7) → go right → left=4

mid=1 (index 5)
nums[left]=0 > nums[mid]=1 → right half [1,2] sorted
target=0, NOT in (1,2] → go left → right=4

mid=0 (index 4)
nums[mid] == 0 → return 4 ✅
```

---

## 🔴 Problem 3: Find First and Last Position ⭐⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/

### 🎯 Use Lower Bound and Upper Bound Templates

```python
def searchRange(nums, target):
    def find_first(target):
        left, right = 0, len(nums) - 1
        result = -1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                result = mid
                right = mid - 1    # ← Keep looking LEFT for first
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return result
    
    def find_last(target):
        left, right = 0, len(nums) - 1
        result = -1
        while left <= right:
            mid = left + (right - left) // 2
            if nums[mid] == target:
                result = mid
                left = mid + 1     # ← Keep looking RIGHT for last
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
        return result
    
    return [find_first(target), find_last(target)]
```

### ⚡ Key Difference Between First and Last
```
Finding FIRST: when found, go LEFT  (right = mid - 1)
Finding LAST:  when found, go RIGHT (left = mid + 1)
```

---

## 🟡 Problem 4: Find Minimum in Rotated Sorted Array II
> 🔗 https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/

```python
def findMin(nums):
    left, right = 0, len(nums) - 1
    
    while left < right:
        mid = left + (right - left) // 2
        
        if nums[mid] > nums[right]:
            left = mid + 1         # min in right half
        elif nums[mid] < nums[right]:
            right = mid            # min in left half (including mid)
        else:
            right -= 1             # Can't determine → shrink right (handles duplicates)
    
    return nums[left]
```

### ⚠️ II vs I Difference
```
Without duplicates: if arr[mid] == arr[right] → impossible (they're different)
With duplicates:    if arr[mid] == arr[right] → can't determine which side
                    → safely shrink right by 1
```

---

## 🟣 Problem 5: Split Array Largest Sum ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/split-array-largest-sum/

### 🧒 Explain Like I'm 6
Split an array into k parts. Minimize the maximum sum of any part.

### 🎯 Binary Search on ANSWER (Not on Array!)
```
KEY INSIGHT: Binary search on the ANSWER (the minimum possible max sum)

Lower bound: max(nums) — at minimum, largest element is in one subarray
Upper bound: sum(nums) — entire array in one subarray

Binary search: can we split into k parts with max sum ≤ mid?
```

```python
def splitArray(nums, k):
    def can_split(max_sum):
        parts = 1
        curr_sum = 0
        for num in nums:
            if curr_sum + num > max_sum:
                parts += 1    # Start new part
                curr_sum = num
                if parts > k:
                    return False
            else:
                curr_sum += num
        return True
    
    left = max(nums)
    right = sum(nums)
    
    while left < right:
        mid = left + (right - left) // 2
        if can_split(mid):
            right = mid        # Try smaller max_sum
        else:
            left = mid + 1    # Need larger max_sum
    
    return left
```

### 🧠 "Binary Search on Answer" Pattern
```
Use when:
  ✅ You can check "is X a valid answer?" in O(n)
  ✅ The answer space is monotone (if X works, X+1 also works)
  ✅ You want to minimize/maximize some value

Examples:
  - Split Array Largest Sum
  - Koko Eating Bananas
  - Minimum Days to Make M Bouquets
  - Capacity to Ship Packages
```

---

## 🟠 Problem 6: Maximum Gap
> 🔗 https://leetcode.com/problems/maximum-gap/

### 🎯 Bucket Sort / Radix Sort Approach O(n)
```
Key Insight: If we sort, max gap ≥ (max-min)/(n-1) (pigeonhole principle)
→ Create buckets of size ≥ ceil((max-min)/(n-1))
→ Max gap MUST be between elements in DIFFERENT buckets
→ Only track min and max of each bucket
```

```python
def maximumGap(nums):
    if len(nums) < 2: return 0
    
    min_val, max_val = min(nums), max(nums)
    if min_val == max_val: return 0
    
    n = len(nums)
    bucket_size = max(1, (max_val - min_val) // (n - 1))
    bucket_count = (max_val - min_val) // bucket_size + 1
    
    buckets = [[float('inf'), float('-inf')] for _ in range(bucket_count)]
    
    for num in nums:
        idx = (num - min_val) // bucket_size
        buckets[idx][0] = min(buckets[idx][0], num)
        buckets[idx][1] = max(buckets[idx][1], num)
    
    max_gap = 0
    prev_max = min_val
    
    for b_min, b_max in buckets:
        if b_min == float('inf'): continue  # Empty bucket
        max_gap = max(max_gap, b_min - prev_max)
        prev_max = b_max
    
    return max_gap
```

---

## 🎯 Binary Search Pattern Recognition

```
┌───────────────────────────────────────────────────────────────┐
│  WHEN TO USE BINARY SEARCH                                    │
├───────────────────────────────────────────────────────────────┤
│  ✅ Sorted array (or can be treated as sorted)               │
│  ✅ "Find minimum X such that condition is satisfied"         │
│  ✅ Rotated sorted array → one half always sorted            │
│  ✅ Problem says O(log n) time complexity required           │
│  ✅ Search in infinite / very large search space             │
└───────────────────────────────────────────────────────────────┘

Binary Search on Value vs Binary Search on Answer:
┌─────────────────────────┬─────────────────────────────────┐
│ Search on Array Value   │ Search on Answer Space          │
├─────────────────────────┼─────────────────────────────────┤
│ Binary search for k     │ Binary search for k, but k      │
│ directly in array       │ isn't in array — it's the answer│
│                         │ (e.g., max sum, min days, etc.) │
│ Example: Find Target    │ Example: Split Array, Koko      │
└─────────────────────────┴─────────────────────────────────┘
```

---

## 📖 Book References
- *"Introduction to Algorithms" (CLRS)* — Chapter 2: Binary Search proof
- *"Competitive Programming 3" by Steven Halim* — Binary Search chapter
- *"Algorithms" by Sedgewick & Wayne* — Chapter 3.1
- *"The Art of Problem Solving"* — Binary search applications

---
*Tags: #BinarySearch #Arrays #DSA #SearchAlgorithm #RotatedArray*
*Next: [[04 - Sliding Window & Stack]] | [[🏠 HOME - DSA Master Index]]*
