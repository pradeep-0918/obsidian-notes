# ✖️ VM-03: Core Multiplication — Nikhilam & Urdhva-Tiryak
> Part of [[VM_00_Vedic_Index]] | 🔙 [[00_Master_Index]]

---

## 🧠 The Two Master Sutras of Multiplication

| Sutra | Best For | Speed |
|---|---|---|
| **Nikhilam** | Numbers near 10, 100, 1000 | ⚡⚡⚡ Fastest |
| **Urdhva-Tiryak** | ANY two numbers (universal) | ⚡⚡ Very fast |

> Master both and you can multiply ANY two numbers mentally in under 10 seconds.

---

## 📌 METHOD 1: Nikhilam Sutra
### *"All from 9, last from 10" — Multiplication Near Base*

### Core Concept:
Instead of direct multiplication, work with **deficiencies (how far from the base)**.

### THE SETUP:
```
Number    Deficiency (Base − Number)
  ───────    ──────────────────────────
  97     →    100 − 97 = 3
  96     →    100 − 96 = 4
```

### THE FORMULA (4 steps):
```
Step 1: Write both numbers and their deficiencies
Step 2: LEFT PART = Either (Number1 − Deficiency2) OR (Number2 − Deficiency1)
        [Both give same answer]
Step 3: RIGHT PART = Deficiency1 × Deficiency2
Step 4: Ensure right part has digits = log₁₀(base) digits
```

---

### CASE 1: Both numbers below base (base = 100)

**Example: 97 × 96**
```
   97  |  -3   (100-97=3)
   96  |  -4   (100-96=4)
   ─────────
   93  |  12   (97-4=93 or 96-3=93) | (3×4=12)

Answer: 9312
```

**Example: 88 × 93**
```
   88  |  -12  (100-88=12)
   93  |  -7   (100-93=7)
   ─────────
   81  |  84   (88-7=81) | (12×7=84)

Answer: 8184
```

**Example: 94 × 97**
```
   94  |  -6
   97  |  -3
   ─────────
   91  |  18   (94-3=91) | (6×3=18)

Answer: 9118 ✓ (Verify: 94×97 = 9118 ✓)
```

**⚠️ When right part needs padding:** Right part must have exactly 2 digits (for base 100)
```
98 × 99:
   98  |  -2
   99  |  -1
   ─────────
   97  |  02   ← Must pad to 2 digits!

Answer: 9702 ✓
```

**⚠️ When right part > 2 digits:** Carry over to left part
```
88 × 88:
   88  |  -12
   88  |  -12
   ─────────
   76  |  144  ← 144 is 3 digits! Carry the 1 to left

Left: 76 + 1 = 77, Right: 44

Answer: 7744 ✓
```

---

### CASE 2: Both numbers ABOVE base

**Rule change:** Deficiency becomes SURPLUS (positive)

**Example: 103 × 104 (base = 100)**
```
   103  |  +3
   104  |  +4
   ──────────
   107  |  12   (103+4=107) | (3×4=12)

Answer: 10712 ✓
```

**Example: 112 × 115**
```
   112  |  +12
   115  |  +15
   ──────────
   127  |  180  ← 3 digits! Carry 1

Left: 127+1=128, Right: 80

Answer: 12880 ✓ (Verify: 112×115 = 12880 ✓)
```

---

### CASE 3: One above, one below base (Mixed)

**Example: 103 × 97 (base = 100)**
```
   103  |  +3
    97  |  -3
   ──────────
   100  |  -9   (103-3=100) | (3×(-3) = -9)

Left: 100, Right: -9 → 100 × 100 - 9 = 9991

OR: When right part is negative:
Left part = 100 - 1 = 99 (borrow 1 × base = 100 from left)
Right part = 100 - 9 = 91
Answer: 9991 ✓
```

**Example: 108 × 94**
```
   108  |  +8
    94  |  -6
   ──────────
   102  |  -48  (108-6=102) | (8×-6=-48)

Right is negative and > 2 digits in magnitude:
Borrow: 102−1=101, Right = 100−48=52
Answer: 10152 ✓
```

---

### BASE 1000 — Nikhilam

**Example: 998 × 997**
```
   998  |  -2
   997  |  -3
   ─────────
   995  |  006  ← Must be 3 digits (base=1000)

Answer: 995006 ✓
```

**Example: 993 × 996**
```
   993  |  -7
   996  |  -4
   ─────────
   989  |  028

Answer: 989028 ✓
```

---

### BASE 10 — Nikhilam

**Example: 7 × 8**
```
   7  |  -3
   8  |  -2
   ─────────
   5  |  6   (7-2=5) | (3×2=6)

Answer: 56 ✓
```

**Example: 6 × 9**
```
   6  |  -4
   9  |  -1
   ─────────
   5  |  4

Answer: 54 ✓
```

---

## 📌 METHOD 2: Urdhva-Tiryak Sutra
### *"Vertically and Crosswise" — Universal Multiplication*

This sutra works for **ANY** multiplication. It processes digits in a specific pattern simultaneously.

### For 2-digit × 2-digit: (ab × cd)

