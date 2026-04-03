# 🔵 Two Pointers — Complete Guide
> *Back to [[🏠 HOME - DSA Master Index]] | Quick Ref: [[⚡ TRICKS & PATTERNS CHEATSHEET]]*

---

## 🎓 What IS Two Pointers? (Feynman Explanation)

> Imagine you have a row of lockers numbered 1 to 100. You want to find two lockers that add up to 50. 
> - **Brute Force (Dumb way):** Open every locker, then for each locker open every other locker → **Too slow!**
> - **Two Pointer (Smart way):** Stand one person at locker 1, another at locker 100. If sum is too big → move right person left. If too small → move left person right. Done in one walk! ✅

**Core Idea:** Use two index variables that move toward each other (or in same direction) to avoid nested loops.

```
Brute Force: O(n²)  ──────►  Two Pointers: O(n) 🚀
```

---

## 📋 When To Use Two Pointers?

```
✅ Array is SORTED (or you can sort it)
✅ You need pairs/triplets that satisfy a condition
✅ You need to partition/rearrange an array
✅ You need to remove duplicates in-place
✅ Problem says "find pair with sum = target"
```

---

## 🔵 Problem 1: Two Sum II — Input Array is Sorted
> 🔗 https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/

### 🧒 Explain Like I'm 6
You have sorted numbers: `[2, 7, 11, 15]`, target = 9.
Start from both ends. Add them. Too big? Move right pointer left. Too small? Move left pointer right.

### 🎯 The Pattern (MEMORIZE THIS)
```
LEFT pointer starts at index 0
RIGHT pointer starts at last index

while left < right:
    sum = arr[left] + arr[right]
    if sum == target → FOUND! ✅
    if sum < target  → left++   (need bigger number)
    if sum > target  → right--  (need smaller number)
```

### 💻 Code
```python
def twoSum(numbers, target):
    left, right = 0, len(numbers) - 1
    
    while left < right:
        curr_sum = numbers[left] + numbers[right]
        
        if curr_sum == target:
            return [left + 1, right + 1]  # 1-indexed
        elif curr_sum < target:
            left += 1   # ← need bigger, move left forward
        else:
            right -= 1  # ← need smaller, move right back
    
    return [-1, -1]
```

### ⚡ Time & Space
| | Complexity |
|--|--|
| Time | O(n) |
| Space | O(1) |

