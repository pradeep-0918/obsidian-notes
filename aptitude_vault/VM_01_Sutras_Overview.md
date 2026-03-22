# 📜 VM-01: All 16 Vedic Sutras — Complete Overview
> Part of [[VM_00_Vedic_Index]] | 🔙 [[00_Master_Index]]

---

## 🕉️ Introduction

The word **"Sutra"** means *thread* in Sanskrit — a concise rule that threads through many applications. Each Vedic Sutra is a single Sanskrit phrase that unlocks a family of calculation techniques.

**Key mindset:** Vedic Maths works by exploiting **patterns in numbers** that conventional arithmetic ignores.

---

## 📌 Sutra 1: Ekadhikena Purvena
### *"By one more than the previous one"*

**Core Idea:** When a number ends in 5, or when denominators of certain fractions are processed, use the digit one more than the previous digit.

### Application 1: Squaring numbers ending in 5
> Rule: n5² = n(n+1) | 25
> "Multiply n by (n+1), append 25"

| Number | n | n+1 | n×(n+1) | Result |
|---|---|---|---|---|
| 15² | 1 | 2 | 1×2=2 | **225** |
| 25² | 2 | 3 | 2×3=6 | **625** |
| 35² | 3 | 4 | 3×4=12 | **1225** |
| 45² | 4 | 5 | 4×5=20 | **2025** |
| 55² | 5 | 6 | 5×6=30 | **3025** |
| 65² | 6 | 7 | 6×7=42 | **4225** |
| 75² | 7 | 8 | 7×8=56 | **5625** |
| 85² | 8 | 9 | 8×9=72 | **7225** |
| 95² | 9 | 10 | 9×10=90 | **9025** |
| 105² | 10 | 11 | 10×11=110 | **11025** |

> 💡 **Exam hack:** You see 75² on the paper — immediately write **5625**. Zero calculation needed once memorized.

### Application 2: Multiplying numbers ending in 5 with 5-apart partner
> 45 × 55 = (5²−1²) × 100 / ... actually use: 45×55 = (50−5)(50+5) = 50²−5² = 2500−25 = **2475**

---

## 📌 Sutra 2: Nikhilam Navatashcaramam Dashatah
### *"All from 9 and the last from 10"*

**Core Idea:** To subtract from a power of 10 (10, 100, 1000...), subtract every digit from 9 except the last digit which is subtracted from 10.

> **Finding complement:** 100 − 37 = ?
> 3 → 9−3 = 6; 7 → 10−7 = 3 → **63** ← instant!
> 1000 − 456 = ? 4→5, 5→4, 6→4 → **544**

### Primary Use: Multiplication near base
→ See full treatment in [[VM_03_Multiplication_Core]]

---

## 📌 Sutra 3: Urdhva-Tiryagbyham
### *"Vertically and Crosswise"*

**The most powerful and general sutra.** Works for ANY multiplication.

### 2-digit × 2-digit example: 23 × 45
```
Step 1 (Vertical right): 3×5 = 15 → write 5, carry 1
Step 2 (Crosswise): 2×5 + 3×4 = 10+12 = 22+1(carry) = 23 → write 3, carry 2
Step 3 (Vertical left): 2×4 = 8+2(carry) = 10

Result: 10 | 3 | 5 = 1035
```
→ See full treatment in [[VM_03_Multiplication_Core]]

---

## 📌 Sutra 4: Paravartya Yojayet
### *"Transpose and Apply"*

**Core Idea:** Move terms to the other side of an equation with sign change. Also used in division as "remainder by transposition."

### In Division: 1234 ÷ 12
→ See [[VM_05_Division]] for full treatment

### In Equations:
> 3x + 7 = 22 → 3x = 22−7 = 15 → x = 5
> (Transposing 7 means changing its sign)

---

## 📌 Sutra 5: Shunyam Saamyasamuccaye
### *"When the sum is the same, that sum is zero"*

**Core Idea:** When both sides of an equation have the same "samuccaya" (sum/common factor), that samuccaya = 0.

### Example 1: Linear equation
> (x+3)(x+5) = (x+2)(x+6)
> Left sum: 3+5=8; Right sum: 2+6=8 → Same!
> By sutra: x+8 = 0 → **x = −8** ✓ (verify: (−5)(−3) = (−6)(−2) = 15 ✓)

### Example 2: Fraction equations
> 1/(x+2) + 1/(x+3) = 1/(x+4) + 1/(x+1)
> Check: 2+3=5 = 4+1=5 → x+5 = 0 → **x = −5**

---

## 📌 Sutra 6: Anurupyena
### *"Proportionality"*

**Core Idea:** Scale a number to a nearby base, compute, then scale back.

