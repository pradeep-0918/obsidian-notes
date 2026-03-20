# 🔤 String
#DataStructure #Linear #String

⬅️ [[Sparse Array]] | ➡️ [[Linked List]]

---

## 📌 Definition
A **string** is a sequence of characters. In most languages, strings are **immutable** arrays of characters with built-in operations.

---

## ⚙️ How It Works
- Internally stored as a char array
- Null-terminated in C (`\0`)
- Immutable in Java, Python (new object on modification)
- Mutable in C++, JS (StringBuilder in Java)

---

## 🧠 When to Use
- Text processing, parsing
- Pattern matching
- Encoding/decoding

---

## 🔥 Key Properties
- Immutability affects performance (concatenation)
- Indexing is O(1) for random access
- String comparison is O(n)

---

## ⏱ Complexity

| Operation | Time |
|---|---|
| Access char | O(1) |
| Concatenation | O(n) per op (O(n²) repeated) |
| Substring | O(k) |
| Search | O(n·m) naive, O(n) with [[KMP]] |

---

## 🔥 Interview Patterns
1. **Two Pointers** — palindrome check, reverse
2. **Sliding Window** — longest substring
3. **Hash Map** — anagram, frequency count
4. **Trie** — prefix matching
5. **DP** — edit distance, LCS

---

## 🧪 Problems
- [ ] [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) — Easy
- [ ] [Is Subsequence](https://leetcode.com/problems/is-subsequence/) — Easy
- [ ] [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) — Easy
- [ ] [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) — Medium
- [ ] [Group Anagrams](https://leetcode.com/problems/group-anagrams/) — Medium
- [ ] [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) — Hard
- [ ] [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/) — Hard

---

## ❌ Common Mistakes
- Using `+` for repeated concatenation (O(n²)) — use StringBuilder
- Forgetting case sensitivity
- Not handling empty string edge case

---

## 🔗 Related Topics
- [[Two Pointers]]
- [[Sliding Window]]
- [[Hash Map]]
- [[Trie]]
- [[KMP]]
- [[Dynamic Programming]] — String DP

---

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered
