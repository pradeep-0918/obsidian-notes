# 🔢 Vedic Squaring, Square Roots & Cubing
> Part of [[VM_00_Vedic_Index]] | 🔙 [[00_Master_Index]]

---

## PART A: SQUARING

### Method 1: Ekadhikena — Numbers Ending in 5
```
n5² → [n×(n+1)] | 25

15² = 1×2|25   = 225
25² = 2×3|25   = 625
35² = 3×4|25   = 1225
45² = 4×5|25   = 2025
55² = 5×6|25   = 3025
65² = 6×7|25   = 4225
75² = 7×8|25   = 5625
85² = 8×9|25   = 7225
95² = 9×10|25  = 9025
105²= 10×11|25 = 11025
115²= 11×12|25 = 13225
125²= 12×13|25 = 15625
135²= 13×14|25 = 18225
145²= 14×15|25 = 21025
155²= 15×16|25 = 24025
```
⏱️ **Master this in 2 seconds. It appears constantly in area problems.**

### Method 2: Yavadunam — Numbers Near Base
```
Near 100:
93² → def=7 → L=93−7=86 | R=7²=49 → 8649
97² → def=3 → L=97−3=94 | R=3²=09 → 9409
98² → def=2 → L=98−2=96 | R=2²=04 → 9604
99² → def=1 → L=99−1=98 | R=1²=01 → 9801
101²→ exc=1 → L=101+1=102| R=1²=01 → 10201
103²→ exc=3 → L=103+3=106| R=3²=09 → 10609
107²→ exc=7 → L=107+7=114| R=7²=49 → 11449
108²→ exc=8 → L=108+8=116| R=8²=64 → 11664

Near 1000:
997²→ def=3 → L=994 | R=009 → 994009
998²→ def=2 → L=996 | R=004 → 996004
1003²→exc=3→ L=1006| R=009 → 1006009
```

### Method 3: The Duplex — General Squaring of Any Number
```
The Duplex D(n) is defined as:
  For single digit:   D(a) = a²
  For two digits:     D(ab) = 2×a×b
  For three digits:   D(abc) = 2×a×c + b²
  For four digits:    D(abcd) = 2×a×d + 2×b×c

To square any number, find duplexes of sub-numbers:
  n² = concat of duplexes (with carries)

Example: 23² 
  D(2) = 4
  D(23) = 2×2×3 = 12
  D(3) = 9
  Read right: 4|12|9 → 4+1|2|9 → 529 ✓

Example: 34²
  D(3)=9, D(34)=2×3×4=24, D(4)=16
  9|24|16 → 9+2|4+1|6 → 11|5|6 → 1156 ✓

Example: 123²
  D(1)=1
  D(12)=2×1×2=4
  D(123)=2×1×3+2²=6+4=10
  D(23)=2×2×3=12
  D(3)=9
  Reading: 1|4|10|12|9
  With carries: 1|4|10|12|9
  From right: 9, 12→2 carry1, 10+1=11→1 carry1, 4+1=5, 1
  = 15129 ✓ (Check: 123²=15129 ✓)

Example: 56²
  D(5)=25, D(56)=2×5×6=60, D(6)=36
  25|60|36 → 25+6|0+3|6 → 31|3|6 → 3136 ✓
```

### Method 4: (a+b)² and (a−b)² Shortcut
```
Near round numbers:
  52² = (50+2)² = 2500+200+4 = 2704
  48² = (50−2)² = 2500−200+4 = 2304
  97² = (100−3)² = 10000−600+9 = 9409
  103² = (100+3)² = 10000+600+9 = 10609
  198² = (200−2)² = 40000−800+4 = 39204
  201² = (200+1)² = 40000+400+1 = 40401
```

---

## PART B: SQUARE ROOTS

### Method 1: By Duplex (Vedic)
```
Finding √5329:

Step 1: Group digits in pairs from right: 53|29
Step 2: Largest perfect square ≤ 53 is 49=7²
        → First digit of root = 7
Step 3: Remainder = 53−49 = 4
Step 4: Bring down next pair: 4|29 = 429
        Divisor = 2×7 = 14
        14 goes into 42 (first 2 of 429): 3 times → 3
Step 5: Check: 73² = 5329? → 5329 ✓
        √5329 = 73

Finding √7056:
  Pairs: 70|56
  Largest sq ≤ 70: 64=8² → first digit = 8
  Remainder: 70−64=6 → bring down: 656
  Divisor: 2×8=16; 16 into 65: 4 times
  Check: 84² = 7056 ✓ → √7056 = 84

Finding √15129:
  Pairs: 1|51|29
  √1 = 1
  Remainder: 0, bring down 51 → 051
  Divisor: 2×1=2; 2 into 5: 2 times → 12
  Remainder: 51−4×1×2−4=51−8−4=... (easier: check 12²=144? 11²=121, 12²=144 too big? 15129 - yes)
  → √15129 = 123 ✓
```

### Method 2: Approximate Square Root (Exam Trick)
```
For non-perfect squares, find approximate value:
  √50:
    7²=49, 8²=64 → between 7 and 8, closer to 7
    Better: √50 = 7.07 (since 50−49=1, gap to next=64−49=15; 1/15≈0.07)
    So √50 ≈ 7.07

  √150:
    12²=144, 13²=169 → between 12 and 13
    (150−144)/(169−144) = 6/25 = 0.24
    √150 ≈ 12.24 (actual: 12.247 ✓)
```

