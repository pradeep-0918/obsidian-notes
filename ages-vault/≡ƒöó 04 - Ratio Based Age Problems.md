# 🔢 04 — Ratio Based Age Problems
> *Back to [[🏠 HOME - Ages Master Vault]] | Tricks: [[⚡ 03 - Shortcut Tricks Arsenal]]*

---

## 🍰 What is a Ratio? (Feynman Style)

Imagine you cut a pizza into 5 slices.
You get 2 slices, your friend gets 3 slices.
The ratio of your pizza to friend's pizza = **2:3**

Ages work the same way. If ages are in ratio 2:3, think of it as:
- You have **2 parts** of age
- Friend has **3 parts** of age
- Total = **5 parts**

---

## 🔑 The k-Method (Most Important!)

> **Always replace a:b with ak and bk**

Why? Because:
- a:b just tells you the *proportion*, not the *actual value*
- k is the actual unit (could be 4, 5, 7, etc.)
- When you find k → multiply to get actual ages

---

## 📐 TYPE 1 — Ratio + Sum Given

**Template:**
> "Ages of A and B are in ratio a:b. Sum of ages = S"

**Steps:**
1. Let ages = ak and bk
2. ak + bk = S
3. k(a+b) = S
4. k = S/(a+b)
5. Ages = ak and bk

**Example:**
> Ratio = 3:7, Sum = 40

- k = 40 / (3+7) = 40/10 = **4**
- A = 3×4 = **12**, B = 7×4 = **28** ✅

⏱️ **10 seconds**

---

## 📐 TYPE 2 — Present Ratio + Future/Past Condition

**Template:**
> "Present ages in ratio a:b. After n years, ratio becomes p:q"

**Steps:**
1. Let ages = ak and bk
2. After n years: (ak+n) / (bk+n) = p/q
3. Cross multiply: q(ak+n) = p(bk+n)
4. Solve for k

**Example:**
> Present ratio = 2:3. After 4 years, ratio = 3:4. Find present ages.

Let ages = 2k and 3k

After 4 years: (2k+4)/(3k+4) = 3/4

Cross multiply:
```
4(2k+4) = 3(3k+4)
8k + 16 = 9k + 12
16 − 12 = 9k − 8k
k = 4
```

Ages: A = 2×4 = **8**, B = 3×4 = **12** ✅

---

## 📐 TYPE 3 — Past Ratio Given

**Template:**
> "n years ago, ages were in ratio a:b. Present sum = S"

**Steps:**
1. Present ages = x and y
2. (x−n)/(y−n) = a/b → cross multiply → get relation
3. x + y = S
4. Solve simultaneously

**Example:**
> 5 years ago, A:B = 1:2. Present sum = 25.

Step 1: (A−5)/(B−5) = 1/2
→ 2(A−5) = B−5
→ 2A−10 = B−5
→ **B = 2A − 5**

Step 2: A + B = 25
→ A + (2A−5) = 25
→ 3A = 30
→ A = **10**, B = **15** ✅

---

## 📐 TYPE 4 — Ratio of 3 People

**Template:**
> "A:B:C = a:b:c"

Let ages = ak, bk, ck
Use any given condition to find k.

**Example:**
> A:B:C = 2:3:5. Sum = 50.

2k + 3k + 5k = 50
10k = 50
k = 5

A = **10**, B = **15**, C = **25** ✅

---

## 🧠 Mental Math Ratio Trick

> If ratio is a:b and **difference** is given (not sum):

```
k = Difference / (b − a)     [assuming b > a]
```

**Example:** Ratio 2:5, Difference = 12
→ k = 12 / (5−2) = 12/3 = **4**
→ Ages = 8 and 20 ✅

---

## 🗺️ Ratio Problem Decision Tree

```
Ratio problem?
│
├── Sum given?
│   └── k = Sum / (a+b) ← TYPE 1
│
├── Difference given?
│   └── k = Diff / (b-a) ← Mental Math
│
├── Future/Past ratio given?
│   └── Cross multiply ← TYPE 2
│
└── 3 people?
    └── Let ak, bk, ck ← TYPE 4
```

---

*Next → [[🕐 05 - Past Present Future Ages]]*
*Review tricks → [[⚡ 03 - Shortcut Tricks Arsenal]]*
