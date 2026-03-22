# 📈 Series
> Part of [[02_Reasoning_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐ Easy–Medium | **Exam Weight:** High | **Time per Q:** 45 sec

---

## 🧠 Concept

Find the **pattern/rule** in a sequence and predict the next term.

**Series types:**
- **Number series:** 2, 4, 8, 16, ?
- **Letter series:** A, C, E, G, ?
- **Mixed series:** A1, C4, E9, G16, ?
- **Wrong term series:** Find the one that doesn't fit

---

## 📌 Pattern Detection Hierarchy

### Step 1: Check Simple Differences (+/−)
> 2, 5, 8, 11, ? → differences: +3, +3, +3 → **14**

### Step 2: Check Ratios (×/÷)
> 3, 6, 12, 24, ? → ratios: ×2, ×2, ×2 → **48**

### Step 3: Check Second-Order Differences
> 1, 2, 4, 7, 11, ? → differences: 1, 2, 3, 4 → next difference = 5 → **16**

### Step 4: Check Squares/Cubes
> 1, 4, 9, 16, 25 → 1², 2², 3², 4², 5² → **next = 36**
> 1, 8, 27, 64 → 1³, 2³, 3³, 4³ → **next = 125**

### Step 5: Check Alternating Series
> 2, 3, 5, 8, 10, 13, 15 → odd terms: 2,5,10,15(+5) ; even terms: 3,8,13(+5) → **two interleaved series**

### Step 6: Check Fibonacci-type
> 1, 1, 2, 3, 5, 8, 13 → each = sum of two before → **21**

---

## 📌 Letter Series Patterns

### Alphabetical Position
A=1, B=2, ..., Z=26
> A, C, E, G → positions 1, 3, 5, 7 (+2) → next = 9 = **I**

### Reverse Alphabet
Z=1, Y=2, ..., A=26 (or use position 27−n)
> Z, X, V, T → 26,24,22,20 (−2) → next = 18 = **R**

### Alternating Skips
> B, E, H, K → +3 each time → next = **N**
> A, D, I, P → differences: 3,5,7 (odd numbers) → next diff = 9 → P+9 = Y

---

## 🔥 Common Series Types in Exams

### Type 1: Pure AP (Arithmetic Progression)
> 5, 8, 11, 14, ? → common difference = 3 → **17**

### Type 2: Pure GP (Geometric Progression)
> 2, 6, 18, 54, ? → common ratio = 3 → **162**

### Type 3: Squares/Cubes Pattern
> 2, 5, 10, 17, 26 → 1²+1, 2²+1, 3²+1, 4²+1, 5²+1 → **37**

### Type 4: n(n+1) pattern
> 2, 6, 12, 20, 30 → 1×2, 2×3, 3×4, 4×5, 5×6 → **42**

### Type 5: Alternating +/×
> 2, 4, 3, 9, 4, 16, 5, ? → pairs: (2,4=2²), (3,9=3²), (4,16=4²), (5,?) → **25**

### Type 6: Prime numbers
> 2, 3, 5, 7, 11, 13, ? → primes → **17**

---

## 💡 Mental Math for Letter Series

Quick position lookup:
```
A B C D E F G H I J  K  L  M
1 2 3 4 5 6 7 8 9 10 11 12 13

N  O  P  Q  R  S  T  U  V  W  X  Y  Z
14 15 16 17 18 19 20 21 22 23 24 25 26
```

**Trick:** Middle letter is M/N (13/14). Letters from end: Z=26, Y=25, X=24...
Use "EJOTY" memory trick: E=5, J=10, O=15, T=20, Y=25. Count up/down from there.

---

## 🧩 Practice Questions

### Easy
1. 2, 4, 6, 8, ? → **10** (+2)
2. A, B, C, D, ? → **E** (consecutive)
3. 1, 3, 5, 7, ? → **9** (+2)
4. Z, Y, X, W, ? → **V** (−1)
5. 10, 20, 30, 40, ? → **50** (+10)
6. B, D, F, H, ? → **J** (+2)
7. 5, 10, 15, 20, ? → **25** (+5)
8. A, C, E, G, ? → **I** (+2)
9. 2, 5, 8, 11, ? → **14** (+3)
10. 1, 4, 9, 16, ? → **25** (n²)

### Medium
1. 2, 4, 8, 16, ? → **32** (×2)
2. A, C, F, J, O, ? → Diffs: 2,3,4,5 → +6 → O+6=U → **U**
3. 1, 3, 6, 10, ? → triangular: +2,+3,+4 → +5 → **15**
4. Z, X, U, Q, L, ? → Diffs: 2,3,4,5 → +6 backward → L−6=F → **F**
5. 5, 7, 10, 14, ? → +2,+3,+4 → +5 → **19**
6. B, E, J, Q, Z, ? → Diffs: 3,5,7,9 → +11 → Z+11=K → **K**
7. 2, 5, 11, 20, ? → Diffs: 3,6,9 → +12 → **32**
8. A, D, I, P, Y, ? → Diffs: 3,5,7,9 → +11 → Y+11=J → **J**
9. 1, 4, 10, 19, ? → Diffs: 3,6,9 → +12 → **31**
10. Z, Y, W, T, P, ? → Diffs: 1,2,3,4 → −5 → P−5=K → **K**

### Hard
1. 1, 2, 6, 15, 31, ? → Diffs: 1,4,9,16 (squares) → +25 → **56**
2. A, C, G, O, C, ? → Positions 1,3,7,15,3... Diffs: 2,4,8 (doubling) → next diff = 16 → 3+16=19=S → but wait pattern resets at Z? 1+2=3, 3+4=7, 7+8=15, 15+16=31 → 31-26=5=E → **E**... or the series: A(1),C(3),G(7),O(15),(31→31-26=5)=E,(E+32=37→37-26=11)=K
3. 2, 5, 11, 23, 44, ? → Diffs: 3,6,12,21 → Diffs of diffs: 3,6,9 → next=12 → 21+12=33 → 44+33=**77**
4. 1, 4, 10, 22, 46, ? → Each = 2×prev + 2: 1→4(×2+2), 4→10(×2+2), 10→22(×2+2), 22→46(×2+2), 46×2+2 = **94**
5. 3, 6, 12, 24, 48, ? → ×2 each → **96**

---

## 🔗 Related Topics
- [[LR_01_Coding_Decoding]] — Uses same letter position logic
- [[LR_07_Analogies]] — Pattern recognition skill
- [[QA_15_Number_System]] — Number properties help recognize patterns

---

*⬅️ [[LR_07_Analogies]] | ➡️ [[LR_09_Data_Sufficiency]]*
