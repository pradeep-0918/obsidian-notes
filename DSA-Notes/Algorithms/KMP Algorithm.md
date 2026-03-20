# KMP Algorithm (Knuth-Morris-Pratt)

## 📌 Definition
A linear-time string matching algorithm that uses a preprocessed **failure function** (partial match table) to skip redundant comparisons when a mismatch occurs.

## ⚙️ How It Works
1. **Preprocess**: Build LPS (Longest Proper Prefix which is also Suffix) array for pattern P
   - LPS[i] = length of longest proper prefix of P[0..i] that is also a suffix
2. **Search**: Slide pattern over text; on mismatch, use LPS to skip ahead (don't restart from 0)

## 🧠 When to Use
- Single pattern search in O(n+m) time
- Repeated searching of the same pattern
- Finding all occurrences of a pattern

## 🔥 Key Properties
- Never backtracks in the text
- LPS preprocessing: O(m)
- Search: O(n)
- Total: O(n+m)

## ⏱ Complexity
| Phase | Time |
|-------|------|
| Preprocess | O(m) |
| Search | O(n) |
| Space | O(m) |

## 🆚 Comparison
| Algorithm | Time | Preprocessing | Use Case |
|-----------|------|--------------|---------|
| Naive | O(n·m) | None | Simple small patterns |
| KMP | O(n+m) | O(m) | Single pattern |
| [[Rabin-Karp Algorithm]] | O(n+m) avg | O(m) | Multiple patterns / rolling hash |
| [[Suffix Tree]] | O(n+m) | O(n) | Many patterns |

## 🧪 Problems
- [[DSA Problem Bank#Find the Index of the First Occurrence in a String]]
- [[DSA Problem Bank#Repeated Substring Pattern]]
- [[DSA Problem Bank#Shortest Palindrome]]

## ❌ Mistakes
- Off-by-one in LPS array construction
- Confusing LPS[i] with the prefix length vs index

## 🔗 Related Topics
[[String]] | [[Rabin-Karp Algorithm]] | [[String Matching Algorithms]] | [[Suffix Array]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #String #PatternMatching #Algorithm
