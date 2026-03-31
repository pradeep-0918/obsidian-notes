# 05 — Average of Consecutive Numbers 🔢
*← [[04_Weighted_Average]] | Next → [[06_Missing_Number_Problems]]*

---

## 🧒 Feynman Explanation

Consecutive numbers are like steps on a staircase — evenly spaced.  
The average is always the **middle step**.

```
1 — 2 — 3 — 4 — 5
          ↑
        Average = 3 (the middle)
```

---

## 📊 Quick Formula Table

| Series | Formula for Average |
|--------|-------------------|
| 1, 2, 3 ... n | **(n + 1) / 2** |
| First n even numbers | **(n + 1)** |
| First n odd numbers | **n** |
| a, a+d, a+2d... (AP) | **(First + Last) / 2** |
| n consecutive from x | **x + (n−1)/2** |

---

## ⚡ The Golden Rules

### Rule 1 — Average of 1 to n
$$\text{Avg} = \frac{n+1}{2}$$

> Avg of 1 to 10 = 11/2 = **5.5**  
> Avg of 1 to 100 = 101/2 = **50.5**  
> Avg of 1 to 99 = 100/2 = **50** ✅

---

### Rule 2 — First n ODD numbers
$$\text{Avg} = n$$

> Avg of first 5 odd numbers (1,3,5,7,9) = **5** ✅  
> Avg of first 10 odd numbers = **10** ✅  
> (No calculation needed!)

---

### Rule 3 — First n EVEN numbers
$$\text{Avg} = n+1$$

> Avg of first 5 even (2,4,6,8,10) = **6** ✅  
> Avg of first 20 even numbers = **21** ✅  
> (No calculation needed!)

---

### Rule 4 — Any AP: Use First + Last
$$\text{Avg} = \frac{\text{First} + \text{Last}}{2}$$

> Series: 7, 11, 15, 19, 23  
> Avg = (7+23)/2 = 30/2 = **15** ✅

---

## 📝 Examples

**Q1:** Average of 11 to 30?
```
AP Series → First = 11, Last = 30
Avg = (11 + 30) / 2 = 41/2 = 20.5 ✅
```

**Q2:** Average of first 15 odd numbers?
```
Rule 2: Avg = n = 15 ✅
```

**Q3:** Average of 5, 10, 15, 20, 25, 30?
```
AP → Avg = (5 + 30)/2 = 35/2 = 17.5 ✅
```

**Q4:** Average of 3 consecutive numbers where smallest = 7?
```
Numbers = 7, 8, 9
Middle number = 8
Avg = 8 ✅
```

---

## 🧠 Memory Tricks

```
ODD numbers → Avg = n (same as count)
EVEN numbers → Avg = n+1 (one more than count)
ANY AP → Avg = (First + Last) / 2 = Middle
```

---

## 🔗 Connected Notes
- [[03_Shortcut_Tricks]] — Trick 2 (Middle number) & Trick 3 (First+Last)
- [[06_Missing_Number_Problems]] — Uses AP pattern
- [[10_Practice_Questions_With_Solutions]] — Practice Q1–Q5

---
*Tags: #consecutive #odd-even #AP #series #averages*
