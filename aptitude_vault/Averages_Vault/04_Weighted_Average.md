# 04 — Weighted Average ⚖️
*← [[03_Shortcut_Tricks]] | Next → [[05_Average_of_Consecutive_Numbers]]*

---

## 🧒 Feynman Explanation

Imagine two classes:
- **Class A** has **10 students**, average marks = **60**
- **Class B** has **30 students**, average marks = **80**

Can you just say the combined average = (60+80)÷2 = 70?  

**NO! ❌** Because Class B has MORE students — they matter more!

> **Weighted Average = When different items have different "importance" (weight)**

---

## 🔑 Formula

$$\text{Weighted Average} = \frac{(n_1 \times A_1) + (n_2 \times A_2)}{n_1 + n_2}$$

Where:
- $n_1, n_2$ = weights (count, quantity, time, etc.)
- $A_1, A_2$ = averages of each group

---

## 📝 Example 1

> Class A: 10 students, avg = 60  
> Class B: 30 students, avg = 80  
> Combined average?

```
Sum of Class A = 10 × 60 = 600
Sum of Class B = 30 × 80 = 2400
Total Sum = 600 + 2400 = 3000
Total students = 10 + 30 = 40
Combined Average = 3000 ÷ 40 = 75 ✅
```

*(Notice: 75 is closer to 80 because Class B has more students)*

---

## ⚡ SHORTCUT — Alligation Method

> Use this when you want to find combined average FAST

**Formula:**
```
Combined Avg = A1 + [n2/(n1+n2)] × (A2 - A1)
```

Or visually — draw a cross:

```
      A1 (60)          A2 (80)
           \           /
            \         /
             Avg (?)
            /         \
           /           \
    (A2−Avg)        (Avg−A1)
    = ratio of n1    = ratio of n2
```

**For above example:**
```
Ratio n1:n2 = 10:30 = 1:3

Avg = 60 + [3/(1+3)] × (80-60)
    = 60 + (3/4) × 20
    = 60 + 15 = 75 ✅
```

---

## 📝 Example 2 — Mixed Commodities

> A shopkeeper mixes 20 kg sugar (₹25/kg) with 30 kg sugar (₹30/kg).  
> Find average price per kg.

```
Weighted Avg = (20×25 + 30×30) ÷ (20+30)
             = (500 + 900) ÷ 50
             = 1400 ÷ 50
             = ₹28/kg ✅
```

---

## 🚨 Common Mistake Alert!

❌ **Wrong:** Average of averages = (A1 + A2) / 2  
✅ **Right:** Use weighted formula when group sizes differ!

> Exception: If n1 = n2 (equal groups), simple average works!

---

## 🔗 Connected Notes
- [[03_Shortcut_Tricks]] — Shortcut Trick 8 (Alligation)
- [[09_Speed_Distance_Average]] — Speed average is weighted average in disguise!
- [[10_Practice_Questions_With_Solutions]] — Practice Q11–Q15

---
*Tags: #weighted-average #alligation #groups #averages*
