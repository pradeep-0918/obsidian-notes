# 🟢 Hashing & Prefix Sum — Complete Guide
> *Back to [[🏠 HOME - DSA Master Index]] | Prev: [[01 - Two Pointers]] | Next: [[03 - Binary Search]]*

---

## 🎓 What IS Hashing? (Feynman Explanation)

> Imagine a magic filing cabinet. You tell it "store Apple → 5", and later ask "where is Apple?" — it tells you **instantly** without searching through all drawers.
> That magic cabinet is a **HashMap**. It uses a math trick (hash function) to convert your key into a drawer number.

**Core Idea:** Trade space for speed. Store things in a way that allows O(1) lookup.

```
Array lookup: O(n) — search one by one
HashMap lookup: O(1) — direct address ✨
```

---

## 🎓 What IS Prefix Sum? (Feynman Explanation)

> Imagine you run a lemonade stand. Each day you write how many cups you sold.
> To find how many cups sold from day 3 to day 7:
> - Dumb way: Add day3 + day4 + day5 + day6 + day7
> - Smart way: Keep a running total. `total[7] - total[2]` → DONE in O(1)!

```
prefix[i] = arr[0] + arr[1] + ... + arr[i]

Sum from L to R = prefix[R] - prefix[L-1]
```

---

## 🔵 Problem 1: Subarray Sum Equals K ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/subarray-sum-equals-k/

### 🧒 Explain Like I'm 6
Find how many continuous groups of numbers add up to exactly K.

### 🎯 The Magic Trick: Prefix Sum + HashMap
```
Key Insight:
  subarray sum from i to j = prefix[j] - prefix[i-1]
  
  We want: prefix[j] - prefix[i-1] = k
  So:      prefix[i-1] = prefix[j] - k
  
  → Store seen prefix sums in hashmap
  → At each step, check if (current_sum - k) exists in map!
```

### 💻 Code
```python
def subarraySum(nums, k):
    count = 0
    prefix_sum = 0
    # HashMap: {prefix_sum: how_many_times_seen}
    seen = {0: 1}  # ← CRUCIAL: empty prefix has sum 0
    
    for num in nums:
        prefix_sum += num
        
        # Check if complement exists
        if (prefix_sum - k) in seen:
            count += seen[prefix_sum - k]
        
        # Store current prefix sum
        seen[prefix_sum] = seen.get(prefix_sum, 0) + 1
    
    return count
```

### ⚡ Why `{0: 1}` initialization?
Because subarray starting from index 0 has prefix = nums[0], and complement = nums[0] - k. If k = nums[0], complement = 0, which needs to exist in map!

### 🧠 Visual Walkthrough
```
nums = [1, 1, 1], k = 2
 
Step 1: prefix=1, check (1-2)=-1 not in map, seen={0:1, 1:1}
Step 2: prefix=2, check (2-2)=0 in map → count+=1, seen={0:1,1:1,2:1}
Step 3: prefix=3, check (3-2)=1 in map → count+=1, seen={0:1,1:1,2:1,3:1}

Answer: 2 ✅
```

---

## 🟢 Problem 2: Top K Frequent Elements
> 🔗 https://leetcode.com/problems/top-k-frequent-elements/

### 🎯 Multiple Approaches

**Approach 1: Heap (Most Common)**
```python
from collections import Counter
import heapq

def topKFrequent(nums, k):
    freq = Counter(nums)  # {element: count}
    # Min-heap of size k
    return heapq.nlargest(k, freq.keys(), key=freq.get)
# Time: O(n log k), Space: O(n)
```

**Approach 2: Bucket Sort (O(n) — OPTIMAL!) ⭐**
```python
def topKFrequent(nums, k):
    freq = Counter(nums)
    # Bucket: index = frequency, value = list of numbers
    bucket = [[] for _ in range(len(nums) + 1)]
    
    for num, count in freq.items():
        bucket[count].append(num)
    
    result = []
    for i in range(len(bucket)-1, 0, -1):  # iterate from high freq to low
        result.extend(bucket[i])
        if len(result) >= k:
            return result[:k]
```

### 🧠 Why Bucket Sort?
```
Frequency can be at most n (all same elements)
→ Create n+1 buckets indexed by frequency
→ Collect from highest frequency bucket downward
→ O(n) time! Better than O(n log n) sorting
```

---

## 🔴 Problem 3: Majority Element (> n/2) — Boyer-Moore ⭐⭐⭐
> 🔗 https://leetcode.com/problems/majority-element/

### 🧒 Explain Like I'm 6
More than half the class voted for Pizza. Even if some voted differently, pizza still wins. If you cancel out every pizza vote with one non-pizza vote, pizza votes remain!

### 🎯 Boyer-Moore Voting Algorithm (O(1) Space Magic!)
```python
def majorityElement(nums):
    candidate = None
    count = 0
    
    for num in nums:
        if count == 0:
            candidate = num  # New candidate
        count += (1 if num == candidate else -1)
    
    return candidate  # Guaranteed to be majority element
```

