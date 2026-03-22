# 🔢 Number System
> Part of [[01_Quant_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** High

---

## 🧠 Concept

Numbers are classified as:
- **Natural:** 1, 2, 3, ...
- **Whole:** 0, 1, 2, 3, ...
- **Integers:** ...-2, -1, 0, 1, 2, ...
- **Rational:** p/q form (fractions, terminating decimals)
- **Irrational:** √2, π, e (non-terminating, non-repeating)
- **Real:** All rational + irrational

---

## 📌 Key Concepts

### Divisibility Rules (MEMORIZE!)
| Divisor | Rule |
|---|---|
| 2 | Last digit even |
| 3 | Sum of digits divisible by 3 |
| 4 | Last 2 digits divisible by 4 |
| 5 | Last digit 0 or 5 |
| 6 | Divisible by 2 AND 3 |
| 7 | (Complex — better to divide) |
| 8 | Last 3 digits divisible by 8 |
| 9 | Sum of digits divisible by 9 |
| 10 | Last digit 0 |
| 11 | (Alternating sum of digits) divisible by 11 |

### Factors & Multiples
- **Factor (divisor):** Divides the number exactly
- **Prime number:** Exactly 2 factors (1 and itself)
- **Composite:** More than 2 factors

### Number of Factors
If N = 2^a × 3^b × 5^c..., then:
$$\text{Number of factors} = (a+1)(b+1)(c+1)...$$

### Sum of All Factors
$$\text{Sum} = \frac{(2^{a+1}-1)}{1} \times \frac{(3^{b+1}-1)}{2} \times ...$$

---

## 🔥 Important Patterns

### Units Digit Patterns
- Powers of 2: 2,4,8,6,2,4,8,6... (cycle of 4)
- Powers of 3: 3,9,7,1,3,9,7,1... (cycle of 4)
- Powers of 7: 7,9,3,1,7,9,3,1... (cycle of 4)
- Powers of 4: 4,6,4,6... (cycle of 2)
- Powers of 9: 9,1,9,1... (cycle of 2)
- Powers of 5: always 5
- Powers of 6: always 6

**To find units digit of 7^55:** 55 mod 4 = 3 → 3rd in cycle = **3**

### Remainder Theorems
**Remainder when divided by 9:** = Remainder when digit sum divided by 9
**Remainder when divided by 11:** = Remainder when alternating sum divided by 11

### Factorials and Trailing Zeros
Trailing zeros in n! = number of times 10 divides n! = min(powers of 2, powers of 5 in n!)
Since powers of 5 < powers of 2:
$$\text{Trailing zeros in n!} = \lfloor n/5 \rfloor + \lfloor n/25 \rfloor + \lfloor n/125 \rfloor + ...$$

> 100! trailing zeros = 20+4+0 = **24**

---

## 💡 Key Shortcuts

### Quick Remainder Tricks
- a^n mod (a+1) = 1^n mod(a+1) = 1 (when n is even) or a
- 2^10 = 1024 ≡ 1024 mod 1000 = 24

### Perfect Squares Ending Patterns
- Perfect squares can ONLY end in: 0, 1, 4, 5, 6, 9
- Can NEVER end in: 2, 3, 7, 8

### Sum Formulas
- Sum of 1 to n = n(n+1)/2
- Sum of squares = n(n+1)(2n+1)/6
- Sum of cubes = [n(n+1)/2]²

---

## 🧩 Practice Questions

### Easy
1. Sum of first 10 odd numbers? → n² = 10² = **100**
2. Place value of 5 in 3527? → **500**
3. Remainder of 25÷4? → **1**
4. Sum of first 8 natural numbers? → 8×9/2 = **36**
5. Quotient of 56÷7? → **8**

### Medium
6. Find number of factors of 72?
   > 72 = 2³×3²; Factors = (3+1)(2+1) = **12**
7. Units digit of 7^53?
   > 53 mod 4 = 1; First in cycle = **7**
8. Trailing zeros in 50!?
   > 10+2+0 = **12**
9. Is 561 divisible by 3? → 5+6+1=12, divisible by 3 → **Yes**
10. Units digit of 3^100? → 100 mod 4 = 0 → 4th in cycle = **1**

### Hard
11. Remainder when 2^100 is divided by 100?
    > 2^10=1024≡24(mod 100); 2^20≡576≡76; 2^40≡76²≡5776≡76; 2^80≡76; 2^100=2^80×2^20≡76×76=5776≡**76**
12. For N=2^a×3^b: N has 12 factors, a>b. Min value of N?
    > 12 = 12×1 or 6×2 or 4×3 or 3×2×2; (a+1)(b+1)=12; Try (5+1)(1+1)=12: N=2^5×3=96; Try (3+1)(2+1)=12: N=2^3×3²=72; Min with a>b: N=2^3×3²=72 but a=3,b=2 ok → **72**
13. Last 2 digits of 7^80?
    > 7^1=07,7^2=49,7^3=43,7^4=01; Cycle 4; 80 mod 4=0 → 4th=**01**

---

## 🔗 Related Topics
- [[QA_04_HCF_LCM]] — Factors and multiples
- [[QA_13_Percentage]] — Number properties
- [[QA_08_Permutations_Combinations]] — Counting principles

---

*⬅️ [[QA_14_Mixtures_Alligations]] | ➡️ [[01_Quant_Index]]*
