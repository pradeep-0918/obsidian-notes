# 🔍 Vedic Divisibility Tests & Factors
> Part of [[VM_00_Vedic_Index]] | 🔙 [[00_Master_Index]]

---

## VEDIC DIVISIBILITY TESTS

### Standard Rules (Reinforced with Vedic Logic)

| Divisor | Vedic Rule | Example |
|---|---|---|
| 2 | Last digit even | 348 ✓ |
| 3 | Digit sum div by 3 | 321: 3+2+1=6 ✓ |
| 4 | Last 2 digits div by 4 | 1324: 24÷4=6 ✓ |
| 5 | Last digit 0 or 5 | 345 ✓ |
| 6 | Div by 2 AND 3 | 342 ✓ |
| 7 | See osculation method | — |
| 8 | Last 3 digits div by 8 | 5824: 824÷8=103 ✓ |
| 9 | Digit sum div by 9 | 729: 7+2+9=18 ✓ |
| 11 | Alternating sum div by 11 | 1234: 1−2+3−4=−2 ✗; 1001: 1−0+0−1=0 ✓ |

### Vedic Test for 7 (Osculation Method)
```
Is 1234567 divisible by 7?

Osculator for 7 = 5 (since 7×5=35, near multiple of 10)

Method:
  Take last digit, multiply by osculator, add to rest:
  1234567:
  Last digit: 7; 7×5=35; Add to 123456: 123456+35=123491
  Last digit: 1; 1×5=5; Add to 12349: 12349+5=12354
  Last digit: 4; 4×5=20; Add to 1235: 1235+20=1255
  Last digit: 5; 5×5=25; Add to 125: 125+25=150 (not 140)
  Actually: test 150÷7=21.4... → NOT divisible by 7 ✓
  
  (This is advanced; in exams, just do 1234567÷7 directly)
```

### Test for 13
```
Osculator for 13 = 4 (since 13×4=52, near 50+2)
Negative osculator: 9 (13×9=117≈120−3)

Faster: just divide.
```

---

## THE CASTING OUT NINES (Digit Sum Method)

### Concept
Any number's digit sum (reduced to single digit) ≡ the number (mod 9)

```
Digit sum of 347: 3+4+7=14→1+4=5
This means 347 ≡ 5 (mod 9)
Check: 347 = 38×9 + 5 ✓

Short rule: "Cast out" any nines or pairs summing to 9:
  5678: Cast out 5+4=9... no. 
  Just add: 5+6+7+8=26→2+6=8
```

### Using Digit Sum for VERIFICATION (Most Useful Application!)
```
Verify: 234 × 567 = 132678?

Digit sum of 234: 2+3+4=9→0 (multiple of 9)
Digit sum of 567: 5+6+7=18→9→0
Product of sums: 0×0=0

Digit sum of 132678: 1+3+2+6+7+8=27→9→0

0=0 ✓ → Likely correct!

(If digit sums don't match → definitely wrong.
If they match → very likely correct.)

Better example:
Verify: 47 × 83 = 3901?

Digit sum 47: 4+7=11→2
Digit sum 83: 8+3=11→2
Product: 2×2=4

Digit sum 3901: 3+9+0+1=13→4

4=4 ✓ → Likely correct!
(Check: 47×83=3901 ✓)
```

---

## 🧩 Divisibility Quick Drills

Check divisibility (yes/no):
1. 1452 by 4? → last 2 digits=52; 52÷4=13 → **Yes**
2. 2673 by 9? → 2+6+7+3=18 → **Yes**
3. 4321 by 11? → 4−3+2−1=2 → **No**
4. 1001 by 11? → 1−0+0−1=0 → **Yes**
5. 729 by 3? → 7+2+9=18 → **Yes**

---

*⬅️ [[VM_06_Squaring_Roots_Cubing]] | ➡️ [[VM_08_Fractions_Decimals]]*