```
Pattern:    a b
           ×c d
           ─────

Step 1 (→): b×d              [rightmost digits, vertical]
Step 2 (✕): a×d + b×c       [cross multiply]
Step 3 (←): a×c              [leftmost digits, vertical]
```

**Diagram:**
```
a  b       a  b       a  b
   \        \/        |
    ×        ×        |
   /        /\        |
c  d       c  d       c  d
STEP 3     STEP 2    STEP 1
```

### Example: 23 × 45
```
   2  3
×  4  5
─────────
Step 1 (vertical right): 3×5 = 15 → write 5, carry 1
Step 2 (cross):          2×5 + 3×4 = 10+12 = 22 + carry(1) = 23 → write 3, carry 2
Step 3 (vertical left):  2×4 = 8 + carry(2) = 10

Reading left to right: 10 | 3 | 5 = 1035 ✓
```

### Example: 67 × 83
```
   6  7
×  8  3
─────────
Step 1: 7×3 = 21 → write 1, carry 2
Step 2: 6×3 + 7×8 = 18+56 = 74 + carry(2) = 76 → write 6, carry 7
Step 3: 6×8 = 48 + carry(7) = 55

Answer: 55 | 6 | 1 = 5561 ✓
```

### Example: 98 × 76
```
   9  8
×  7  6
─────────
Step 1: 8×6 = 48 → write 8, carry 4
Step 2: 9×6 + 8×7 = 54+56 = 110 + carry(4) = 114 → write 4, carry 11
Step 3: 9×7 = 63 + carry(11) = 74

Answer: 74 | 4 | 8 = 7448 ✓
```

---

### For 3-digit × 3-digit: (abc × def)

**5-step pattern:**
```
Step 1 (→→):  c×f
Step 2 (✕→):  b×f + c×e
Step 3 (✕✕):  a×f + b×e + c×d
Step 4 (←✕):  a×e + b×d
Step 5 (←←):  a×d
```

### Example: 123 × 321
```
   1  2  3
×  3  2  1
───────────
Step 1: 3×1 = 3
Step 2: 2×1 + 3×2 = 2+6 = 8
Step 3: 1×1 + 2×2 + 3×3 = 1+4+9 = 14 → write 4, carry 1
Step 4: 1×2 + 2×3 = 2+6 = 8 + carry(1) = 9
Step 5: 1×3 = 3

Answer: 3 | 9 | 4 | 8 | 3 = 39483 ✓
```

---

### For 2-digit × 3-digit (pad 2-digit with 0):

**Example: 23 × 456**
```
Treat 23 as 023:
   0  2  3
×  4  5  6
───────────
Step 1: 3×6 = 18 → write 8, carry 1
Step 2: 2×6 + 3×5 = 12+15 = 27 + 1 = 28 → write 8, carry 2
Step 3: 0×6 + 2×5 + 3×4 = 0+10+12 = 22 + 2 = 24 → write 4, carry 2
Step 4: 0×5 + 2×4 = 8 + 2 = 10 → write 0, carry 1
Step 5: 0×4 = 0 + 1 = 1

Answer: 1 | 0 | 4 | 8 | 8 = 10488 ✓
```

---

## 📌 CHOOSING: Nikhilam vs Urdhva-Tiryak

| Situation | Use |
|---|---|
| Both numbers within ±15 of 100 | **Nikhilam** (faster) |
| Both numbers within ±5 of 1000 | **Nikhilam** (much faster) |
| Random 2-digit numbers | **Urdhva-Tiryak** |
| One near base, one not | **Urdhva-Tiryak** |
| 3-digit × 3-digit, not near base | **Urdhva-Tiryak** |

---

## 🔥 Speed Practice (Solve Mentally!)

### Nikhilam (base 100)
1. 96 × 98 → 94|04 = **9408** ✓
2. 91 × 93 → 84|21 = **8463** (check: wait 91-7=84, 9×7=63; → 84|63 = **8463** ✓)
3. 87 × 94 → 81|42 = **8142** ✓ (87-6=81, 13×6=78; 81+0|78 → 8178... recheck: 87×94; 87-6=81,94-13→ hmm use 87(base 100,def=13),94(def=6); cross: 87-6=81; right=13×6=78; → **8178** ✓)
4. 104 × 106 → 110|24 = **11024** ✓
5. 998 × 995 → 993|010 = **993010** ✓

### Urdhva-Tiryak
6. 43 × 67 → S1:21, S2:4×7+3×6=28+18=46→6c4, S3:4×6=24+4=28 → **2881** ✓
7. 72 × 58 → S1:16c1, S2:7×8+2×5=56+10=66+1=67c6, S3:7×5=35+6=41 → **4176** ✓
8. 85 × 37 → S1:35c3, S2:8×7+5×3=56+15=71+3=74c7, S3:8×3=24+7=31 → **3145** ✓

---

## 🔗 Related Notes
- [[VM_04_Multiplication_Advanced]] — Anurupyena, Ekanyunena
- [[VM_06_Squares_Cubes]] — Special multiplication cases
- [[VM_10_Verification_Checks]] — Verify your answers

---

*⬅️ [[VM_02_Addition_Subtraction]] | ➡️ [[VM_04_Multiplication_Advanced]]*
