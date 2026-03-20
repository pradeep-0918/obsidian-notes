# 📦 Suffix Array

#DataStructure #String #Advanced

⬅️ [[Rope]] | ➡️ [[Suffix Tree]]

---

## 📌 Definition

A **Suffix Array** is a sorted array of all suffixes of a string. Combined with LCP (Longest Common Prefix) array, enables powerful string operations.

---

## ⚙️ Example

```
String: "banana"
Suffixes sorted:
  "a"
  "ana"
  "anana"
  "banana"
  "na"
  "nana"

SA = [5, 3, 1, 0, 4, 2]
```

---

## ⏱ Complexity

| Operation | Time |
|-----------|------|
| Build (naive) | O(n² log n) |
| Build (DC3/SA-IS) | O(n) |
| Pattern matching | O(m log n) |

---

## 🧪 Problems

- [Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)

---

## 🔗 Related Topics

- [[Suffix Tree]] — more powerful, more space
- [[String]] — applied to strings
- [[Binary Search]] — pattern matching on SA

---

## ✅ Status

- [ ] Learned
- [ ] Practiced
- [ ] Mastered
