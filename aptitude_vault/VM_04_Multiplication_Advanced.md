# ✖️ VM-04: Advanced Multiplication Techniques
> Part of [[VM_00_Vedic_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

---

## 📌 1. Anurupyena — Multiplication Near Multiple of Base
### *"By Proportionality"*

**When to use:** Numbers near a multiple of 10/100 (like 200, 300, 50, 500)

### How it works:
1. Choose **working base** = nearest multiple of 10/100
2. Compute using Nikhilam method
3. Multiply LEFT PART only by the **multiplier** (working base ÷ actual base)

### Example: 212 × 213 (working base = 200 = 2×100)
```
   212  |  +12
   213  |  +13
   ──────────
   225  |  156   (212+13=225) | (12×13=156)

Left × multiplier(2): 225 × 2 = 450
Right: 156 → 3 digits, carry 1 → Left: 450+1=451, Right: 56
Answer: 45156 ✓
```

### Example: 48 × 46 (working base = 50 = 5×10)
```
Multiplier = 5
   48  |  -2
   46  |  -4
   ─────────
   44  |  08   (must be 1 digit for base 10 → carry: 0|8)

Left × 5: 44 × 5 = 220; Right = 08 → 220×10+8 = 2208... 
Actually: Left part 44×5=220, append right part 08 → **2208** ✓
```

### Example: 302 × 305 (working base = 300 = 3×100)
```
   302  |  +2
   305  |  +5
   ──────────
   307  |  10

Left × 3: 307 × 3 = 921; append 10 → **92110** ✓
```

---

## 📌 2. Ekanyunena Purvena — Multiplying by 9, 99, 999...
### *"By One Less than the Previous"*

### N × 99 Formula:
> **Answer = (N−1) | (99 compliment = 100−N)**
> Write N−1, then write (100−N) as 2 digits

| Problem | N−1 | 100−N | Answer |
|---|---|---|---|
| 37 × 99 | 36 | 63 | **3663** |
| 48 × 99 | 47 | 52 | **4752** |
| 83 × 99 | 82 | 17 | **8217** |
| 25 × 99 | 24 | 75 | **2475** |
| 07 × 99 | 06 | 93 | **0693 = 693** |

### N × 999 Formula:
> **Answer = (N−1) | (1000−N) as 3 digits**

| Problem | N−1 | 1000−N | Answer |
|---|---|---|---|
| 237 × 999 | 236 | 763 | **236763** |
| 845 × 999 | 844 | 155 | **844155** |
| 058 × 999 | 057 | 942 | **057942** |

### N × 9 (single/double digit):
> **N × 9 = (N−1) then single digit: complement**
> Simpler: Just do N×10 − N mentally
> 47 × 9: 470 − 47 = **423** (subtract in head: 470−40=430, 430−7=423)

---

## 📌 3. Multiplying by 11 — The "Add Neighbors" Trick

### Rule: Write first | sum each adjacent pair | write last (carry if >9)

### 2-digit examples:
| Problem | Pattern | Answer |
|---|---|---|
| 23 × 11 | 2 \| 2+3 \| 3 | **253** |
| 48 × 11 | 4 \| 4+8=12 \| 8 → carry | **528** |
| 67 × 11 | 6 \| 6+7=13 \| 7 → carry | **737** |
| 99 × 11 | 9 \| 9+9=18 \| 9 → carry | **1089** |

### 3-digit examples:
| Problem | Pattern | Answer |
|---|---|---|
| 342 × 11 | 3\|3+4\|4+2\|2 = 3\|7\|6\|2 | **3762** |
| 567 × 11 | 5\|11\|13\|7 → carry right to left | **6237** |
| 123 × 11 | 1\|3\|5\|3 | **1353** |

### 4-digit: 2345 × 11
```
2 | 2+3 | 3+4 | 4+5 | 5
= 2 | 5 | 7 | 9 | 5 = 25795 ✓
```

---

## 📌 4. Special Number Pairs (Exam Gold)

### Pattern A: Same tens digit, units add to 10
> Format: (a b) × (a c) where b+c = 10
> **Formula:** a(a+1) | b×c [right part padded to 2 digits]

| Problem | a | b+c | a(a+1) | b×c | Answer |
|---|---|---|---|---|---|
| 43 × 47 | 4 | 3+7=10 | 4×5=20 | 3×7=21 | **2021** |
| 62 × 68 | 6 | 2+8=10 | 6×7=42 | 2×8=16 | **4216** |
| 81 × 89 | 8 | 1+9=10 | 8×9=72 | 1×9=09 | **7209** |
| 53 × 57 | 5 | 3+7=10 | 5×6=30 | 3×7=21 | **3021** |
| 24 × 26 | 2 | 4+6=10 | 2×3=06 | 4×6=24 | **0624=624** |
| 94 × 96 | 9 | 4+6=10 | 9×10=90 | 4×6=24 | **9024** |
| 73 × 77 | 7 | 3+7=10 | 7×8=56 | 3×7=21 | **5621** |

### Pattern B: Consecutive Numbers Multiplication
> n × (n+1) → must compute traditionally or use (n+0.5)² − 0.25
> 47 × 48 = 47.5² − 0.25 = 2256.25 − 0.25 = **2256**

### Pattern C: Numbers differ by 2
> n × (n+2) = (n+1)² − 1
> 47 × 49 = 48² − 1 = 2304−1 = **2303** ✓
> 98 × 96 = 97² − 1 = 9409−1 = **9408** ✓
> 23 × 25 = 24² − 1 = 576−1 = **575** ✓

### Pattern D: Multiplying two numbers equidistant from a round number
> Both numbers are d away from N: (N+d)(N−d) = N² − d²
> 47 × 53 = 50² − 3² = 2500−9 = **2491** ✓
> 38 × 42 = 40² − 2² = 1600−4 = **1596** ✓
> 97 × 103 = 100² − 3² = 10000−9 = **9991** ✓
> 196 × 204 = 200² − 4² = 40000−16 = **39984** ✓

---

## 📌 5. Multiplying by 5, 25, 50, 125, 500

| Multiply by | Trick | Example |
|---|---|---|
| 5 | ÷2, then ×10 | 348×5: 348÷2=174, ×10 = **1740** |
| 15 | ×10 + ×5 | 48×15: 480+240 = **720** |
| 25 | ÷4, then ×100 | 348×25: 348÷4=87, ×100 = **8700** |
| 50 | ÷2, then ×100 | 348×50: 174×100 = **17400** |
| 125 | ÷8, then ×1000 | 248×125: 248÷8=31, ×1000 = **31000** |
| 250 | ÷4, then ×1000 | 348×250: 87×1000 = **87000** |
| 500 | ÷2, then ×1000 | 348×500: 174×1000 = **174000** |
| 75 | ×100, then ×3÷4 | 48×75: 4800×3/4=3600 = **3600** |

---

## 📌 6. The "By Parts" Method (Distributive Law Speed)

**Useful for:** Multiplying when one number is close to a round number

### Rule: a × b = a × (round + diff) = a×round ± a×diff

### Examples:
> 47 × 38 = 47 × (40−2) = 1880 − 94 = **1786** ✓
> 63 × 97 = 63 × (100−3) = 6300 − 189 = **6111** ✓
> 84 × 52 = 84 × (50+2) = 4200 + 168 = **4368** ✓
> 124 × 98 = 124 × (100−2) = 12400 − 248 = **12152** ✓
> 376 × 25 = 376 × (100/4) = 37600/4 = **9400** ✓

---

## 🔥 Speed Drill — All Advanced Methods

| Problem | Method | Answer |
|---|---|---|
| 47 × 99 | Ekanyunena | **4653** |
| 83 × 99 | Ekanyunena | **8217** |
| 348 × 999 | Ekanyunena | **347652** |
| 567 × 11 | Add neighbors | **6237** |
| 43 × 47 | Same tens, →10 | **2021** |
| 72 × 78 | Same tens, →10 | **5616** |
| 47 × 53 | Equidistant from 50 | **2491** |
| 97 × 103 | Equidistant from 100 | **9991** |
| 348 × 25 | ÷4 × 100 | **8700** |
| 248 × 125 | ÷8 × 1000 | **31000** |
| 63 × 97 | By parts (×100−3) | **6111** |
| 212 × 213 | Anurupyena (×200) | **45156** |
| 48 × 49 | (n)(n+1): n²+n or Nikhilam from 50 | **2352** |
| 23 × 25 | Diff-of-2: 24²−1 | **575** |
| 84 × 52 | By parts (50+2) | **4368** |

---

## 🔗 Related Notes
- [[VM_03_Multiplication_Core]] — Nikhilam and Urdhva basics
- [[VM_06_Squares_Cubes]] — Squaring tricks built on these
- [[CALC_01_Speed_Tricks]] — More non-Vedic speed tricks

---

*⬅️ [[VM_03_Multiplication_Core]] | ➡️ [[VM_05_Division]]*
