# ⚙️ Sliding Window

#Algorithm #Pattern #SlidingWindow

⬅️ [[Two Pointers]] | ➡️ [[Prefix Sum]]

---

## 📌 Definition

**Sliding Window** maintains a window (subarray/substring) that slides over the data. Avoids recalculating everything from scratch by adding new elements and removing old ones.

---

## ⚙️ How It Works

### Fixed Size Window
```python
k = 3
window_sum = sum(arr[:k])
max_sum = window_sum
for i in range(k, len(arr)):
    window_sum += arr[i] - arr[i-k]
    max_sum = max(max_sum, window_sum)
```

### Dynamic (Variable) Window
```python
left = 0
for right in range(len(s)):
    # Expand window
    window.add(s[right])
    
    # Shrink window while invalid
    while not valid(window):
        window.remove(s[left])
        left += 1
    
    # Record answer
    ans = max(ans, right - left + 1)
```

---

## 🧠 When to Use

- Contiguous subarray/substring problems
- "Find minimum/maximum subarray of size k"
- "Longest substring with condition"
- Anagram / permutation in string

---

## ⏱ Complexity

| Metric | Value |
|--------|-------|
| Time | O(n) |
| Space | O(k) or O(1) |

---

## 🧪 Problems

### Easy
- [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)
- [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)

### Medium
- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
- [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
- [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

### Hard
- [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
- [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

---

## 🔗 Related Topics

- [[Two Pointers]] — left/right form the window
- [[Hash Map]] — track character frequencies
- [[Monotonic Queue]] — max/min in window
- [[Array]] — applied to arrays
- [[String]] — applied to strings

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
