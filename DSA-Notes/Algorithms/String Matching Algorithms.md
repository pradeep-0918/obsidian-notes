# String Matching Algorithms

## 📌 Definition
Algorithms for finding occurrences of a pattern string within a larger text string.

## 🧠 Algorithm Comparison
| Algorithm | Time (avg) | Preprocessing | Multi-pattern | Notes |
|-----------|-----------|--------------|--------------|-------|
| Naive | O(n·m) | None | ❌ | Simple, slow |
| [[KMP Algorithm]] | O(n+m) | O(m) LPS | ❌ | No backtrack |
| [[Rabin-Karp Algorithm]] | O(n+m) | O(m) hash | ✅ | Rolling hash |
| [[Suffix Tree]] | O(n+m) | O(n) | ✅ | Complex build |
| [[Suffix Array]] | O(m log n) | O(n log n) | ✅ | Space efficient |
| Aho-Corasick | O(n+m+z) | O(m) | ✅ | Best multi-pattern |

## 🧠 When to Use Which
- **Single pattern, simple**: Naive or KMP
- **Multiple patterns**: Rabin-Karp or Aho-Corasick
- **Repeated queries on same text**: Suffix Tree / Suffix Array
- **Approximate matching**: Dynamic Programming

## 🧪 Problems
- [[DSA Problem Bank#Find the Index of the First Occurrence in a String]]
- [[DSA Problem Bank#Repeated DNA Sequences]]
- [[DSA Problem Bank#Shortest Palindrome]]
- [[DSA Problem Bank#Longest Duplicate Substring]]

## 🔗 Related Topics
[[KMP Algorithm]] | [[Rabin-Karp Algorithm]] | [[Suffix Tree]] | [[Suffix Array]] | [[String]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #String #PatternMatching #Algorithm
