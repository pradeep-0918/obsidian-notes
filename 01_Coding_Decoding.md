# 🔐 Coding & Decoding
### [← Back to Index](./00_INDEX.md) | Next: [Blood Relations →](./02_Blood_Relations.md)

---

## 🧒 What is Coding & Decoding? (Feynman Explanation)

Imagine you and your best friend make a **secret language** so no one else understands your messages. That's exactly what coding is!

> **Coding** = Changing words/numbers into a secret form  
> **Decoding** = Figuring out what the secret message means

**Real-life example:**  
If A=1, B=2, C=3... then "CAT" becomes "3-1-20". That's a code!

---

## 🔤 The Alphabet Position Table (MEMORIZE THIS!)

```
A=1   B=2   C=3   D=4   E=5   F=6   G=7
H=8   I=9   J=10  K=11  L=12  M=13
N=14  O=15  P=16  Q=17  R=18  S=19  T=20
U=21  V=22  W=23  X=24  Y=25  Z=26
```

### ⚡ 1-Second Trick: EJOTY Rule
```
E=5   J=10   O=15   T=20   Y=25
```
> Just remember **E-J-O-T-Y** and count ±1, ±2, ±3 from them!
> Example: S = T-1 = 20-1 = **19** ✅ (Done in 1 second!)

---

## 🟢 BASIC LEVEL

### Type 1 — Letter Shifting (Most Common!)

**The Idea:** Every letter is moved forward or backward by a fixed number.

#### Example 1:
> If CAT = DBU, then DOG = ?

**Solution:**
```
C → D  (moved +1)
A → B  (moved +1)
T → U  (moved +1)

So: D→E, O→P, G→H
Answer = EPH ✅
```

#### ⚡ 1-Second Trick:
- Find the shift value using **just the first letter**
- Apply same shift to all letters of the new word

---

### Type 2 — Opposite Letter Coding

**The Idea:** Each letter is replaced by its **mirror opposite** in the alphabet.

```
A ↔ Z   B ↔ Y   C ↔ X   D ↔ W   E ↔ V
F ↔ U   G ↔ T   H ↔ S   I ↔ R   J ↔ Q
K ↔ P   L ↔ O   M ↔ N
```

#### ⚡ 1-Second Formula:
```
Opposite of any letter = 27 - (letter's position)
Example: Opposite of D(4) = 27 - 4 = 23 = W ✅
```

#### Example:
> If MONKEY is coded as NLMSVB, find the code for ORANGE.

**Solution:**
```
M(13) → N(14)  [+1 shift]
O(15) → L(12)  ... wait, that's not consistent!
```
> ⚠️ Always check MORE than one letter to find the real pattern!

---

### Type 3 — Number Coding

**The Idea:** Letters are replaced by numbers (positions or some formula).

#### Example:
> If SUN = 19+21+14 = 54, what is MOON?

```
M=13, O=15, O=15, N=14
MOON = 13+15+15+14 = 57 ✅
```

---

## 🟡 INTERMEDIATE LEVEL

### Type 4 — Word/Sentence Coding

**The Idea:** Whole words are coded as other words. Look for **consistent substitution**.

#### Example:
```
"sky is blue"  → "tic pic mic"
"grass is green" → "sic pic lic"
```
> What does "pic" mean?

**Solution:**
- "is" appears in both → "pic" = **is** ✅

#### ⚡ Trick: Find the word that appears in BOTH sentences — that's your key!

---

### Type 5 — Mixed Coding (Letters + Numbers)

#### Example:
> ROAD is coded as 5%3@, LOAD is coded as 4%3@. Find code for ROLE.

**Solution:**
```
R=5, O=%, A=3, D=@
L=4 (from LOAD)
E=? (not given — look for pattern or elimination)

ROLE = 5 % 4 E-code
```
> These need you to build a **substitution table** step by step.

---

### Type 6 — Reverse Coding

**The Idea:** The word is written in reverse, then coded.

#### Example:
> If CAT is coded as TAC, what is DOG coded as?

```
CAT reversed = TAC ✅
DOG reversed = GOD ✅
```

---

## 🔴 ADVANCED LEVEL

### Type 7 — Symbol Coding

Letters are replaced by symbols. Build a mapping table.

#### Example:
> If A=@, B=*, C=#, D=$, then ABCD = @*#$
> Decode: #@*$ = ?

```
# = C, @ = A, * = B, $ = D
Answer = CABD ✅
```

---

### Type 8 — Matrix Coding

A grid/matrix is given. Letters are coded as (row, column) pairs.

```
     0    1    2
0  [ A    B    C ]
1  [ D    E    F ]
2  [ G    H    I ]
```

> A = (0,0), E = (1,1), I = (2,2)

#### Example: Code for "BED" = ?
```
B = (0,1)
E = (1,1)
D = (1,0)
Code = 01, 11, 10 ✅
```

---

## ⚡ MASTER SHORTCUT SHEET

| Pattern | How to Solve | Time |
|---------|-------------|------|
| Same shift forward/backward | Check shift with 1st letter, apply to all | 3 sec |
| Opposite letters | Use formula 27 - position | 2 sec |
| Word coding | Find common word in sentences | 5 sec |
| Reverse word | Reverse letters directly | 2 sec |
| Number coding | Sum of letter positions | 4 sec |
| Symbol coding | Build mapping table | 10 sec |

---

## 🧪 Practice Questions

### 🟢 Basic
1. If CAT = 3120, what is DOG? *(A=1, Z=26 system)*
2. If APPLE → BQQMF, what is the code for MANGO?

### 🟡 Intermediate
3. "roses are red" = "mic tic pic", "sky is blue" = "sic bic tic". What is "tic"?
4. If ROAD → DARE, decode LANE.

### 🔴 Advanced
5. Matrix code: Row 0: PQR, Row 1: STU, Row 2: VWX. Code PUTS using (row,col) format.

---

## 📝 Answers

1. D=4, O=15, G=7 → DOG = 41567 *(or as per system used)*
2. +1 shift → NBOHO
3. "tic" = **are**
4. Find pattern: R→D, O→A, A→R, D→E (each letter goes back 14 or follows a pattern)
5. P=(0,0), U=(1,2), T=(1,0), S=(1,0)... → **00, 12, 10, 10**

---

## 🔗 Navigation

| ← Previous | 🏠 Home | Next → |
|------------|---------|--------|
| *(Start)* | [Index](./00_INDEX.md) | [Blood Relations](./02_Blood_Relations.md) |
