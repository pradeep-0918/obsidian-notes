# 🪢 Rope
#DataStructure #Advanced #Rare #String

⬅️ [[Skip List]] | ➡️ [[Suffix Array]]

---

## 📌 Definition
A **rope** is a binary tree where each leaf node holds a short string and each internal node holds the length of the string in its left subtree. Used for efficient string manipulation.

---

## ⚙️ How It Works
```
         (13)
        /    \
      (6)    (7)
      / \    / \
  "Hello" " " "World!"
```
- Concatenation: O(1) — create new root
- Split: O(log n)
- Index: O(log n)

---

## 🧠 When to Use
- Text editors (large strings with frequent edits)
- Collaborative editing
- When repeated concatenation needed (vs O(n²) naive)

---

## ⏱ Complexity

| Operation | Time |
|---|---|
| Concat | O(1) |
| Split | O(log n) |
| Index | O(log n) |
| Insert | O(log n) |
| Delete | O(log n) |

---

## 🔗 Related Topics
- [[String]]
- [[Binary Tree]]

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
