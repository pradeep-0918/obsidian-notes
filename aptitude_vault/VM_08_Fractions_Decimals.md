# 🔢 VM-08: Fractions & Decimals + VM-10: Verification
> Part of [[VM_00_Vedic_Index]] | 🔙 [[00_Master_Index]]

---

# PART A: Fractions & Recurring Decimals

## 📌 1. Ekadhikena for Recurring Decimals

### 1/19, 1/29, 1/39... (denominator ends in 9)

**For 1/19:** Ekadhikena = 1+1 = 2. Divide by 2 repeatedly (working right to left)

**Shortcut table for common fractions:**

| Fraction | Decimal |
|---|---|
| 1/7 | 0.142857142857... |
| 1/11 | 0.090909... |
| 1/13 | 0.076923076923... |
| 1/14 | 0.0714285... |
| 1/17 | 0.0588235294117647... |
| 1/19 | 0.052631578947368421... |
| 2/7 | 0.285714... |
| 3/7 | 0.428571... |

### The 1/7 family (memorize cyclic pattern):
> 1/7 = 0.**142857**... (cycle of 6 digits: 142857)
> 2/7 = 0.**285714**... (start from 2 in cycle)
> 3/7 = 0.**428571**... (start from 4)
> 4/7 = 0.**571428**... (start from 5)
> 5/7 = 0.**714285**... (start from 7)
> 6/7 = 0.**857142**... (start from 8)

**Memory trick:** 142857 × 7 = 999999

---

## 📌 2. Converting Recurring Decimals to Fractions

### Rule: For 0.abcabc... (repeating abc):
> Fraction = abc / 999

| Decimal | Fraction |
|---|---|
| 0.333... = 0.3̄ | 3/9 = **1/3** |
| 0.666... = 0.6̄ | 6/9 = **2/3** |
| 0.142857... | 142857/999999 = **1/7** |
| 0.272727... | 27/99 = **3/11** |
| 0.142142... | 142/999 |

### For mixed: 0.1333... = 0.13̄
> = (13−1)/90 = 12/90 = **2/15**

### General rule:
- Pure recurring (all digits repeat): Divide by (10^period − 1)
- Mixed recurring: (Full number − Non-repeating part) / (10^full − 10^non-repeating)

---

## 📌 3. Quick Fraction Comparison

### Method 1: Cross multiply
> 3/7 vs 4/9: 3×9=27 vs 4×7=28 → 27<28 → **3/7 < 4/9**

### Method 2: Convert to decimals (for common fractions):
> Use the memorized table above

### Method 3: Subtract and check sign:
> a/b vs c/d: Compare ad vs bc

---

# PART B: VM-10 — Verification Checks

## 📌 4. The Digit Sum Method (Casting Out 9s)
### *"Gunitasamuchyah" — Most Powerful Verification Tool*

### How to find digit sum:
Sum all digits; if ≥ 10, sum again until single digit.
> 1234: 1+2+3+4=10→1+0=**1**
> 9876: 9+8+7+6=30→3+0=**3**
> Shortcut: Drop 9s and pairs summing to 9 immediately!
> 9876: drop 9→876→8+7=15→drop→6+6=drop...actually: 8+7+6=21→2+1=3

### The Verification Rule:
For A × B = C:
> Digit_sum(A) × Digit_sum(B) must equal Digit_sum(C) (both reduced to single digit)

### Examples:
**Verify: 23 × 45 = 1035**
> DS(23) = 2+3 = 5
> DS(45) = 4+5 = 9
> 5 × 9 = 45 → DS(45) = 9
> DS(1035) = 1+0+3+5 = 9 ✓ **Correct!**

**Verify: 67 × 83 = 5561**
> DS(67) = 6+7 = 13 → 4
> DS(83) = 8+3 = 11 → 2
> 4 × 2 = 8
> DS(5561) = 5+5+6+1 = 17 → 8 ✓ **Correct!**

**Catch an error: Is 47 × 53 = 2492?**
> DS(47) = 11 → 2
> DS(53) = 8
> 2 × 8 = 16 → 7
> DS(2492) = 2+4+9+2 = 17 → 8
> 7 ≠ 8 → **ERROR! Correct answer is 2491**

---

## 📌 5. Digit Sum for Addition & Subtraction

**Addition:** DS(A+B) = DS(A) + DS(B) [mod 9]
> 1234 + 5678 = 6912
> DS: 1 + 8 = 9 → 0; DS(6912) = 6+9+1+2=18→9→**0** ✓

**Subtraction:** DS(A−B) = DS(A) − DS(B) [if negative, add 9]
> 9876 − 1234 = 8642
> DS: 3 − 1 = 2; DS(8642) = 8+6+4+2=20→2 ✓

---

## 📌 6. Casting Out Nines — Step by Step

**Fast digit sum trick:**
1. Cross out any digit that is 9
2. Cross out any pair of digits that sum to 9 (like 3+6, 2+7, 1+8)
3. Sum remaining digits

> DS(97683): cross 9→7683; cross 7+2? No 2 here; cross 6+3=9→78; 7+8=15→6
> Answer: DS = 6

**Even faster:** Just think of the number mod 9:
> 1000 mod 9 = 1 (since 999 is divisible by 9)
> Any number mod 9 = digit sum mod 9

---

## 📌 7. The Navashesh (9's Check) — Practical Examples

### Division verification:
If A ÷ B = Q remainder R, then:
> A = B×Q + R
> DS(A) = DS(B×Q + R) = DS(B×Q) + DS(R) [mod 9]

**Verify: 1234 ÷ 43 = 28 R 30**
> DS(1234) = 1; DS(43×28+30) = DS(1204+30) = DS(1234) = 1 ✓

---

## 📌 8. Quick Percentage ↔ Fraction Check

| Check | Rule |
|---|---|
| Is answer reasonable? | 25% ≈ 1/4, so 25% of 200 ≈ 50 |
| Sanity check for % | If result > original, it increased |
| Cross-check | Work backward: SP × (100/profit%) should = CP |

---

## 🔥 Verification Practice

Verify each using digit sum (mark ✓ or ✗):
1. 34 × 56 = 1904 → DS(34)=7, DS(56)=2, 7×2=14→5; DS(1904)=14→5 ✓
2. 78 × 92 = 7176 → DS(78)=6, DS(92)=2, 6×2=12→3; DS(7176)=21→3 ✓
3. 123 × 456 = 56099 → DS(123)=6, DS(456)=6, 36→0; DS(56099)=29→2 ≠ 0 ✗ (correct: 56088)
4. 345 + 678 = 1023 → DS: 3+6=9→0; DS(1023)=6 ≠ 0 ✗ (correct: 1023: 1+0+2+3=6; DS(345)=3,DS(678)=21→3; 3+3=6 ✓ Oh wait: 3+3=6=DS(1023) ✓)
5. 999 × 888 = 887112 → DS(999)=0; 0×anything=0; DS(887112)=8+8+7+1+1+2=27→0 ✓

---

## 🔗 Related Notes
- [[VM_03_Multiplication_Core]] — Verify all multiplications
- [[VM_06_Squares_Cubes]] — Verify squares
- [[QA_15_Number_System]] — Digit sum and number properties

---

*⬅️ [[VM_07_Square_Roots_Cube_Roots]] | ➡️ [[VM_09_Algebra_Equations]]*
