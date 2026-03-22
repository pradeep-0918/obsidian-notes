# ➗ VM-05: Vedic Division Methods
> Part of [[VM_00_Vedic_Index]] | 🔙 [[00_Master_Index]]

---

## 📌 1. Paravartya Division by 9 (Fastest Trick)

**Algorithm for N ÷ 9:**
> Bring down first digit; each next result = previous + current digit; last result = remainder

### Examples:
**321 ÷ 9:**
```
Digits: 3, 2, 1
→ 3 | 3+2=5 | 5+1=6(R)
Quotient: 35, Remainder: 6 ✓ (35×9+6=321)
```

**1234 ÷ 9:**
```
1 | 1+2=3 | 3+3=6 | 6+4=10 → R≥9 → Q=137, R=1 ✓
```

**2035 ÷ 9:**
```
2 | 2+0=2 | 2+3=5 | 5+5=10 → Q=225+1=226, R=1 ✓
```

---

## 📌 2. Dividing by 5, 25, 125 (Exam Gold)

| Divide by | Method | Example |
|---|---|---|
| 5 | ×2 then ÷10 | 345÷5 = 690÷10 = **69** |
| 25 | ×4 then ÷100 | 425÷25 = 1700÷100 = **17** |
| 50 | ×2 then ÷100 | 350÷50 = 700÷100 = **7** |
| 125 | ×8 then ÷1000 | 375÷125 = 3000÷1000 = **3** |
| 250 | ×4 then ÷1000 | 2500÷250 = 10000÷1000 = **10** |
| 500 | ×2 then ÷1000 | 1500÷500 = 3000÷1000 = **3** |

---

## 📌 3. Remainder Tricks (Super Fast)

### ÷9: Digit sum
> 5678÷9: 5+6+7+8=26→2+6=8 → **R=8**

### ÷11: Alternating sum
> 1234÷11: (4+2)−(3+1)=6−4=2 → **R=2**

### ÷3: Digit sum mod 3
> 1234÷3: 1+2+3+4=10→1+0=1 → **R=1**

---

## 📌 4. Quick Divisibility Tests

| By | Rule | Quick Test on 1836 |
|---|---|---|
| 2 | Last digit even | 6 → **Yes** |
| 3 | Digit sum ÷3 | 18 → **Yes** |
| 4 | Last 2 digits ÷4 | 36÷4=9 → **Yes** |
| 6 | By 2 AND 3 | Both yes → **Yes** |
| 8 | Last 3 digits ÷8 | 836÷8=104.5 → **No** |
| 9 | Digit sum ÷9 | 18÷9=2 → **Yes** |
| 11 | Alternating sum ÷11 | (6+8)-(3+1)=10 → **No** |
| 25 | Last 2 digits ÷25 | 36→ **No** |

---

## 🔥 Speed Practice

1. 432 ÷ 9 = ? → 4|4+3=7|7+2=9→R=9≥9: Q=47+1=48, R=0 → **48 R 0** ✓
2. 1234 ÷ 25 = 1234×4÷100 = 4936÷100 = **49 R 36** (49.36)
3. Is 13572 divisible by 4? → Last 2 digits: 72÷4=18 → **Yes**
4. 875 ÷ 125 = 875×8÷1000 = 7000÷1000 = **7**
5. Remainder of 9876 ÷ 9 = 9+8+7+6=30→3+0=3 → **R=3**

---

## 🔗 Related Notes
- [[VM_03_Multiplication_Core]] — Inverse relationship
- [[VM_10_Verification_Checks]] — Verify divisions
- [[QA_15_Number_System]] — Divisibility in aptitude

---

*⬅️ [[VM_04_Multiplication_Advanced]] | ➡️ [[VM_06_Squares_Cubes]]*
