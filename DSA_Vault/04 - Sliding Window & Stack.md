# 🔴 Sliding Window & Stack — Complete Guide
> *Back to [[🏠 HOME - DSA Master Index]] | Prev: [[03 - Binary Search]] | Next: [[05 - Advanced Array Problems]]*

---

## 🎓 What IS a Sliding Window? (Feynman Explanation)

> Imagine you're looking through a train window. As the train moves, the window slides forward — you see new things on one side and old things disappear on the other side.
> 
> In arrays: instead of recalculating the whole subarray every time, just **add the new element** coming in and **remove the old element** going out. O(n) instead of O(n²)!

```
Brute Force: Check every subarray → O(n²) or O(n³)
Sliding Window: Maintain a "window" → O(n) 🚀
```

---

## 🎓 What IS a Monotonic Stack? (Feynman Explanation)

> Imagine a queue for a ride where tall people block the view of shorter people behind them. If a taller person comes, all shorter people behind are now "blocked" (useless). We kick them out!
> 
> **Monotonic Stack:** A stack that stays always increasing OR always decreasing by popping elements that violate the order.

```
Monotonic INCREASING stack: bottom → top is increasing (small at bottom, large at top)
Monotonic DECREASING stack: bottom → top is decreasing (large at bottom, small at top)
```

---

## 🔵 Problem 1: Sliding Window Maximum ⭐⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/sliding-window-maximum/

### 🧒 Explain Like I'm 6
You have a row of numbers. A window of size k moves across it. At each step, find the maximum in the window.

### 🎯 The Trick: Monotonic Deque (Double-ended Queue)
```
Maintain a deque that stores INDICES in DECREASING order of values.
→ Front of deque always has the index of current max!

When sliding:
  1. Remove indices outside the window from FRONT
  2. Remove from BACK any index whose value ≤ current (they're useless!)
  3. Add current index to BACK
  4. Front of deque = current window max
```

```python
from collections import deque

def maxSlidingWindow(nums, k):
    dq = deque()   # Stores indices, front=max
    result = []
    
    for i, num in enumerate(nums):
        # Remove elements outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        
        # Remove smaller elements from back (they're useless)
        while dq and nums[dq[-1]] < num:
            dq.pop()
        
        dq.append(i)
        
        # Window is full
        if i >= k - 1:
            result.append(nums[dq[0]])
    
    return result
```

### 🧠 Visual Walkthrough
```
nums = [1,3,-1,-3,5,3,6,7], k=3

i=0: dq=[0]
i=1: 3>1, pop 0. dq=[1]
i=2: -1<3, keep. dq=[1,2]. Window full → result=[3]
i=3: -3<-1, keep. dq=[1,2,3]. pop 1 (out of window[1,3])? No. result=[3,3]
i=4: 5>-3,-1,3 → pop all. dq=[4]. result=[3,3,5]
...
```

### ⚡ Time: O(n), Space: O(k)

---

## 🟢 Problem 2: Largest Rectangle in Histogram ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/largest-rectangle-in-histogram/

### 🧒 Explain Like I'm 6
You have a bar graph. Find the biggest rectangle you can draw inside it.

### 🎯 Monotonic Stack Approach
```
Key Insight: For each bar, find the WIDEST rectangle with that bar's height.
→ Need: left boundary (first bar shorter than current, on left)
         right boundary (first bar shorter than current, on right)
→ Use a monotonic INCREASING stack!
```

```python
def largestRectangleArea(heights):
    stack = []  # Monotonic increasing stack (stores indices)
    max_area = 0
    heights = heights + [0]  # Sentinel 0 at end to flush remaining bars
    
    for i, h in enumerate(heights):
        while stack and heights[stack[-1]] > h:
            height = heights[stack.pop()]
            width = i if not stack else i - stack[-1] - 1
            max_area = max(max_area, height * width)
        stack.append(i)
    
    return max_area
```

### 🧠 Visual
```
Heights: [2, 1, 5, 6, 2, 3]

When we hit h=2 (index 4, height=2):
  Stack has [1(h=1), 2(h=5), 3(h=6)]
  Pop 6 (height=6): width = 4 - 2 - 1 = 1, area = 6
  Pop 5 (height=5): width = 4 - 1 - 1 = 2, area = 10 ← MAX!
  ...
```

