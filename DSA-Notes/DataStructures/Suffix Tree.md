# Suffix Tree

## 📌 Definition
A compressed trie of all suffixes of a given string. Allows O(m) pattern matching (m = pattern length) after O(n) or O(n log n) construction.

## ⚙️ How It Works
- Build by inserting all suffixes of string S into a trie
- Compress chains of single-child nodes into single edges
- Each leaf represents a suffix; edge labels are substrings
- Ukkonen's algorithm builds in O(n) time

## 🧠 When to Use
- Substring search in O(m) time
- Longest common substring between two strings
- Count occurrences of a pattern
- Bioinformatics (genome sequencing)

## 🔥 Key Properties
- O(n) space (compressed)
- O(m) pattern match after build
- Generalised Suffix Tree: works over multiple strings

## ⏱ Complexity
| Operation | Time |
|-----------|------|
| Build | O(n) |
| Search | O(m) |
| Space | O(n) |

## 🆚 Comparison
| Structure | Build | Search | Space |
|-----------|-------|--------|-------|
| [[Suffix Array]] | O(n log n) | O(m log n) | O(n) |
| Suffix Tree | O(n) | O(m) | O(n) (large constant) |
| [[Trie]] | O(n²) | O(m) | O(n²) |

## 🧪 Problems
- [[DSA Problem Bank#Longest Duplicate Substring]]
- [[DSA Problem Bank#Shortest Palindrome]]

## ❌ Mistakes
- Confusing Suffix Tree with [[Trie]] (Suffix Tree is compressed)
- Forgetting the sentinel character '$' to make all suffixes unique

## 🔗 Related Topics
[[Suffix Array]] | [[Trie]] | [[String]] | [[KMP Algorithm]]

## ⬅️ Previous
[[Rope]]

## ➡️ Next
[[KD Tree]]

## ✅ Status
- [ ] Learned
- [ ] Practiced
- [ ] Mastered

#DSA #SuffixTree #AdvancedDS #DataStructure
