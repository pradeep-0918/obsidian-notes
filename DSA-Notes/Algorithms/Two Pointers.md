# ⚙️ Two Pointers

#Algorithm #Pattern #TwoPointers

⬅️ [[Algorithms Index]] | ➡️ [[Sliding Window]]

---

## 📌 Definition

**Two Pointers** uses two indices (usually `left` and `right`) that move toward each other or in the same direction to solve array/string problems efficiently.

---

## ⚙️ How It Works

### Pattern 1: Converging Pointers (sorted array)
```python
left, right = 0, len(arr) - 1
while left < right:
    if condition: left += 1
    elif condition: right -= 1
    else: # found answer
```

### Pattern 2: Same Direction (fast/slow)
```python
slow = 0
for fast in range(len(arr)):
    if condition:
        arr[slow] = arr[fast]
        slow += 1
```

### Pattern 3: Two Arrays
```python
i, j = 0, 0
while i < len(a) and j < len(b):
    if a[i] < b[j]: i += 1
    else: j += 1
```

---

## 🧠 When to Use

- Sorted array + target sum
- Remove duplicates in-place
- Palindrome check
- Container with most water
- Merge two sorted arrays

---

## ⏱ Complexity

| Metric | Value |
|--------|-------|
| Time | O(n) |
| Space | O(1) |

---

## 🧪 Problems

### Easy
- [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)
- [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/)
- [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

### Medium
- [3Sum](https://leetcode.com/problems/3sum/)
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)
- [Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

### Hard
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

---

## 🔗 Related Topics

- [[Sliding Window]] — window variant
- [[Fast and Slow Pointers]] — cycle detection
- [[Binary Search]] — sorted array patterns
- [[Array]] — primary application

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