### 💡 Key Formula
```
When popping bar at index `top`:
  height = heights[top]
  width = i - stack[-1] - 1  (if stack not empty)
          i                   (if stack is empty → extends to left edge)
  area = height × width
```

---

## 🔴 Problem 3: Trapping Rain Water ⭐⭐⭐⭐
> 🔗 https://leetcode.com/problems/trapping-rain-water/

### 🧒 Explain Like I'm 6
Water fills valleys between walls. For each position, water level = min(tallest wall on left, tallest wall on right) - current height.

### 🎯 Three Approaches

**Approach 1: Pre-compute Left & Right Max (O(n) space)**
```python
def trap(height):
    n = len(height)
    left_max = [0] * n
    right_max = [0] * n
    
    left_max[0] = height[0]
    for i in range(1, n):
        left_max[i] = max(left_max[i-1], height[i])
    
    right_max[n-1] = height[n-1]
    for i in range(n-2, -1, -1):
        right_max[i] = max(right_max[i+1], height[i])
    
    water = 0
    for i in range(n):
        water += min(left_max[i], right_max[i]) - height[i]
    
    return water
```

**Approach 2: Two Pointers (O(1) space — OPTIMAL!) ⭐**
```python
def trap(height):
    left, right = 0, len(height) - 1
    left_max = right_max = 0
    water = 0
    
    while left < right:
        if height[left] < height[right]:
            if height[left] >= left_max:
                left_max = height[left]
            else:
                water += left_max - height[left]
            left += 1
        else:
            if height[right] >= right_max:
                right_max = height[right]
            else:
                water += right_max - height[right]
            right -= 1
    
    return water
```

**Approach 3: Stack (O(n) space)**
```python
def trap(height):
    stack = []
    water = 0
    
    for i, h in enumerate(height):
        while stack and height[stack[-1]] < h:
            top = stack.pop()
            if not stack: break
            
            width = i - stack[-1] - 1
            bounded_height = min(h, height[stack[-1]]) - height[top]
            water += width * bounded_height
        
        stack.append(i)
    
    return water
```

### 🧠 Key Formula
```
Water at position i = min(max_left, max_right) - height[i]
```

---

## 🎯 Sliding Window Patterns

### Fixed Window Size
```python
# Sum of every subarray of size k
def fixed_window(arr, k):
    window_sum = sum(arr[:k])
    result = [window_sum]
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i-k]  # Add new, remove old
        result.append(window_sum)
    return result
```

### Variable Window Size
```python
# Find smallest subarray with sum >= target
def variable_window(arr, target):
    left = curr_sum = 0
    min_len = float('inf')
    for right in range(len(arr)):
        curr_sum += arr[right]
        while curr_sum >= target:
            min_len = min(min_len, right - left + 1)
            curr_sum -= arr[left]
            left += 1
    return min_len
```

---

## 🎯 Stack Pattern Summary

```
┌────────────────────────────────────────────────────────────┐
│  PROBLEM                     STACK TYPE                    │
├────────────────────────────────────────────────────────────┤
│  Sliding window max          Monotonic DECREASING deque    │
│  Largest rectangle           Monotonic INCREASING stack    │
│  Trapping rain water         Monotonic DECREASING stack    │
│  Next greater element        Monotonic DECREASING stack    │
│  Daily temperatures          Monotonic DECREASING stack    │
│  Valid parentheses           Regular stack                 │
└────────────────────────────────────────────────────────────┘
```

### When to use Monotonic Stack?
```
✅ "Find next greater/smaller element"
✅ "Find previous greater/smaller element"  
✅ Problems involving SPAN or WIDTH of valid ranges
✅ Histogram, rain water, view problems
```

---

## 📖 Book References
- *"Introduction to Algorithms" (CLRS)* — Chapter 10: Elementary Data Structures (Stacks)
- *"Competitive Programming Handbook" by Antti Laaksonen* — Chapter on monotonic queues
- *"Cracking the Coding Interview"* — Chapter 3: Stacks & Queues
- *"LeetCode Patterns"* by Sean Prashad

---
*Tags: #SlidingWindow #MonotonicStack #Deque #Arrays #DSA #RainWater #Histogram*
*Next: [[05 - Advanced Array Problems]] | [[🏠 HOME - DSA Master Index]]*
