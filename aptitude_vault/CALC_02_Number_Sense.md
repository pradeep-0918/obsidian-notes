# 🔭 Number Sense — Pattern Recognition & Estimation
> Part of [[VM_00_Vedic_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

---

## 🧠 What is Number Sense?

Number sense is your **intuitive feel for numbers** — knowing instantly whether an answer is reasonable, spotting patterns, and choosing the fastest solution path.

> Elite math students don't calculate differently — they **see** numbers differently.

---

## 🔍 SECTION 1: APPROXIMATION TECHNIQUES

### 1.1 Leading Digit Approximation
```
When exact answer is not needed — use first 2 significant digits:

  47 × 83 ≈ 50 × 80 = 4000  (actual: 3901, error ≈ 2.5%)
  237 × 41 ≈ 240 × 40 = 9600  (actual: 9717, error ≈ 1.2%)
  √502 ≈ √500 ≈ 22.4  (actual: 22.4, nearly exact!)

Rule: Round to nearest "clean" number, compute, adjust if needed.
```

### 1.2 Benchmarking
```
Fix anchor numbers and work relative to them:

Is 63/127 bigger or smaller than 1/2?
  63/127 vs 63.5/127 = 1/2 → 63 < 63.5 → 63/127 < 1/2 ✓

Is 7/17 bigger or smaller than 2/5?
  Compare: 7×5=35 vs 2×17=34 → 35>34 → 7/17 > 2/5 ✓

Compare 3/7 and 5/12:
  3×12=36 vs 5×7=35 → 36>35 → 3/7 > 5/12 ✓ (very close!)
```

### 1.3 The 1% Method
```
Calculate 1%, then scale:

  37% of 425:
    1% of 425 = 4.25
    37% = 37 × 4.25
    = 40×4.25 − 3×4.25
    = 170 − 12.75 = 157.25

  23% of 650:
    1% = 6.5
    23% = 20×6.5 + 3×6.5 = 130 + 19.5 = 149.5
```

### 1.4 Fermi Estimation (for rough answers)
```
Strategy: Express everything as powers of 10 or clean fractions.

Estimate 47 × 83:
  ≈ 50 × 80 = 4000
  Adjust: 47/50=0.94, 83/80=1.0375
  Correction: 4000 × 0.94 × 1.0375 ≈ 4000 × 0.975 ≈ 3900

For exam MCQs: eliminate options far from your estimate!
```

---

## 🔍 SECTION 2: NUMBER PATTERNS TO RECOGNIZE INSTANTLY

### 2.1 Perfect Squares Quick Recognition
```
Last digit pattern — perfect squares ONLY end in:
  0, 1, 4, 5, 6, 9

NEVER end in: 2, 3, 7, 8

Quick test:
  1234567 — ends in 7 → NOT a perfect square ✓
  98765432 — ends in 2 → NOT a perfect square ✓
  10201 — ends in 1 → COULD be. Check: 101²=10201 ✓

Digital root of perfect squares: only 1, 4, 7, 9, 0
  54321: digit sum = 15 → 6 → NOT a perfect square ✓
  12544: digit sum = 16 → 7 → Could be. √12544 = 112 ✓
```

### 2.2 Divisibility Patterns — Extended
```
Divisible by 2: Last digit even
Divisible by 3: Digit sum ÷ 3
Divisible by 4: Last 2 digits ÷ 4
Divisible by 5: Last digit 0 or 5
Divisible by 6: Both 2 and 3
Divisible by 7: Remove last digit, subtract 2× that digit from rest, repeat
  343: 34 − 2×3 = 28; 28 ÷ 7 = 4 ✓ → 343 divisible by 7
  462: 46 − 2×2 = 42; 42 ÷ 7 = 6 ✓
Divisible by 8: Last 3 digits ÷ 8
Divisible by 9: Digit sum ÷ 9
Divisible by 11: Alternating sum ÷ 11
  1234: 1−2+3−4 = −2 → NO
  1562: 1−5+6−2 = 0 → YES
  2728: 2−7+2−8 = −11 → YES (−11 ÷ 11 = −1)
Divisible by 12: Both 3 and 4
Divisible by 15: Both 3 and 5
Divisible by 25: Last 2 digits = 00, 25, 50, 75
Divisible by 125: Last 3 digits ÷ 125
```

### 2.3 Power of 2 Recognition
```
Perfect powers of 2:
  2, 4, 8, 16, 32, 64, 128, 256, 512, 1024,
  2048, 4096, 8192, 16384, 32768, 65536

Test: Is 4096 a power of 2?
  4096 = 4096/2 = 2048/2 = 1024/2 = 512/2 = 256/2 = 128/2 = 64/2 = 32/2 = 16/2 = 8/2 = 4/2 = 2/2 = 1 → YES! 2^12
```

### 2.4 Number Families
```
Multiples of 9: Digit sum = 9 or 18 or 27...
  18, 27, 36, 45, 54, 63, 72, 81, 90, 99, 108, 117...
  
Multiples of 11: 11, 22, 33, 44, 55, 66, 77, 88, 99, 110, 121...
  Recognition: Two-digit numbers with same digits (11,22,...99)
  Three-digit palindromes (121, 131, 141... — all ÷11!)
  
Twin primes (pairs differing by 2):
  (3,5), (5,7), (11,13), (17,19), (29,31), (41,43), (59,61), (71,73)
```

### 2.5 Pythagorean Triples (Critical for area problems)
```
MEMORIZE these completely:

Basic:    (3, 4, 5)     → 3²+4²=5²=25 ✓
Scaled:   (6,8,10) (9,12,15) (12,16,20) (15,20,25)

Next:     (5, 12, 13)   → 25+144=169=13² ✓
Scaled:   (10,24,26) (15,36,39)

Next:     (8, 15, 17)   → 64+225=289=17² ✓
Next:     (7, 24, 25)   → 49+576=625=25² ✓
Next:     (20, 21, 29)  → 400+441=841=29² ✓
Next:     (9, 40, 41)   → 81+1600=1681=41² ✓
Next:     (11, 60, 61)  → 121+3600=3721=61² ✓
Next:     (28, 45, 53)  → 784+2025=2809=53² ✓
Next:     (33, 56, 65)  → 1089+3136=4225=65² ✓
Next:     (36, 77, 85)  → 1296+5929=7225=85² ✓

EXAM POWER: When you see sides 5, 12 — INSTANTLY know hypotenuse = 13!
```

---

## 🔍 SECTION 3: ESTIMATION & BOUNDING

### 3.1 The Squeeze Technique
```
Bound the answer between two known values:

√200 = ?
  14² = 196, 15² = 225
  So 14 < √200 < 15
  Closer to 14 (since 200 is closer to 196)
  Linear interpolation: 14 + (200−196)/(225−196) = 14 + 4/29 ≈ 14.14 ✓

∛100 = ?
  4³=64, 5³=125
  So 4 < ∛100 < 5
  Closer to 5 (since 100 is closer to 125... hmm: 100−64=36, 125−100=25, closer to 5)
  ∛100 ≈ 4.64 ✓
```

### 3.2 Option Elimination Using Bounding
```
In MCQ: if you know the answer is between 40 and 50, 
eliminate options outside that range immediately.

Example: 37% of 543 = ?
  30% of 543 = 162.9
  40% of 543 = 217.2
  37% is between these → answer is between 163 and 217
  
  Eliminate: 143, 211, 225... narrowed to answer near 200-201
  (Actual: 200.91)
```

### 3.3 Estimation of % Errors
```
When multiplying two rounded numbers:
  If A is overestimated by a% and B by b%:
  Error in A×B ≈ a+b%  (for small a,b)

  47 ≈ 50 (+6.4%), 83 ≈ 80 (-3.6%)
  Estimated 47×83 ≈ 50×80 = 4000
  Expected error: +6.4−3.6 = +2.8% → actual should be ≈ 4000/1.028 ≈ 3891
  (Actual: 3901 ✓ — close estimate)
```

---

## 🔍 SECTION 4: MENTAL CALCULATION PATHS

### 4.1 Choosing the Best Path
```
For each problem, ask:
  Can I use complement? (for subtraction from 10/100/1000)
  Can I use near-base? (for multiplication near 100)
  Can I use factoring? (break one number into easier factors)
  Can I use distributive? (split into 2 easy multiplications)
  Can I use fraction shortcut? (% as fraction instead)

Problem: 48 × 375
  Path 1: 48 × 375 = 48 × 3 × 125 = 144 × 125 = 144000/8 = 18000 ✓
  Path 2: 48 × 375 = 48 × 400 − 48 × 25 = 19200 − 1200 = 18000 ✓
  
  Path 1 is much faster! Recognize 375 = 3 × 125.
```

### 4.2 Factor Trees for Mental Math
```
Recognize useful factorizations:
  12 = 4×3 = 6×2
  15 = 5×3
  24 = 8×3 = 6×4
  36 = 9×4 = 6×6
  48 = 16×3 = 8×6
  72 = 8×9
  96 = 32×3
  125 = 5³
  144 = 12² = 16×9
  
Use these to simplify:
  144 × 25 = 144 × 100/4 = 14400/4 = 3600
  72 × 125 = 72 × 1000/8 = 72000/8 = 9000
  96 × 375 = 96 × 3 × 125 = 288 × 125 = 288000/8 = 36000
```

### 4.3 The BODMAS Speed Path
```
Always simplify before operating:

  24 × 25 + 24 × 75 = 24 × (25+75) = 24 × 100 = 2400

  36 × 47 − 36 × 37 = 36 × (47−37) = 36 × 10 = 360

  15² − 14² = (15+14)(15−14) = 29 × 1 = 29

  999² − 1 = (999+1)(999−1) = 1000 × 998 = 998000
```

---

## 🔍 SECTION 5: PATTERN RECOGNITION IN EXAM QUESTIONS

### 5.1 Recognizing Question Type from Numbers
```
When you see these numbers → think these methods:

Numbers ending in 5      → Ekadhikena squaring
Numbers near 100         → Nikhilam multiplication
Two numbers summing to 100 → (50+x)(50−x) = 50²−x²
Numbers like 37, 63      → might sum to 100 (37+63=100)
Numbers 3,4,5 or 5,12,13→ Pythagorean triple
x% and (100−x)%         → complementary percentages
n and 100/n              → useful for % ↔ fraction conversion
```

### 5.2 "Trap" Number Patterns
```
These look hard but are easy:
  
  99 × 99 = 9801  (don't multiply — it's (100−1)² = 9801)
  
  0.5 × 0.5 = 0.25  (not 0.25 but... it IS 0.25!)
  
  1.5² = 2.25  (1.5 = 3/2; (3/2)² = 9/4 = 2.25)
  
  2.5² = 6.25  (5/2)² = 25/4 = 6.25)
  
  0.999... = 1  (exact! infinite series proof)
  
  (0.1)³ = 0.001  (just move decimal 3 places)
```

### 5.3 Ratio Spotting
```
In word problems, spot when ratios simplify everything:

"A:B:C = 2:3:5, total = 100" → A=20, B=30, C=50 instantly
"A is 2/5 of B" → A:B = 2:5 → if B=250, A=100

When two ratios share a term:
  A:B = 3:4, B:C = 2:5
  Make B same: A:B = 6:8, B:C = 8:20
  A:B:C = 6:8:20 = 3:4:10
```

---

## 🔍 SECTION 6: BRAIN TRAINING PATTERNS

### 6.1 The "Near Neighbor" Squaring Pattern
```
If you know n², then (n+1)² = n² + 2n + 1

  Know 30²=900 → 31²=900+60+1=961 → 32²=961+62+1=1024
  Know 50²=2500 → 51²=2500+100+1=2601 → 52²=2601+102+1=2704
  Know 99²=9801 → 100²=9801+198+1=10000 ✓
```

### 6.2 Multiplication by Doubling/Halving
```
If one number is even:
  48 × 37 = 24 × 74 = 12 × 148 = 6 × 296 = 3 × 592 = 1776

Or: 48 × 37 = 48 × 40 − 48 × 3 = 1920 − 144 = 1776

Choosing the right path matters!
```

### 6.3 "Casting Out" for Quick Checking
```
After computing any answer, spend 3 seconds:
  1. Find digit sum of each input
  2. Apply same operation to digit sums
  3. Find digit sum of result — should match answer's digit sum

  47 × 63:
    DS(47)=11→2; DS(63)=9→0
    2×0=0
    Answer: 2961; DS=2+9+6+1=18→0 ✓

  234+567:
    DS(234)=9→0; DS(567)=18→0
    0+0=0
    Answer: 801; DS=9→0 ✓

  Limitation: doesn't catch errors that don't change digit sum (e.g., transposition)
  but catches ~89% of arithmetic errors.
```

---

## 🔍 SECTION 7: ADVANCED ESTIMATION FOR APTITUDE

### 7.1 Percentage Change Estimation
```
If price increases by 11% and quantity decreases by 9%:
  Net change ≈ 11−9 + (11×−9/100) = 2 − 0.99 ≈ 1%

Compound effect formula:
  Two changes a% and b%: net = a+b+ab/100

  +15% and −10%: net = 15−10+(15×−10/100) = 5−1.5 = 3.5%
  −5% and −5%:   net = −5−5+(25/100) = −9.75%
```

### 7.2 Ratio Estimation in Profit/Loss
```
If selling at 20% profit and buying at 10% discount:
  Effective multiplier: SP/CP = 1.2/0.9 = 4/3 ≈ 1.333 → 33.3% profit
  
  Shortcut: "Sell 20% more than you pay, but you bought 10% cheaper"
  Actual profit on original: you paid 0.9P, sold for 1.2P, profit = 0.3P/0.9P = 33.3%
```

### 7.3 Time-Speed-Distance Estimation
```
For average speed problems:
  If one speed >> other, average is dominated by slower:
  
  1 km at 1 km/h, 1 km at 1000 km/h:
  Total distance = 2 km
  Total time = 1 + 0.001 ≈ 1 hour
  Avg speed ≈ 2 km/h (NOT 500.5 km/h!)
  
  Moral: harmonic mean is always dominated by smallest value.
```

---

## ⚡ Number Sense Quick-Fire Drills

### Drill A: Is it divisible? (2 seconds each, Yes/No)
1. 4536 ÷ 9? → DS=18 → **Yes**
2. 7823 ÷ 11? → 7−8+2−3=−2 → **No**
3. 2436 ÷ 4? → 36÷4=9 → **Yes**
4. 1234 ÷ 8? → 234÷8=29.25 → **No**
5. 9801 ÷ 9? → DS=18 → **Yes** (99²!)
6. 1001 ÷ 7? → 1001÷7=143 → **Yes**
7. 3456 ÷ 6? → Even + DS=18 div by 3 → **Yes**
8. 7777 ÷ 11? → 7−7+7−7=0 → **Yes**
9. 12345 perfect square? → DS=15→6 → **No** (6∉{0,1,4,7,9})
10. 1156 perfect square? → DS=13→4 ✓ and ends in 6 ✓ → **Yes** (34²=1156)

### Drill B: Estimate to nearest 100 (5 seconds each)
11. 47 × 83 ≈ **3900** (actual 3901)
12. 234 × 17 ≈ **4000** (actual 3978)
13. 1234 ÷ 37 ≈ **33** (actual 33.35)
14. √4850 ≈ **70** (actual 69.6)
15. 23% of 456 ≈ **105** (actual 104.88)

### Drill C: Pythagorean Triple Recognition (instant)
16. Sides 3 and 4. Hypotenuse? → **5**
17. Sides 5 and 12. Hypotenuse? → **13**
18. Sides 8 and 15. Hypotenuse? → **17**
19. Sides 6 and 8. Hypotenuse? → **10** (scaled 3-4-5)
20. Hypotenuse 25, one side 7. Other? → **24** (7-24-25 triple)

---

## 🔗 Related Files
- [[VM_00_Vedic_Index]] — Vedic system overview
- [[CALC_01_Mental_Math_Master]] — Speed calculation tricks
- [[03_Formula_Sheet]] — All aptitude formulas
- [[QA_13_Percentage]] — Percentage in aptitude context

---

*⬅️ [[CALC_01_Mental_Math_Master]] | 🔙 [[aptitude_vault/00_Master_Index]]*
