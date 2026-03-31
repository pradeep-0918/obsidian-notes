# 06 — Missing Number Problems 🔍
*← [[05_Average_of_Consecutive_Numbers]] | Next → [[07_Age_Problems]]*

---

## 🧒 Feynman Explanation

You know the **average** and most of the numbers.  
One number is **hiding**. Find it!

> The secret weapon: **Sum = Average × n**  
> Find the total, subtract what you know = missing number!

---

## 🔑 The One Trick That Solves Everything

```
╔══════════════════════════════════════════╗
║  MISSING NUMBER FORMULA                  ║
║                                          ║
║  Missing = (Avg × n) − Sum of rest       ║
╚══════════════════════════════════════════╝
```

---

## 📝 Example 1 — Basic

> Average of 5 numbers is 40.  
> Four numbers are: 30, 42, 45, 38.  
> Find the 5th number.

```
Total Sum = 40 × 5 = 200
Sum of 4 known = 30+42+45+38 = 155
Missing number = 200 − 155 = 45 ✅
```

---

## 📝 Example 2 — With Variables

> Average of a, 6, 7, 8, 9 is 8.  
> Find a.

```
Total Sum = 8 × 5 = 40
Sum of known = 6+7+8+9 = 30
a = 40 − 30 = 10 ✅
```

---

## 📝 Example 3 — Wrong Entry (Classic Exam Type!)

> Average of 10 numbers is 20.  
> One number was written as 36 instead of 63.  
> Find the correct average.

```
Wrong Sum = 20 × 10 = 200
Error = 63 − 36 = 27 (we wrote 27 less)
Correct Sum = 200 + 27 = 227
Correct Average = 227 ÷ 10 = 22.7 ✅
```

> ⚡ SHORTCUT: Change in Avg = Error ÷ n = 27 ÷ 10 = 2.7  
> New Avg = 20 + 2.7 = 22.7 ✅

---

## 📝 Example 4 — Two Missing Numbers with Extra Condition

> Average of 6 numbers is 15.  
> 4 numbers are 10, 12, 18, 20.  
> The remaining two numbers are in ratio 1:2.  
> Find both.

```
Total Sum = 15 × 6 = 90
Sum of 4 = 10+12+18+20 = 60
Sum of remaining 2 = 90 − 60 = 30
Ratio 1:2 → Numbers are x and 2x
x + 2x = 30 → 3x = 30 → x = 10
Numbers = 10 and 20 ✅
```

---

## 🚨 Exam Trap — Wrong Number Correction

| Situation | Effect on Sum |
|-----------|--------------|
| Written too small (e.g., 36 instead of 63) | Sum is less → Add the difference |
| Written too big (e.g., 63 instead of 36) | Sum is more → Subtract the difference |

---

## ⚡ Quick Template for Wrong Entry Problems

```
Correct Avg = Wrong Avg + (Correct value − Wrong value) / n
```

---

## 🔗 Connected Notes
- [[02_Simple_Average_Formula]] — SANe formula (Sum = Avg × n)
- [[03_Shortcut_Tricks]] — Trick 4 (Change in average)
- [[10_Practice_Questions_With_Solutions]] — Practice Q16–Q20

---
*Tags: #missing-number #wrong-entry #sum-trick #averages*
