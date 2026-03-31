# 🎲 Permutations & Combinations
> Part of [[01_Quant_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐⭐⭐ Hard | **Exam Weight:** Medium

---

## 🧠 Concept

**Permutation:** Arrangement where ORDER matters (ABC ≠ BAC)
**Combination:** Selection where order DOESN'T matter (ABC = BAC = CAB)

> Mnemonic: **P**ermutation = **P**osition matters | **C**ombination = just **C**hoose

---

## 📌 Key Formulas

### Factorial
$$n! = n \times (n-1) \times (n-2) \times ... \times 1$$
$$0! = 1, \quad 1! = 1$$

### Permutation (arrange r from n)
$$P(n,r) = \frac{n!}{(n-r)!}$$

### Combination (choose r from n)
$$C(n,r) = \binom{n}{r} = \frac{n!}{r!(n-r)!}$$

### Relationship
$$P(n,r) = C(n,r) \times r!$$

### Circular Arrangement
$$\text{Circular arrangements of n} = (n-1)!$$

### With Repetition
$$\text{Arrangements of n with repetition} = \frac{n!}{p! \times q! \times ...}$$
> Where p, q... are counts of repeated elements

---

## 🔥 Common Patterns

### "Must be together" → Treat as one unit
> 6 books, 2 must be together: (5)! × 2! = 120 × 2 = 240

### "Must NOT be together" → Total − (together)
> Total − forced-together arrangements

### "Alternating" (men/women)
> 4 men, 4 women alternating in row: 4! × 4! = 576

### Key Identities
- C(n,0) = C(n,n) = 1
- C(n,1) = n
- C(n,r) = C(n, n-r)
- C(n,r) + C(n,r+1) = C(n+1, r+1)

---

## 🧩 Practice Questions

### Easy
1. Arrangements of 5 books? → 5! = **120**
2. Choose 3 from 6? → C(6,3) = **20**
3. Arrange 4 people in circle? → (4-1)! = **6**
4. Ways to choose 2 from 5? → C(5,2) = **10**
5. Arrange letters in CAT? → 3! = **6**

### Medium
6. 5 men, 3 women in row, women together? → (5+1)! × 3! = 720×6 = **4320**
7. 4 men, 4 women alternate? → 4! × 4! × 2 (start with M or W) = **1152** (for circle: (4-1)!×4! = 144)
8. Arrangements of APPLE? → 5!/2! = **60** (A appears... wait A once, P twice: 5!/2! = 60)
9. Committee of 3 from 5M + 4W, at least 1W?
   > Total − all men = C(9,3) − C(5,3) = 84−10 = **74**
10. 7 books, 3 specific together? → (5)! × 3! = 120×6 = **720**

### Hard
11. 6M, 4W in row, no two women together?
    > Arrange 6 men: 6! = 720; 7 gaps; choose 4 for women: C(7,4) × 4! = 35×24=840; Total = 720×840 = **604800**
12. 8 students, 3 together, 2 cannot be together?
    > Complex — treat 3 as unit (6 entities), arrange = 6!×3! = 4320; subtract cases where the 2 are also together in this arrangement
13. Letters of MISSISSIPPI arranged? → 11!/(4!×4!×2!) = **34650**

---

## 🔗 Related Topics
- [[QA_12_Probability]] — P&C is the foundation of probability
- [[03_Formula_Sheet]] — All formulas

---

*⬅️ [[QA_07_Profit_Loss]] | ➡️ [[QA_09_Ratio_Proportion]]*
