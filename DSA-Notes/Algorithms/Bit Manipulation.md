# Bit Manipulation

## 📌 Definition
Techniques that directly operate on the binary representation of integers using bitwise operators. Often used to achieve O(1) space and speed up operations significantly.

## ⚙️ Core Operations
| Operation | Syntax | Effect |
|-----------|--------|--------|
| AND | `a & b` | 1 if both bits 1 |
| OR | `a \| b` | 1 if either bit 1 |
| XOR | `a ^ b` | 1 if bits differ |
| NOT | `~a` | Flip all bits |
| Left Shift | `a << k` | Multiply by 2^k |
| Right Shift | `a >> k` | Divide by 2^k |

## 🔥 Common Tricks
```
Check bit k:     (n >> k) & 1
Set bit k:       n | (1 << k)
Clear bit k:     n & ~(1 << k)
Toggle bit k:    n ^ (1 << k)
Check power of 2: n & (n-1) == 0
Remove lowest set bit: n & (n-1)
Isolate lowest set bit: n & (-n)
XOR same number twice cancels: a^a = 0
```

## 🧠 When to Use
- Finding the single non-duplicate number
- Subset enumeration (bitmask DP)
- Counting set bits
- Swapping without temp variable
- Efficient multiplication/division by powers of 2

## ⏱ Complexity
- All operations: O(1) per operation
- Bitmask DP: O(2^n · n)

## 🧪 Problems
- [[DSA Problem Bank#Single Number]]
- [[DSA Problem Bank#Number of 1 Bits]]
- [[DSA Problem Bank#Counting Bits]]
- [[DSA Problem Bank#Reverse Bits]]
- [[DSA Problem Bank#Missing Number]]
- [[DSA Problem Bank#Sum of Two Integers]]
- [[DSA Problem Bank#Subsets]]

## ❌ Mistakes
- Integer overflow when shifting (use `1L << k` in Java/C++)
- Operator precedence: `&`, `|`, `^` have lower precedence than `==`
- Signed vs unsigned right shift (`>>` vs `>>>` in Java)

## 🔗 Related Topics
[[Dynamic Programming]] | [[Bitmask DP]] | [[Array]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #BitManipulation #Algorithm
