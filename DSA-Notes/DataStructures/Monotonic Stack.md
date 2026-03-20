# 📦 Monotonic Stack

#DataStructure #Linear #Stack #Pattern

⬅️ [[Stack]] | ➡️ [[Monotonic Queue]]

---

## 📌 Definition

A **Monotonic Stack** is a stack that maintains elements in monotonically increasing or decreasing order. Elements that violate the property are popped before inserting.

---

## ⚙️ Templates

### Monotonic Decreasing (Next Greater Element)
```python
stack = []
result = [-1] * n
for i in range(n):
    while stack and arr[stack[-1]] < arr[i]:
        idx = stack.pop()
        result[idx] = arr[i]  # arr[i] is next greater
    stack.append(i)
```

### Monotonic Increasing (Next Smaller Element)
```python
stack = []
result = [-1] * n
for i in range(n):
    while stack and arr[stack[-1]] > arr[i]:
        idx = stack.pop()
        result[idx] = arr[i]  # arr[i] is next smaller
    stack.append(i)
```

---

## 🧠 When to Use

- Next greater/smaller element
- Largest rectangle in histogram
- Daily temperatures
- Span/range problems

---

## 🧪 Problems

### Easy
- [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

### Medium
- [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)
- [Online Stock Span](https://leetcode.com/problems/online-stock-span/)
- [132 Pattern](https://leetcode.com/problems/132-pattern/)

### Hard
- [Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

---

## 🔗 Related Topics

- [[Stack]] — underlying structure
- [[Monotonic Queue]] — deque variant
- [[Array]] — usually applied to arrays

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
