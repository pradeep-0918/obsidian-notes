# ➕➖ VM-02: Vedic Addition & Subtraction
> Part of [[VM_00_Vedic_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

---

## 🧠 Why Vedic Addition/Subtraction is Different

Conventional method: Work right-to-left, carry over.
Vedic method: Work **left-to-right** (how we naturally read), no carrying needed.

> 💡 Left-to-right is faster because you can **say the answer out loud as you compute** — critical for oral aptitude tests!

---

## 📌 1. Left-to-Right Addition (The Core Shift)

### Traditional vs Vedic:
```
Traditional: 347 + 256
             Right to left: 7+6=13 (write 3, carry 1) → etc.

Vedic:       3+2=5 (hundreds) | 4+5=9 (tens) | 7+6=13
             5 | 9 | 13 → Adjust: 5 | 10 | 3 → 6 | 0 | 3 = 603
```

### Steps:
1. Add left to right, column by column
2. If any column ≥ 10, add 1 to the column to its LEFT
3. Read the final numbers from left to right

### Practice: 568 + 374
```
5+3=8 | 6+7=13 | 8+4=12
8 | 13 | 12
    ↑ 13 means add 1 to 8 → 9; write 3
         ↑ 12 means add 1 to 3 → 4; write 2
= 9 | 4 | 2 = 942
```

> ✓ Verify: 568+374 = **942** ✓

### Multi-number addition: 237 + 485 + 319
```
2+4+3=9 | 3+8+1=12 | 7+5+9=21
9 | 12 | 21
Adjust right: 21 → add 2 to 12 → 14, write 1
14 → add 1 to 9 → 10, write 4
= 10 | 4 | 1 = 1041
```

---

## 📌 2. The Vinculum (Bar Number) System

**Vinculum** = A bar over a digit meaning that digit is **negative**.

### Why use it?
Converts "difficult" digits (6, 7, 8, 9) into "easy" digits (1, 2, 3, 4) for faster mental arithmetic.

### The Conversion Rule:
> If digit > 5: Replace with (10 − digit) and put bar on it, add 1 to digit left of it.

### Example: 39 in vinculum
```
9 > 5, so: 9 → 1̄ (bar on 1, meaning −1), carry 1 to tens
3 + 1 = 4
So 39 = 4 1̄ (forty minus one = 39) ✓
```

### Example: 78 in vinculum
```
8 → 2̄ (=−2), carry to tens: 7+1=8
78 = 8 2̄ (eighty minus two = 78) ✓
```

### Example: 189 in vinculum
```
9 → 1̄, carry: 8+1=9 > 5 → 1̄, carry: 1+1=2
189 = 2 1̄ 1̄ (200 − 10 − 1 = 189) ✓
```

### Why this helps — Example: 87 × 9
```
Traditional: 87 × 9 = 783 (calculation needed)
Vinculum:   87 = 9 3̄ (ninety minus three)
So 87 × 9 = (90−3) × 9 = 810 − 27 = 783 ✓
```

---

## 📌 3. Nikhilam Subtraction
### *"All from 9, last from 10"*

**Sutra:** Nikhilam Navatashcaramam Dashatah

### Subtracting from powers of 10:

**Formula:**
- Each digit (except last) → 9 − digit
- Last digit → 10 − digit

### Example: 10000 − 3471
```
Ten-thousands: (only 1 digit being subtracted from 5-digit base)
3 → 9−3 = 6
4 → 9−4 = 5
7 → 9−7 = 2
1 → 10−1 = 9
Answer: 6529
Verify: 10000 − 3471 = 6529 ✓
```

### Example: 1000 − 456
```
4 → 9−4 = 5
5 → 9−5 = 4
6 → 10−6 = 4
Answer: 544 ✓
```

### Example: 100000 − 34567
```
3→6, 4→5, 5→4, 6→3, 7→3 → **65433** ✓
```

### Special case: When number has zeros
**Rule:** First non-zero digit from right uses "10 − digit", all preceding zeros stay 0, and all digits to the left use "9 − digit".

> 10000 − 3400 = ?
> 3→6, 4→5, 0→10→carry: 0→0+carry → hmm
> Better: 10000−3400 = 6600
> Nikhilam way: trailing zeros stay zero: 3→6, 4→6 (since 4 is last non-zero) → 6600 ✓

> 1000 − 530 = ?
> 5→4 (non-zero leftmost), 3→7 (last non-zero, use 10−3), 0→0 → 470 ✓

---

## 📌 4. The Complements System (Base Complement)

**10's complement of n** = 10 − n
**100's complement of n** = 100 − n
**9's complement of n** = 9 − n

### Rapid complement table:

| Number | 10-comp | 9-comp | 100-comp |
|---|---|---|---|
| 1 | 9 | 8 | 99 |
| 2 | 8 | 7 | 98 |
| 3 | 7 | 6 | 97 |
| 4 | 6 | 5 | 96 |
| 5 | 5 | 4 | 95 |
| 6 | 4 | 3 | 94 |
| 7 | 3 | 2 | 93 |
| 8 | 2 | 1 | 92 |
| 9 | 1 | 0 | 91 |

### Application in subtraction: 253 − 178
```
Method: 253 − 178 = 253 + complement(178) − 1000 (when using 1000 base)
complement(178): 8→2, 2→2 (last→10−8=2; others→9−digit)
Wait: 1000-comp of 178 = 822
253 + 822 = 1075 → remove the 1 (base 1000) → **075 = 75**
Verify: 253 − 178 = 75 ✓
```

> This turns subtraction into addition — which is mentally easier!

---

## 📌 5. Dot Method for Large Column Addition

Add a column of numbers by tracking "tens" with dots:

### Example: Add 67 + 48 + 93 + 55 + 34
```
Units column: 7+8+3+5+4 = 27 → write 7, carry 2 (2 dots in tens)
Tens column: 6+4+9+5+3 = 27 + 2(carry) = 29 → write 9, carry 2

Final: 297
```

### The Dot Shortcut:
As you add units digits:
- Every time you cross 10, put a dot (tally the carries)
- At the end, units digit = final units digit, tens = total dots + natural tens digit

---

## 📌 6. Speed Addition Drills

### Drill 1: Adding to reach 100
Pair numbers that sum to 100:
> 27 + 73, 45 + 55, 38 + 62, 91 + 09

### Drill 2: The "Making 10" trick
Group numbers to make 10s before adding:
> 6 + 7 + 4 + 3 + 8 + 2 = (6+4) + (7+3) + (8+2) = 10+10+10 = **30** (instant!)

### Drill 3: Progressive addition (key for average calculations)
> 12 + 15 + 18 + 21 + 24
> See it's AP: avg = middle = 18, count = 5, sum = 90

---

## 📌 7. Vedic Subtraction Shortcuts

### Shortcut 1: Round-up then adjust
> 856 − 497 = 856 − 500 + 3 = 356 + 3 = **359**
> (Subtract round number, add back the extra)

### Shortcut 2: Both-adjust
> 783 − 296 = 787 − 300 = **487**
> (Add same number to both: 783+4=787, 296+4=300)

### Shortcut 3: Complements for exam
> 8643 − 2789 = ?
> Complement of 2789 from 10000 = 7211
> 8643 + 7211 = 15854 → subtract 10000 → **5854**

---

## 🔥 Practice Problems

### Addition
1. 347 + 256 = **603**
2. 1847 + 2596 = **4443**
3. 47 + 83 + 29 + 41 = **200**
4. 10000 − 4567 = **5433** (Nikhilam)
5. 568 + 374 + 219 = **1161**

### Subtraction
6. 1000 − 347 = **653** (Nikhilam: 3→6, 4→5, 7→3)
7. 10000 − 2865 = **7135** (Nikhilam)
8. 743 − 398 = 747 − 402... better: 743−400+2 = **345**
9. 5000 − 1234 = **3766** (Nikhilam: 1→8, 2→7, 3→6, 4→6)
10. 100000 − 45678 = **54322** (Nikhilam)

---

## 🔗 Related Notes
- [[VM_03_Multiplication_Core]] — Nikhilam in multiplication
- [[VM_10_Verification_Checks]] — Check your addition instantly
- [[CALC_01_Speed_Tricks]] — More non-Vedic speed tricks

---

*⬅️ [[VM_01_Sutras_Overview]] | ➡️ [[VM_03_Multiplication_Core]]*
