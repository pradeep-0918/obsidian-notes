# Rabin-Karp Algorithm

## 📌 Definition
A string matching algorithm that uses **rolling hashing** to efficiently find a pattern in a text, particularly effective for multiple pattern search.

## ⚙️ How It Works
1. Compute hash of pattern P and first window of text T[0..m-1]
2. Slide the window one character at a time:
   - **Rolling hash**: remove leftmost char, add new rightmost char in O(1)
   - If hashes match → verify character by character (avoid false positives)
3. Report matches

## 🧠 When to Use
- Multi-pattern search (hash all patterns)
- Plagiarism detection
- DNA sequence matching
- 2D pattern matching

## 🔥 Key Properties
- Rolling hash: O(1) window slide
- Spurious hits: hash collision without actual match → verify
- Average O(n+m), worst O(n·m) due to collisions

## ⏱ Complexity
| Case | Time |
|------|------|
| Average | O(n+m) |
| Worst | O(n·m) |
| Space | O(1) |

## 🆚 Comparison
| Algorithm | Multi-pattern | Time (avg) |
|-----------|-------------|-----------|
| [[KMP Algorithm]] | ❌ | O(n+m) |
| Rabin-Karp | ✅ | O(n+m) |
| [[Suffix Tree]] | ✅ | O(n+m) |

## 🧪 Problems
- [[DSA Problem Bank#Find the Index of the First Occurrence in a String]]
- [[DSA Problem Bank#Longest Duplicate Substring]]
- [[DSA Problem Bank#Repeated DNA Sequences]]

## ❌ Mistakes
- Integer overflow in hash computation (use modular arithmetic)
- Forgetting to verify after hash match (false positives)

## 🔗 Related Topics
[[KMP Algorithm]] | [[String Matching Algorithms]] | [[String]] | [[Hash Table]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #String #PatternMatching #Algorithm
