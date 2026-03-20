# String DP

## 📌 Definition
Dynamic programming on one or two string inputs. State typically represents prefixes or substrings.

## ⚙️ How It Works
- Two strings: `dp[i][j]` = answer for `s1[0..i-1]` and `s2[0..j-1]`
- One string: `dp[i][j]` = answer for `s[i..j]`
- Transitions depend on whether characters match: if `s1[i]==s2[j]`, diagonal; else from adjacent states

## 🧠 When to Use
- Edit distance, LCS, palindrome subproblems
- Interleaving, distinct subsequences
- Any substring/subsequence optimisation

## 🧪 Problems
- [[DSA Problem Bank#Longest Common Subsequence]]
- [[DSA Problem Bank#Edit Distance]]
- [[DSA Problem Bank#Distinct Subsequences]]
- [[DSA Problem Bank#Interleaving String]]
- [[DSA Problem Bank#Longest Palindromic Subsequence]]

## ❌ Mistakes
- Not handling base cases (empty prefix = row/col 0)
- Confusing subsequence (non-contiguous) vs substring (contiguous)

## 🔗 Related Topics
[[Dynamic Programming]] | [[String]] | [[Two Pointers]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #String #Algorithm