### 🧠 Why Does This Work?
```
Every non-majority element cancels out one majority element.
Since majority appears > n/2 times, it ALWAYS survives.

Example: [3, 2, 3, 1, 3]
  3 → candidate=3, count=1
  2 → count=0
  3 → candidate=3, count=1
  1 → count=0
  3 → candidate=3, count=1
  
  Answer: 3 ✅
```

### ⚡ Time & Space
```
Time: O(n), Space: O(1) ← Better than HashMap!
```

---

## 🟡 Problem 4: Majority Element II (> n/3)
> 🔗 https://leetcode.com/problems/majority-element-ii/

### 🎯 Extended Boyer-Moore with 2 Candidates
```
Key Insight: At most 2 elements can appear > n/3 times
→ Track 2 candidates simultaneously!
```

```python
def majorityElement(nums):
    cand1, cand2 = None, None
    count1, count2 = 0, 0
    
    # Phase 1: Find candidates
    for num in nums:
        if num == cand1:
            count1 += 1
        elif num == cand2:
            count2 += 1
        elif count1 == 0:
            cand1, count1 = num, 1
        elif count2 == 0:
            cand2, count2 = num, 1
        else:
            count1 -= 1
            count2 -= 1
    
    # Phase 2: Verify candidates
    result = []
    for cand in [cand1, cand2]:
        if nums.count(cand) > len(nums) // 3:
            result.append(cand)
    
    return result
```

### 🧠 n/k Generalization
```
For > n/k majority: Track k-1 candidates
Maximum k-1 elements can exceed n/k threshold
```

---

## 🟣 Problem 5: Longest Consecutive Sequence ⭐⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/longest-consecutive-sequence/

### 🧒 Explain Like I'm 6
Given numbers [100, 4, 200, 1, 3, 2], find the longest chain of consecutive numbers. Answer: 1,2,3,4 → length 4.

### 🎯 The Trick: HashSet + Smart Starting Point
```
KEY INSIGHT: Only start counting from the BEGINNING of a sequence!
How to know it's a beginning? (num - 1) is NOT in set.

This way each element is processed at most TWICE → O(n)!
```

```python
def longestConsecutive(nums):
    num_set = set(nums)
    max_len = 0
    
    for num in num_set:
        # Only start if it's the beginning of a sequence
        if (num - 1) not in num_set:
            current = num
            length = 1
            
            while (current + 1) in num_set:
                current += 1
                length += 1
            
            max_len = max(max_len, length)
    
    return max_len
```

### ⚡ Why O(n) Despite Nested Loop?
```
Each number is visited AT MOST TWICE:
1. Once in the outer for loop
2. Once in the inner while loop (only if it's part of a sequence)

Total operations ≤ 2n → O(n) ✅
```

### 🧠 Visual
```
nums = [100, 4, 200, 1, 3, 2]
set  = {1, 2, 3, 4, 100, 200}

Check 100: (99) not in set → start! 101 not in set → length=1
Check 4:   (3) IS in set → skip (not a start)
Check 200: (199) not in set → start! 201 not in set → length=1
Check 1:   (0) not in set → start! 2,3,4 in set → length=4 ✅
```

---

## 🎯 Hashing Patterns Cheat Sheet

```
┌──────────────────────────────────────────────────────────────┐
│  PROBLEM                        TECHNIQUE                    │
├──────────────────────────────────────────────────────────────┤
│  Count frequencies              Counter / defaultdict(int)   │
│  Check if seen before           set                          │
│  Store index of last seen       dict {val: index}            │
│  Find complement (a+b=target)   dict {val: index}            │
│  Subarray sum = k               prefix_sum + dict            │
│  Majority element               Boyer-Moore voting           │
│  Top k elements                 heap or bucket sort          │
│  Consecutive sequence           HashSet + smart start        │
└──────────────────────────────────────────────────────────────┘
```

## 🎯 Prefix Sum Patterns Cheat Sheet

```python
# 1. Build prefix sum
prefix = [0] * (n + 1)
for i in range(n):
    prefix[i+1] = prefix[i] + arr[i]

# 2. Query range sum [L, R] (0-indexed)
range_sum = prefix[R+1] - prefix[L]

# 3. Subarray sum = k (counting)
seen = {0: 1}
curr = 0
for x in arr:
    curr += x
    count += seen.get(curr - k, 0)
    seen[curr] = seen.get(curr, 0) + 1
```

---

## 📖 Book References
- *"Introduction to Algorithms" (CLRS)* — Chapter 11: Hash Tables
- *"Programming Pearls" by Jon Bentley* — Column 1 (Space-Time tradeoffs)
- *"Algorithm Design Manual" by Skiena* — Hashing chapter
- *"Data Structures & Algorithms in Python"* by Goodrich

---
*Tags: #Hashing #PrefixSum #HashMap #Arrays #BoyerMoore #DSA*
*Next: [[03 - Binary Search]] | [[🏠 HOME - DSA Master Index]]*
