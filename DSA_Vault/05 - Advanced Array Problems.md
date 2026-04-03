# 🟣 Advanced Array Problems — Complete Guide
> *Back to [[🏠 HOME - DSA Master Index]] | Prev: [[04 - Sliding Window & Stack]]*

---

## 🔵 Problem 1: First Missing Positive ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/first-missing-positive/

### 🧒 Explain Like I'm 6
Find the smallest positive number (1, 2, 3...) that's NOT in the list. Magic trick: use the array itself as a HashSet!

### 🎯 The Genius Trick: Array as Hash Table
```
KEY INSIGHT:
  Answer is always in range [1, n+1]
  → Use array positions to mark which numbers exist
  → Place each number at its "correct" index (number 1 at index 0, etc.)
```

```python
def firstMissingPositive(nums):
    n = len(nums)
    
    # Phase 1: Put each number in its correct position
    for i in range(n):
        while 1 <= nums[i] <= n and nums[nums[i]-1] != nums[i]:
            # Swap nums[i] to its correct position
            nums[nums[i]-1], nums[i] = nums[i], nums[nums[i]-1]
    
    # Phase 2: Find first position where number doesn't match
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1
    
    return n + 1  # All 1 to n present, answer is n+1
```

### 🧠 Visual
```
[3, 4, -1, 1]
After Phase 1: [1, -1, 3, 4]  (1→pos0, 3→pos2, 4→pos3)
Check: pos0=1✅, pos1=-1❌ → return 2
```

### ⚡ Time: O(n), Space: O(1) 🔥

---

## 🟢 Problem 2: Median of Two Sorted Arrays ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/median-of-two-sorted-arrays/

### 🧒 Explain Like I'm 6
Two sorted lists. Find the middle value if they were merged — WITHOUT actually merging!

### 🎯 Binary Search on Partition
```
KEY INSIGHT:
  Binary search on the smaller array.
  Find partition point such that:
    LEFT half of array1 + LEFT half of array2 = RIGHT half
  Partition is correct when:
    max(left1, left2) ≤ min(right1, right2)
```

```python
def findMedianSortedArrays(nums1, nums2):
    # Ensure nums1 is smaller
    if len(nums1) > len(nums2):
        nums1, nums2 = nums2, nums1
    
    m, n = len(nums1), len(nums2)
    left, right = 0, m
    
    while left <= right:
        partition1 = (left + right) // 2
        partition2 = (m + n + 1) // 2 - partition1
        
        max_left1 = float('-inf') if partition1 == 0 else nums1[partition1-1]
        min_right1 = float('inf') if partition1 == m else nums1[partition1]
        max_left2 = float('-inf') if partition2 == 0 else nums2[partition2-1]
        min_right2 = float('inf') if partition2 == n else nums2[partition2]
        
        if max_left1 <= min_right2 and max_left2 <= min_right1:
            # Correct partition found!
            if (m + n) % 2 == 0:
                return (max(max_left1, max_left2) + min(min_right1, min_right2)) / 2
            else:
                return max(max_left1, max_left2)
        elif max_left1 > min_right2:
            right = partition1 - 1   # Too far right in nums1
        else:
            left = partition1 + 1    # Too far left in nums1
```

### ⚡ Time: O(log(min(m,n))), Space: O(1)

---

## 🔴 Problem 3: Count Inversions ⭐⭐⭐
> 🔗 https://www.geeksforgeeks.org/problems/inversion-of-array/1

### 🧒 Explain Like I'm 6
An inversion is when a bigger number comes BEFORE a smaller one. Count all such "wrong order" pairs.

### 🎯 Modified Merge Sort
```
KEY INSIGHT:
  During merge sort, when we take element from RIGHT side before LEFT side,
  ALL remaining elements in left half form inversions with it!
```

```python
def merge_count(arr, temp, left, right):
    if right == left:
        return 0
    
    mid = (left + right) // 2
    count = 0
    count += merge_count(arr, temp, left, mid)
    count += merge_count(arr, temp, mid+1, right)
    count += merge(arr, temp, left, mid, right)
    return count

def merge(arr, temp, left, mid, right):
    i, j, k = left, mid+1, left
    count = 0
    
    while i <= mid and j <= right:
        if arr[i] <= arr[j]:
            temp[k] = arr[i]
            i += 1
        else:
            # arr[j] < arr[i]: all elements from i to mid are inversions!
            count += (mid - i + 1)
            temp[k] = arr[j]
            j += 1
        k += 1
    
    while i <= mid:
        temp[k] = arr[i]; i += 1; k += 1
    while j <= right:
        temp[k] = arr[j]; j += 1; k += 1
    
    arr[left:right+1] = temp[left:right+1]
    return count

def inversionCount(arr, n):
    temp = [0] * n
    return merge_count(arr, temp, 0, n-1)
```

### ⚡ Time: O(n log n), Space: O(n)

---

## 🟡 Problem 4: Reverse Pairs ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/reverse-pairs/

### 🎯 Modified Merge Sort (Count phase BEFORE merge)
```
Condition: nums[i] > 2 * nums[j] where i < j

KEY: During merge sort, for each i in left half,
     count j's in right half where nums[i] > 2*nums[j]
     → Use two pointer since both halves are sorted!
```

```python
def reversePairs(nums):
    def merge_count(arr, left, right):
        if right == left:
            return 0
        mid = (left + right) // 2
        count = merge_count(arr, left, mid) + merge_count(arr, mid+1, right)
        
        # Count reverse pairs (BEFORE merging, both halves sorted)
        j = mid + 1
        for i in range(left, mid+1):
            while j <= right and arr[i] > 2 * arr[j]:
                j += 1
            count += j - (mid + 1)
        
        # Standard merge
        arr[left:right+1] = sorted(arr[left:right+1])
        return count
    
    return merge_count(nums, 0, len(nums)-1)
```