### Perfect Squares to Memorize
```
1²=1    2²=4    3²=9    4²=16   5²=25
6²=36   7²=49   8²=64   9²=81   10²=100
11²=121 12²=144 13²=169 14²=196 15²=225
16²=256 17²=289 18²=324 19²=361 20²=400
21²=441 22²=484 23²=529 24²=576 25²=625
26²=676 27²=729 28²=784 29²=841 30²=900
31²=961 32²=1024 33²=1089 40²=1600 50²=2500
```

---

## PART C: CUBING

### Method 1: Near Base Cubing (Anurupyena)
```
Near 100:
103³:
  Excess = 3
  Step 1: 103 + 2×3 = 103+6 = 109
  Step 2: 103 + 3 = 106; 106×3 = 318; pad to 4 digits: 0318... hmm
  
  Cleaner method for n³ near base:
  
  (100+a)³ = 100³ + 3×100²×a + 3×100×a² + a³
  
  103³: a=3
    = 1000000 + 3×10000×3 + 3×100×9 + 27
    = 1000000 + 90000 + 2700 + 27
    = 1092727 ✓

  97³: a=−3
    = 1000000 − 90000 + 2700 − 27
    = 912673 ✓
```

### Method 2: Anurupyena (Ratio Method) for Cubing
```
To find n³ using ratio (Vedic):

Example: 12³
  Ratio = 1:2
  Row 1: 1 | 1×2 | 1×4 | 1×8     = 1 | 2 | 4 | 8
  Row 2: Multiply middle two by 2: 0 | 0 | 4 | 16 ... 
  
  The formal Vedic cube method:
  
  12³:
  Let a=1, b=2 (digits of 12 with ratio a:b = 1:2)
  
  1  |  1×2×2  |  1×2²×2  |  2³   (no, this is getting complex)
  
  Simplest: Use (a+b)³ = a³+3a²b+3ab²+b³
  12³ = (10+2)³ = 1000+3×100×2+3×10×4+8 = 1000+600+120+8 = 1728 ✓
  
  13³ = (10+3)³ = 1000+900+270+27 = 2197 ✓
  15³ = (10+5)³ = 1000+1500+750+125 = 3375 ✓
  20³ = 8000
  25³ = 15625
```

### Cubes to Memorize
```
1³=1     2³=8     3³=27    4³=64    5³=125
6³=216   7³=343   8³=512   9³=729   10³=1000
11³=1331 12³=1728 13³=2197 14³=2744 15³=3375
20³=8000 25³=15625
```

### Cube Root Trick (Vedic)
```
For perfect cubes up to 6 digits:
Units digit pattern:
  x=1→1, x=2→8, x=3→7, x=4→4, x=5→5
  x=6→6, x=7→3, x=8→2, x=9→9, x=0→0

  Note: 3 and 7 swap, 2 and 8 swap!

Find ∛17576:
  Step 1: Units digit of 17576 = 6 → cube root ends in 6
  Step 2: Remove last 3 digits: left with 17
           2³=8 < 17 < 27=3³ → first digit is 2
  Step 3: ∛17576 = 26 ✓ (Check: 26³=17576 ✓)

Find ∛704969:
  Step 1: Units digit = 9 → cube root ends in 9
  Step 2: Remove last 3: left with 704
           8³=512 < 704 < 729=9³ → first digit = 8
  Step 3: ∛704969 = 89 ✓ (Check: 89³=704969 ✓)

Find ∛12167:
  Units digit: 7 → ends in 3
  Left: 12 → 2³=8<12<27=3³ → first digit = 2
  ∛12167 = 23 ✓ (23³=12167 ✓)
```

---

## 🧩 Practice Drills

### Squaring (2 seconds each)
1. 45² = **2025**
2. 55² = **3025**
3. 75² = **5625**
4. 35² = **1225**
5. 65² = **4225**
6. 97² = **9409**
7. 98² = **9604**
8. 103² = **10609**
9. 48² = **2304** [(50−2)²]
10. 52² = **2704** [(50+2)²]
11. 23² = **529** [Duplex]
12. 34² = **1156** [Duplex]
13. 56² = **3136** [Duplex]
14. 78² = **6084** [(80−2)²=6400−320+4]
15. 99² = **9801**

### Square Roots (5 seconds each)
16. √1225 = **35**
17. √5625 = **75**
18. √9409 = **97**
19. √2304 = **48**
20. √10609 = **103**

### Cube Roots (10 seconds each)
21. ∛13824 = **24** [units=4, left=13→2³<13<3³→2 → 24]
22. ∛32768 = **32** [units=8→2, left=32→3 → 32]
23. ∛50653 = **37** [units=3→7, left=50→3 → 37]
24. ∛79507 = **43** [units=7→3, left=79→4 → 43]

---

## 🔗 Related Files
- [[VM_01_Sutras_Overview]] — Sutras 1 and 10
- [[VM_04_Multiplication_Advanced]] — Duplex comes from Urdhva-Tiryak
- [[QA_10_Area_Mensuration]] — Squaring used constantly

---

*⬅️ [[VM_05_Division]] | ➡️ [[VM_07_Divisibility_Factors]]*
