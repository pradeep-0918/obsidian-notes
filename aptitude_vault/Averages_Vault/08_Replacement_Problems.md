# 08 — Replacement Problems 🔄
*← [[07_Age_Problems]] | Next → [[09_Speed_Distance_Average]]*

---

## 🧒 Feynman Explanation

Imagine a team of 5 players.  
**One player leaves. Another player joins.**  
The average score changes.  
Can you find the NEW player's score? YES!

---

## 🔑 The Master Formula

```
╔══════════════════════════════════════════════════╗
║  REPLACEMENT FORMULA                             ║
║                                                  ║
║  New person's value =                            ║
║  Leaving person's value + n × (New Avg − Old Avg)║
╚══════════════════════════════════════════════════╝
```

**If average INCREASES** → New person > Old person  
**If average DECREASES** → New person < Old person

---

## 📝 Example 1 — Average Increases

> Average weight of 8 persons = 55 kg  
> One person (45 kg) is replaced  
> New average = 58 kg  
> Find weight of new person.

```
New person = 45 + 8 × (58 − 55)
           = 45 + 8 × 3
           = 45 + 24
           = 69 kg ✅
```

**Verify:**
- Old sum = 8 × 55 = 440
- Remove 45 → 440 − 45 = 395
- New sum = 8 × 58 = 464
- New person = 464 − 395 = 69 ✅

---

## 📝 Example 2 — Average Decreases

> Average salary of 10 people = ₹5000  
> One person leaves  
> Average becomes ₹4800  
> Find the salary of the person who left.

```
Using leaving formula:
Person who left = Old Avg + n × (Old Avg − New Avg)  
                        [notice: Old-New since avg dropped]

Actually, use the SUM method (safest):
Old sum = 10 × 5000 = 50,000
New sum = 9 × 4800 = 43,200
Person who left = 50,000 − 43,200 = ₹6,800 ✅
```

> 💡 For "someone LEAVES only" — always use sum difference. It's faster!

---

## 📝 Example 3 — Two Replacements

> Average of 5 numbers = 40  
> Two numbers 30 and 35 are replaced by 50 and 45  
> Find new average.

```
Old sum = 5 × 40 = 200
Remove 30+35 = 65 → Remaining = 200−65 = 135
Add 50+45 = 95 → New sum = 135+95 = 230
New Average = 230 ÷ 5 = 46 ✅
```

**Shortcut:**
```
Change in sum = (50+45) − (30+35) = 95 − 65 = +30
Change in avg = +30 ÷ 5 = +6
New avg = 40 + 6 = 46 ✅
```

---

## ⚡ One-Line Shortcuts

| Situation             | Shortcut                              |
| --------------------- | ------------------------------------- |
| One leaves, one joins | New = Old person + n×(ΔAvg)           |
| Only one leaves       | Left person = Old sum − New sum       |
| Only one joins        | New = New avg + n×(New avg − Old avg) |
| Multiple replacements | Track sum changes                     |

---

## 🧠 Visualization

```
BEFORE: [P1][P2][P3][P4][P5] → Avg = 30
                  ↓ P3 leaves
AFTER:  [P1][P2][??][P4][P5] → Avg = 32
                  ↑ New P3 joins

New P3 = Old P3 + 5 × (32−30) = Old P3 + 10
```

---

## 🔗 Connected Notes
- [[03_Shortcut_Tricks]] — Trick 5 (Replacement) & Trick 6 (New member)
- [[07_Age_Problems]] — Same logic applied to ages
- [[10_Practice_Questions_With_Solutions]] — Practice Q26–Q30

---
*Tags: #replacement #new-member #average-change #averages*
