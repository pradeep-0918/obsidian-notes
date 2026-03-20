# 📦 Monotonic Queue

#DataStructure #Linear #Queue #Pattern

⬅️ [[Monotonic Stack]] | ➡️ [[Hash Table]]

---

## 📌 Definition

A **Monotonic Queue** (using [[Deque]]) maintains elements in monotonic order while supporting O(1) max/min queries over a sliding window.

---

## ⚙️ Template (Sliding Window Maximum)

```python
from collections import deque

def sliding_window_max(arr, k):
    dq = deque()  # stores indices, decreasing values
    result = []
    
    for i, x in enumerate(arr):
        # Remove elements outside window
        while dq and dq[0] < i - k + 1:
            dq.popleft()
        
        # Maintain decreasing order
        while dq and arr[dq[-1]] < x:
            dq.pop()
        
        dq.append(i)
        
        if i >= k - 1:
            result.append(arr[dq[0]])
    
    return result
```

---

## 🧠 When to Use

- Sliding window max/min in O(n)
- DP optimization (Jump Game VI)

---

## 🧪 Problems

### Hard
- [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)
- [Jump Game VI](https://leetcode.com/problems/jump-game-vi/)
- [Max Value of Equation](https://leetcode.com/problems/max-value-of-equation/)

---

## 🔗 Related Topics

- [[Deque]] — underlying structure
- [[Sliding Window]] — applied pattern
- [[Monotonic Stack]] — stack variant

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
