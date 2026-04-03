# ⚡ TRICKS & PATTERNS CHEATSHEET
> *The ultimate "solve in seconds" reference | Back to [[🏠 HOME - DSA Master Index]]*

---

## 🚀 Instant Pattern Recognition

```
See these keywords in the problem → Think this technique:

"sorted array" + "pair/triplet sum"     → TWO POINTERS
"subarray sum = k"                       → PREFIX SUM + HASHMAP
"majority element"                       → BOYER-MOORE VOTING
"consecutive sequence"                   → HASHSET
"rotated sorted array"                   → BINARY SEARCH (one half always sorted)
"minimum/maximum of all windows"         → MONOTONIC DEQUE
"next greater element"                   → MONOTONIC STACK
"histogram rectangle"                    → MONOTONIC STACK
"rain water / container"                 → TWO POINTERS or STACK
"first missing positive"                 → ARRAY AS HASHSET (cyclic sort)
"count inversions"                       → MERGE SORT
"median of sorted arrays"                → BINARY SEARCH ON PARTITION
"max subarray sum"                       → KADANE'S ALGORITHM
"buy sell stock"                         → ONE-PASS MIN TRACKING
"top k frequent"                         → HEAP or BUCKET SORT
"minimize the maximum"                   → BINARY SEARCH ON ANSWER
"find kth element"                       → QUICKSELECT or BINARY SEARCH
```

---

## 🔑 Code Templates (Copy-Paste Ready)

### Template 1: Two Pointers (Pair Sum)
```python
def two_pointer_sum(arr, target):
    left, right = 0, len(arr)-1
    while left < right:
        s = arr[left] + arr[right]
        if s == target: return [left, right]
        elif s < target: left += 1
        else: right -= 1
```

### Template 2: Prefix Sum + HashMap
```python
def prefix_hash(arr, k):
    seen = {0: 1}
    curr = count = 0
    for x in arr:
        curr += x
        count += seen.get(curr - k, 0)
        seen[curr] = seen.get(curr, 0) + 1
    return count
```

### Template 3: Binary Search (Find First True)
```python
def binary_search_first_true(left, right, condition):
    while left < right:
        mid = left + (right - left) // 2
        if condition(mid):
            right = mid
        else:
            left = mid + 1
    return left
```

### Template 4: Sliding Window (Variable Size)
```python
def sliding_window(arr, target):
    left = curr = result = 0
    for right in range(len(arr)):
        curr += arr[right]
        while curr > target:  # Shrink
            curr -= arr[left]
            left += 1
        result = max(result, right - left + 1)
    return result
```

### Template 5: Monotonic Deque (Window Max)
```python
from collections import deque
def window_max(nums, k):
    dq, result = deque(), []
    for i, num in enumerate(nums):
        while dq and dq[0] < i - k + 1: dq.popleft()
        while dq and nums[dq[-1]] < num: dq.pop()
        dq.append(i)
        if i >= k-1: result.append(nums[dq[0]])
    return result
```

### Template 6: Monotonic Stack (Next Greater)
```python
def next_greater(arr):
    stack, result = [], [-1] * len(arr)
    for i, num in enumerate(arr):
        while stack and arr[stack[-1]] < num:
            result[stack.pop()] = num
        stack.append(i)
    return result
```

### Template 7: Kadane's Algorithm
```python
def kadane(nums):
    max_s = curr = nums[0]
    for num in nums[1:]:
        curr = max(num, curr + num)
        max_s = max(max_s, curr)
    return max_s
```

### Template 8: Boyer-Moore Voting
```python
def boyer_moore(nums):
    cand, count = None, 0
    for num in nums:
        if count == 0: cand = num
        count += 1 if num == cand else -1
    return cand
```

### Template 9: Dutch National Flag
```python
def dutch_flag(arr):  # Sort 0,1,2
    lo = mid = 0; hi = len(arr)-1
    while mid <= hi:
        if arr[mid] == 0: arr[lo],arr[mid]=arr[mid],arr[lo]; lo+=1; mid+=1
        elif arr[mid] == 1: mid+=1
        else: arr[mid],arr[hi]=arr[hi],arr[mid]; hi-=1
```

---

## 🧠 Complexity Quick Reference

| Algorithm | Time | Space |
|-----------|------|-------|
| Two Pointers | O(n) | O(1) |
| Prefix Sum + Hash | O(n) | O(n) |
| Binary Search | O(log n) | O(1) |
| Binary Search on Answer | O(n log n) | O(1) |
| Sliding Window (fixed) | O(n) | O(1) |
| Monotonic Deque | O(n) | O(k) |
| Monotonic Stack | O(n) | O(n) |
| Kadane's | O(n) | O(1) |
| Boyer-Moore | O(n) | O(1) |
| Modified Merge Sort | O(n log n) | O(n) |
| Dutch National Flag | O(n) | O(1) |

---

## 💡 Instant Tips

```
🔹 Avoid O(n²): Think hashing or two pointers
🔹 Need O(log n): Think binary search (even on answer space!)
🔹 O(1) space for majority: Boyer-Moore
🔹 O(1) space with array: Use array as hashmap (index trick)
🔹 Sort + pair check: Always consider two pointers
🔹 "All windows of size k": Monotonic Deque
🔹 "Previous/Next smaller or larger": Monotonic Stack
🔹 Subarray with negative numbers: Prefix sum (not sliding window!)
🔹 Count inversions / pairs: Merge sort
🔹 Sum of subarrays: Prefix sum array
```

---

## 🔥 Interview 60-Second Decision Tree

```
Is array SORTED?
├── YES → Try Two Pointers or Binary Search
└── NO  → Can I SORT it? (check if order matters)
           ├── YES → Sort first, then two pointers
           └── NO  → Try HashMap / HashSet / Prefix Sum

Is it about SUBARRAYS?
├── Fixed size → Sliding Window (add new, remove old)
├── Variable size, positives only → Sliding Window
└── Variable size, any numbers → Prefix Sum + HashMap

Is it about COUNTING pairs/inversions?
└── Modified Merge Sort

Is it about WINDOWS with MIN/MAX?
└── Monotonic Deque

Is it about SPANS or NEXT GREATER?
└── Monotonic Stack
```

---
*Tags: #Cheatsheet #Tricks #Patterns #Interview #DSA*
*See Also: [[🔁 REVISION PAGE - Quick Recall]] | [[🏠 HOME - DSA Master Index]]*