### Extension: 4Sum
> See also: [[#Problem 3 3Sum]] for the pattern that scales!

**4Sum trick:** Sort → Fix 2 pointers with 2 loops → Use two-pointer for remaining 2
```python
# 4Sum = Two outer loops + inner Two Pointer
for i in range(n):
    for j in range(i+1, n):
        # Two pointer on rest
        left, right = j+1, n-1
        target_2 = target - nums[i] - nums[j]
        # ... same two pointer logic
```

---

## 🟢 Problem 2: Sort Colors (Dutch National Flag)
> 🔗 https://leetcode.com/problems/sort-colors/

### 🧒 Explain Like I'm 6
You have red, white, blue balls mixed up. Sort them using 3 pointers — like sorting into 3 buckets but IN PLACE!

### 🎯 The Dutch National Flag Algorithm
```
Three pointers:
  low  → boundary of 0s (red)
  mid  → current element being checked
  high → boundary of 2s (blue)

RULE:
  arr[mid] == 0 → swap(low, mid), low++, mid++
  arr[mid] == 1 → mid++ (already in place)
  arr[mid] == 2 → swap(mid, high), high-- (DON'T mid++ !)
```

### 💻 Code
```python
def sortColors(nums):
    low = mid = 0
    high = len(nums) - 1
    
    while mid <= high:
        if nums[mid] == 0:
            nums[low], nums[mid] = nums[mid], nums[low]
            low += 1
            mid += 1
        elif nums[mid] == 1:
            mid += 1
        else:  # nums[mid] == 2
            nums[mid], nums[high] = nums[high], nums[mid]
            high -= 1
            # ⚠️ DON'T increment mid! New element at mid is unprocessed
    
    return nums
```

### ⚡ Why Don't We increment `mid` when swapping with `high`?
Because we swap an **unprocessed** element from `high` to `mid`. We haven't seen that element yet, so check it again!

### 🔑 KEY INSIGHT
```
[0, 0, 0 | 1, 1, 1 | ?, ?, ? | 2, 2, 2]
          low      mid        high
                    ↑ 
               Working zone
```

---

## 🔴 Problem 3: 3Sum
> 🔗 https://leetcode.com/problems/3sum/

### 🧒 Explain Like I'm 6
Find 3 friends from a list whose ages add up to zero. No same group twice!

### 🎯 The Pattern
```
Step 1: SORT the array
Step 2: Fix one element (outer loop)
Step 3: Two pointer on the remaining part

for i in range(n):
    if i > 0 and nums[i] == nums[i-1]: SKIP (avoid duplicates)
    left = i + 1, right = n - 1
    Two Pointer with target = -nums[i]
```

### 💻 Code
```python
def threeSum(nums):
    nums.sort()
    result = []
    
    for i in range(len(nums) - 2):
        # Skip duplicates for first element
        if i > 0 and nums[i] == nums[i-1]:
            continue
        
        left, right = i + 1, len(nums) - 1
        
        while left < right:
            total = nums[i] + nums[left] + nums[right]
            
            if total == 0:
                result.append([nums[i], nums[left], nums[right]])
                # Skip duplicates for second and third elements
                while left < right and nums[left] == nums[left+1]:
                    left += 1
                while left < right and nums[right] == nums[right-1]:
                    right -= 1
                left += 1
                right -= 1
            elif total < 0:
                left += 1
            else:
                right -= 1
    
    return result
```

### ⚡ Time & Space
| | Complexity |
|--|--|
| Time | O(n²) |
| Space | O(1) extra |

### 🧠 Duplicate Skipping (CRUCIAL TRICK)
```python
# After finding a valid triplet, skip duplicates:
while left < right and nums[left] == nums[left+1]: left += 1
while left < right and nums[right] == nums[right-1]: right -= 1
```

### 🔗 2Sum → 3Sum → 4Sum Pattern
```
2Sum: 1 loop + two pointer → O(n)
3Sum: 1 fixed + two pointer → O(n²)
4Sum: 2 fixed + two pointer → O(n³)
kSum: (k-2) fixed + two pointer → O(n^(k-1))
```

---

## 🟡 Problem 4: Rearrange Array Alternately
> 🔗 https://www.geeksforgeeks.org/problems/-rearrange-array-alternately/1

### 🧒 Explain Like I'm 6
Given sorted array `[1,2,3,4,5,6]`, rearrange to `[6,1,5,2,4,3]` — alternate max and min!

### 🎯 The Trick: Encoding Two Values in One Cell
```python
# Magic formula: arr[i] = arr[i] + (value_to_store % max_val) * max_val
# To retrieve: arr[i] // max_val (new value) or arr[i] % max_val (old value)
```

### 💻 Code
```python
def rearrange(arr, n):
    max_idx = n - 1  # pointer to max
    min_idx = 0      # pointer to min
    max_elem = arr[n-1] + 1  # element greater than all
    
    for i in range(n):
        if i % 2 == 0:  # even index → max element
            arr[i] += (arr[max_idx] % max_elem) * max_elem
            max_idx -= 1
        else:           # odd index → min element
            arr[i] += (arr[min_idx] % max_elem) * max_elem
            min_idx += 1
    
    # Decode
    for i in range(n):
        arr[i] //= max_elem
    
    return arr
```

---

## 🟣 Problem 5: Minimum Swaps to Sort
> 🔗 https://www.geeksforgeeks.org/problems/minimum-swaps/1

### 🧒 Explain Like I'm 6
You have students standing in wrong order. How many swaps (exchanges of position) needed to sort them?

### 🎯 The Key Insight: Cycle Detection!
```
Each element needs to go to its "correct" position.
Elements form cycles. Swaps needed = (cycle_length - 1) per cycle.

Total swaps = Σ (cycle_length - 1) for all cycles
            = n - (number of cycles)
```

### 💻 Code
```python
def minSwaps(arr):
    n = len(arr)
    # Create (value, original_index) pairs and sort
    indexed = sorted(enumerate(arr), key=lambda x: x[1])
    
    visited = [False] * n
    swaps = 0
    
    for i in range(n):
        if visited[i] or indexed[i][0] == i:
            continue
        
        # Count cycle length
        cycle_size = 0
        j = i
        while not visited[j]:
            visited[j] = True
            j = indexed[j][0]
            cycle_size += 1
        
        swaps += (cycle_size - 1)
    
    return swaps
```

### 🧠 Visual: Cycle Detection
```
Array: [4, 3, 2, 1]
Sorted: [1, 2, 3, 4]

1 should be at index 0, currently at index 3
4 should be at index 3, currently at index 0
→ Cycle: 0→3→0, length=2, swaps=1

2 should be at index 1, currently at index 2
3 should be at index 2, currently at index 1
→ Cycle: 1→2→1, length=2, swaps=1

Total swaps = 2 ✅
```

---

## 🎯 Two Pointers: Ultimate Pattern Summary

```
┌─────────────────────────────────────────────────────┐
│  PROBLEM TYPE          →  STRATEGY                  │
├─────────────────────────────────────────────────────┤
│  Pair sum in sorted    →  Opposite ends             │
│  Remove duplicates     →  Same direction (fast/slow)│
│  Partition array       →  Dutch National Flag       │
│  Triplet sum           →  Fix one + two pointer     │
│  k-sum                 →  Fix k-2 + two pointer     │
└─────────────────────────────────────────────────────┘
```

---

## 📖 Book References
- *"Introduction to Algorithms" (CLRS)* — Chapter on Sorting
- *"Cracking the Coding Interview"* — Chapter 11 (Sorting & Searching)
- *"Elements of Programming Interviews"* — Array problems chapter
- *"Algorithm Design Manual" by Skiena* — Chapter 4

---
*Tags: #TwoPointers #Arrays #DSA #Sorting #ThreeSum #DutchFlag*
*Next: [[02 - Hashing & Prefix Sum]] | [[🏠 HOME - DSA Master Index]]*