---

## 🟠 Problem 5: Merge Without Extra Space
> 🔗 https://www.geeksforgeeks.org/problems/merge-two-sorted-arrays5135/1

### 🎯 Gap Method (Shell Sort Idea)
```python
def merge_without_extra(arr1, arr2, n, m):
    gap = (n + m + 1) // 2
    
    while gap > 0:
        i, j = 0, gap
        
        while j < n + m:
            # Both in arr1
            a = arr1[i] if i < n else arr2[i - n]
            b = arr1[j] if j < n else arr2[j - n]
            
            if a > b:
                if i < n and j < n:
                    arr1[i], arr1[j] = arr1[j], arr1[i]
                elif i < n and j >= n:
                    arr1[i], arr2[j-n] = arr2[j-n], arr1[i]
                else:
                    arr2[i-n], arr2[j-n] = arr2[j-n], arr2[i-n]
            i += 1; j += 1
        
        gap = gap // 2 if gap > 1 else 0
```

---

## ⭐⭐⭐⭐⭐⭐⭐ Kadane's Algorithm — MUST KNOW FOR INTERVIEWS
> 🔗 https://leetcode.com/problems/maximum-subarray/

### 🧒 Explain Like I'm 6
You're walking a path. Each step you gain or lose points. At each step: if keeping your history helps → keep it. If your history is negative baggage → start fresh!

### 🎯 The Algorithm (MEMORIZE!)
```python
def maxSubArray(nums):
    max_sum = nums[0]      # Global max
    curr_sum = nums[0]     # Current window sum
    
    for num in nums[1:]:
        curr_sum = max(num, curr_sum + num)  # Start fresh OR continue
        max_sum = max(max_sum, curr_sum)
    
    return max_sum
```

### 🧠 The Decision at Each Step
```
max(num, curr_sum + num)
  If curr_sum < 0: start fresh (num alone is better)
  If curr_sum ≥ 0: extend window (curr_sum + num is better)
```

### Extended: Return Actual Subarray
```python
def maxSubArray_with_indices(nums):
    max_sum = curr_sum = nums[0]
    start = end = temp_start = 0
    
    for i in range(1, len(nums)):
        if curr_sum + nums[i] < nums[i]:
            curr_sum = nums[i]
            temp_start = i       # Potential new start
        else:
            curr_sum += nums[i]
        
        if curr_sum > max_sum:
            max_sum = curr_sum
            start = temp_start
            end = i
    
    return nums[start:end+1], max_sum
```

### ⚡ Time: O(n), Space: O(1)

---

## 🟢 Stock Buy and Sell
> 🔗 https://leetcode.com/problems/best-time-to-buy-and-sell-stock/

### 🎯 One-Pass Greedy
```python
def maxProfit(prices):
    min_price = float('inf')
    max_profit = 0
    
    for price in prices:
        min_price = min(min_price, price)         # Track minimum seen so far
        max_profit = max(max_profit, price - min_price)  # Best profit if sell today
    
    return max_profit
```

---

## 🔴 Longest Subarray with Sum K (Positives Only)
> 🔗 https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1

### 🎯 Sliding Window (works only for non-negative numbers)
```python
def longestSubarray(arr, k):
    left = curr_sum = max_len = 0
    
    for right in range(len(arr)):
        curr_sum += arr[right]
        
        while curr_sum > k and left <= right:
            curr_sum -= arr[left]
            left += 1
        
        if curr_sum == k:
            max_len = max(max_len, right - left + 1)
    
    return max_len
```

For arrays with **negative numbers** → use prefix sum + hashmap (see [[02 - Hashing & Prefix Sum]])

---

## 🟡 Maximum Consecutive Ones
> 🔗 https://leetcode.com/problems/max-consecutive-ones/

```python
def findMaxConsecutiveOnes(nums):
    max_ones = curr = 0
    for num in nums:
        if num == 1:
            curr += 1
            max_ones = max(max_ones, curr)
        else:
            curr = 0
    return max_ones
```

---

## 🎯 Advanced Problem Patterns Summary

```
┌──────────────────────────────────────────────────────────────┐
│  TECHNIQUE              USED FOR                            │
├──────────────────────────────────────────────────────────────┤
│  Array as HashSet       First Missing Positive, Cycle sort  │
│  Modified Merge Sort    Count inversions, Reverse pairs     │
│  Binary Search on       Median of two sorted arrays         │
│    partition            Split array, etc.                   │
│  Gap Method             Merge without extra space           │
│  Kadane's DP            Maximum subarray sum                │
│  One-pass Greedy        Stock buy/sell                      │
│  Sliding Window         Longest subarray (positive nums)    │
│  Prefix Sum + HashMap   Longest subarray (any nums)         │
└──────────────────────────────────────────────────────────────┘
```

---

## 📖 Book References
- *"Introduction to Algorithms" (CLRS)* — Chapter 33: Maximum Subarray (Kadane's proof)
- *"Algorithm Design" by Kleinberg & Tardos* — Dynamic Programming intro
- *"Programming Pearls" by Jon Bentley* — Column 8 (Maximum subarray origin story!)
- *"Competitive Programmer's Handbook"* — Divide & Conquer chapter

---
*Tags: #Kadane #MergeSort #InversionCount #FirstMissingPositive #Stock #DSA #Advanced*
*Back to: [[🏠 HOME - DSA Master Index]] | [[🔁 REVISION PAGE - Quick Recall]]*
