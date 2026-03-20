# ⚙️ Binary Search

#Algorithm #Searching #BinarySearch

⬅️ [[Algorithms Index]] | ➡️ [[Sliding Window]]

---

## 📌 Definition

**Binary Search** is a divide-and-conquer algorithm that finds a target in a **sorted** collection by repeatedly halving the search space.

---

## ⚙️ How It Works

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = left + (right - left) // 2  # Avoid overflow
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

**Key Insight:** Each iteration eliminates half the search space → O(log n)

---

## 🧠 When to Use

- Sorted array — find element
- "Search space" can be reduced by half
- Monotonic function → find boundary
- Answer lies in a range → binary search on answer

---

## 🔥 Key Patterns

### 1. Standard (find exact value)
```python
left, right = 0, n-1
while left <= right:
    mid = (left+right)//2
```

### 2. Find left boundary (first occurrence)
```python
while left < right:
    mid = (left+right)//2
    if arr[mid] < target: left = mid+1
    else: right = mid
```

### 3. Find right boundary (last occurrence)
```python
while left < right:
    mid = (left+right+1)//2
    if arr[mid] > target: right = mid-1
    else: left = mid
```

### 4. Binary search on answer
```python
left, right = min_possible, max_possible
while left < right:
    mid = (left+right)//2
    if feasible(mid): right = mid
    else: left = mid+1
```

---

## ⏱ Complexity

| Case | Time | Space |
|------|------|-------|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

---

## 🧪 Problems

### Easy
- [Search Insert Position](https://leetcode.com/problems/search-insert-position/)
- [Binary Search](https://leetcode.com/problems/binary-search/)

### Medium
- [Find First and Last Position](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
- [Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
- [Find Peak Element](https://leetcode.com/problems/find-peak-element/)
- [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)
- [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)

### Hard
- [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)
- [Find in Mountain Array](https://leetcode.com/problems/find-in-mountain-array/)

---

## ❌ Common Mistakes

- `mid = (left+right)//2` can overflow in Java/C++ → use `left + (right-left)//2`
- Off-by-one: `left <= right` vs `left < right`
- Wrong boundary update direction
- Forgetting sorted precondition

---

## 🔗 Related Topics

- [[Array]] — applied to sorted arrays
- [[Divide and Conquer]] — same principle
- [[Two Pointers]] — related pattern
- [[Ternary Search]] — for unimodal functions

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
