# 📐 Vedic Algebra — Equations & Shortcuts
> Part of [[VM_00_Vedic_Index]] | 🔙 [[00_Master_Index]]

---

## METHOD 1: Sunyam — Immediate Equation Solutions

### Pattern: Same expression on both sides
```
(x+3)(x+4) = (x+3)(x+2)
  → x+3 = 0 → x = −3 ✓

(2x+1)(x−3) = (2x+1)(x+2)
  → 2x+1 = 0 → x = −1/2 ✓
```

### Pattern: Sum of numerator = Sum of denominator
```
If (ax+b)/(cx+d) = (ex+f)/(gx+h)
AND (ax+b)+(cx+d) = (ex+f)+(gx+h)
Then sum of all terms = 0.

Example: (2x+3)/(3x+2) = (5x+1)/(6x−1)
  Sum LHS: 2x+3+3x+2 = 5x+5
  Sum RHS: 5x+1+6x−1 = 11x
  Not equal here, use cross multiplication.

Clear Sunyam case:
  (x+2)/(x+3) = (x+4)/(x+5)
  
  Numerator difference = denominator difference:
  (x+2)−(x+4) = −2; (x+3)−(x+5) = −2 → same!
  
  This means: cross multiply → (x+2)(x+5) = (x+3)(x+4)
  x²+7x+10 = x²+7x+12 → 10=12 (no solution!) ← parallel lines
  
  Better Sunyam example:
  4/(2x+1) = 4/(x+3)
  Since numerators same → denominators same → 2x+1=x+3 → x=2
```

---

## METHOD 2: Paravartya — Linear Equations

### Standard Linear: ax + b = cx + d
```
Vedic: Transpose cross-terms
  5x + 3 = 2x + 9
  5x−2x = 9−3
  3x = 6 → x = 2

  7x − 4 = 3x + 12
  7x−3x = 12+4
  4x = 16 → x = 4

  (3x+1)/2 = (x+7)/3
  Cross: 3(3x+1) = 2(x+7)
  9x+3 = 2x+14
  7x = 11 → x = 11/7
```

---

## METHOD 3: Sankalana — Simultaneous Equations

### Standard Method (Add & Subtract)
```
System 1 — Direct elimination:
  x + y = 7   ...(1)
  x − y = 3   ...(2)
  
  Add: 2x=10 → x=5; Sub: 2y=4 → y=2

System 2 — Mirror coefficients:
  3x + 5y = 21
  5x + 3y = 27
  
  Add: 8x+8y=48 → x+y=6
  Sub: −2x+2y=−6 → x−y=3
  Solve: x=4.5, y=1.5

System 3 — General elimination:
  2x + 3y = 12  ...(1)
  4x + y = 10   ...(2)
  
  Multiply (2)×3: 12x+3y=30 ...(3)
  (3)−(1): 10x=18 → x=1.8
  Sub back: 3.6+3y=12 → y=2.8
```

### Cramer's Rule (Vedic Shortcut Version)
```
For: ax+by=p, cx+dy=q

  x = (pd−qb)/(ad−bc)
  y = (aq−cp)/(ad−bc)

Example: 2x+3y=7, 4x+y=9
  x = (7×1−9×3)/(2×1−4×3) = (7−27)/(2−12) = −20/−10 = 2
  y = (2×9−4×7)/(2×1−4×3) = (18−28)/(−10) = −10/−10 = 1
  → x=2, y=1 ✓
```

---

## METHOD 4: Quadratic Equations

### By Factoring (Calana-Kalanabhyam)
```
x² − 5x + 6 = 0
  Factors of 6 summing to −5: −2 and −3
  (x−2)(x−3) = 0 → x=2 or x=3

x² + x − 6 = 0
  Factors of −6 summing to 1: 3 and −2
  (x+3)(x−2) = 0 → x=−3 or x=2

2x² + 7x + 3 = 0
  Product = 2×3=6, sum=7 → 6 and 1
  2x²+6x+x+3 = 2x(x+3)+1(x+3) = (2x+1)(x+3)
  x = −1/2 or x = −3
```

### The Vedic Sum/Product Check
```
For x²+bx+c=0 with roots α,β:
  α+β = −b  (sum of roots)
  αβ = c    (product of roots)

Quick check: x²−5x+6: roots sum=5✓, product=6✓
```

### Completing the Square (Puranapuranabhyam)
```
Solve x²+6x+5=0:
  x²+6x = −5
  Add (3)²: x²+6x+9 = −5+9 = 4
  (x+3)² = 4
  x+3 = ±2 → x=−1 or x=−5 ✓
```

---

## METHOD 5: Vilokanam — By Inspection / Observation

### Immediately obvious solutions
```
x + 1/x = 2 → by inspection x=1 (since 1+1=2)

x + 1/x = −2 → x=−1

x² = x → x(x−1)=0 → x=0 or x=1

For ax = bx: (a−b)x=0 → x=0 (unless a=b, then any x)
```

---

## ⚡ Speed Formula Applications

### Sum of Roots and Product (Vieta's Formulas — Vedic Verification)
```
For ax²+bx+c=0:
  Sum of roots = −b/a
  Product of roots = c/a

Find quadratic when roots are 3 and 5:
  Sum = 8, Product = 15
  Equation: x²−8x+15 = 0 ✓

Find quadratic when roots are 1/2 and −3:
  Sum = 1/2−3 = −5/2; Product = 1/2×(−3) = −3/2
  Equation: x²+(5/2)x−(3/2)=0 → multiply by 2:
  2x²+5x−3=0 ✓
```

---

## 🧩 Practice Problems

### Linear Equations
1. 3x+7 = x+15 → x = **4**
2. 5x−3 = 2x+9 → x = **4**
3. (x+3)/2 = (2x−1)/3 → x = **11**
4. 7x = 3x+20 → x = **5**

### Simultaneous
5. x+y=10, x−y=4 → x=**7**, y=**3**
6. 2x+y=7, x+2y=8 → x=**2**, y=**3**
7. 3x+5y=21, 5x+3y=27 → x=**4.5**, y=**1.5**

### Quadratic
8. x²−7x+12=0 → x=**3 or 4**
9. x²+5x+6=0 → x=**−2 or −3**
10. 2x²−5x+3=0 → **(2x−3)(x−1)=0** → x=**3/2 or 1**

### Sum/Product
11. Roots sum=7, product=12. Equation? → **x²−7x+12=0**
12. Roots are 2 and −5. Equation? → x²+3x−10=0

---

*⬅️ [[VM_08_Fractions_Decimals]] | ➡️ [[VM_10_Drills_Practice]]*