### Multiplication near multiple of base:
> 212 × 213 → Base = 200 (= 2 × 100)
→ See [[VM_04_Multiplication_Advanced]]

---

## 📌 Sutra 7: Sankalana-Vyavakalanabhyam
### *"By Addition and Subtraction"*

**Core Idea:** Solve simultaneous equations by adding and subtracting them.

### Example:
> 3x + 4y = 43 ... (i)
> 4x + 3y = 43 ... (ii)
> Add: 7x + 7y = 86 → x + y = 86/7 ≈ 12.28
> Subtract: −x + y = 0 → **x = y**
> So 7x = 86/2... wait: if x=y, then 3x+4x = 43 → 7x = 43 → x = 43/7
> Better example: 3x+4y=11, 4x+3y=10; Add: 7x+7y=21→x+y=3; Subtract: −x+y=1→y=2,x=1

---

## 📌 Sutra 8: Puranapuranabhyam
### *"By Completion or Non-Completion"*

**Core Idea:** Complete the square or cube to solve quadratics/cubics.

### Example: x² + 6x + 5 = 0
> Complete square: (x+3)² − 9 + 5 = 0 → (x+3)² = 4 → x+3 = ±2
> x = **−1** or x = **−5**

---

## 📌 Sutra 9: Chalana-Kalanabyham
### *"Differences and Similarities"*

Used in factorization. When the product of extremes = product of means (ratio).

---

## 📌 Sutra 10: Yavadunam
### *"Whatever the extent of the deficiency"*

**Core Idea:** Square numbers near a base using the deficiency.

> 97² (near base 100, deficiency = 3)
> Left part: 97 − 3 = 94
> Right part: 3² = 09
> Answer: **9409**

> 104² (near base 100, excess = 4)
> Left: 104 + 4 = 108
> Right: 4² = 16
> Answer: **10816**

→ See full treatment in [[VM_06_Squares_Cubes]]

---

## 📌 Sutra 11: Vyashtisamanstih
### *"Specific and General"*

Used in factorization — separating specific numeric values from general variables.

---

## 📌 Sutra 12: Shesanyankena Caramena
### *"Remainders by the Last Digit"*

Used in converting fractions to decimals — the recurring pattern is found using remainders.

> 1/7 = 0.142857... (recurring)
→ See [[VM_08_Fractions_Decimals]]

---

## 📌 Sutra 13: Sopaantyadvayamantyam
### *"The Ultimate and Twice the Penultimate"*

Used in solving specific types of fraction equations.

> 1/((x+2)(x+3)) = 1/((x+1)(x+4)) type problems

---

## 📌 Sutra 14: Ekanyunena Purvena
### *"By One Less than the Previous One"*

**Core Idea:** Multiply by 9, 99, 999, etc., using "one less."

### Multiplying by 9:
> 7 × 9 = 7(10−1) = 70−7 = **63**
> 43 × 9 = 43(10−1) = 430−43 = **387**
> 43 × 99 = 43(100−1) = 4300−43 = **4257**
> 43 × 999 = 43(1000−1) = 43000−43 = **42957**

### The Vedic Way (Ekanyunena):
> N × 9: Write (N−1) then (10−N's last digit complement)
> 43 × 9: Left = 43−1=42, Right = 10−3=7 → **387** ✓
> 
> N × 99: Write (N−1) then (100 − N)
> 43 × 99: 43−1=42 | 100−43=57 → **4257** ✓
>
> N × 999: Write (N−1) then (1000 − N)
> 43 × 999: 42 | 957 → **42957** ✓

---

## 📌 Sutra 15: Gunitasamuchyah
### *"The Product of the Sum is Equal to the Sum of the Product"*

**Core Idea (Digit Sum Verification):** If your multiplication is correct, the digit sum of the product = digit sum of the (digit sum of factors multiplied together).

### Example: 23 × 45 = 1035
> Digit sum of 23 = 2+3 = 5
> Digit sum of 45 = 4+5 = 9
> 5 × 9 = 45 → digit sum = 4+5 = **9**
> Digit sum of 1035 = 1+0+3+5 = **9** ✓ Correct!

→ See full treatment in [[VM_10_Verification_Checks]]

---

## 📌 Sutra 16: Gunakasamuchyah
### *"The Factors of the Sum is Equal to the Sum of the Factors"*

Used to verify algebraic factorizations.

---

## 🔗 Next Steps
- [[VM_02_Addition_Subtraction]] — Start building speed here
- [[VM_03_Multiplication_Core]] — The most exam-relevant techniques
- [[VM_00_Vedic_Index]] — Back to Vedic index

---

*⬅️ [[VM_00_Vedic_Index]] | ➡️ [[VM_02_Addition_Subtraction]]*
