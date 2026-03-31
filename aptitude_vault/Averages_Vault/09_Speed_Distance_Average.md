# 09 — Speed, Distance & Average 🚗
*← [[08_Replacement_Problems]] | Next → [[10_Practice_Questions_With_Solutions]]*

---

## 🧒 Feynman Explanation

If you drive **60 km/h** for 1 hour, then **40 km/h** for 1 hour —  
Is your average speed **50 km/h**?

**YES** — only because the TIME was equal!

But if you drove 60 km/h for the **first 60 km**, then 40 km/h for the **next 60 km** —  
Average speed is **NOT 50 km/h!** It's less!

> ⚠️ This is the #1 trap in speed-average problems!

---

## 🔑 Two Cases — Know Both!

### Case 1 — Equal TIME (Simple Average Works)
> If you travel at different speeds for **equal durations**:
$$\text{Avg Speed} = \frac{S_1 + S_2}{2}$$

---

### Case 2 — Equal DISTANCE (Harmonic Mean)
> If you travel at different speeds for **equal distances**:
$$\text{Avg Speed} = \frac{2 \times S_1 \times S_2}{S_1 + S_2}$$

> 🧠 This is called **Harmonic Mean** — always LESS than simple average!

---

## 📝 Example 1 — Equal Time

> A car goes at 60 km/h for 2 hours, then 80 km/h for 2 hours.  
> Average speed?

```
Equal time → Simple average
Avg Speed = (60 + 80) / 2 = 70 km/h ✅
```

---

## 📝 Example 2 — Equal Distance (Exam Favourite!)

> A car goes from A to B at 60 km/h, returns at 40 km/h.  
> Average speed for the whole trip?

```
Equal distance (same route both ways) → Harmonic Mean
Avg Speed = 2×60×40 / (60+40)
          = 4800 / 100
          = 48 km/h ✅
```

*(NOT 50 — that's the trap!)*

---

## 📝 Example 3 — Three Speeds, Equal Distance

> Three equal legs at 30, 60, 90 km/h.  
> Average speed?

```
Avg = 3 / (1/30 + 1/60 + 1/90)
    = 3 / (6/180 + 3/180 + 2/180)
    = 3 / (11/180)
    = 3 × 180/11
    = 540/11 ≈ 49.1 km/h ✅
```

**Shortcut for 3 equal distances:**
$$\text{Avg} = \frac{3}{\frac{1}{S_1} + \frac{1}{S_2} + \frac{1}{S_3}}$$

---

## 📝 Example 4 — Mixed (Unequal distances, different speeds)

> Travels 120 km at 60 km/h, then 80 km at 40 km/h.  
> Average speed?

```
Time 1 = 120/60 = 2 hours
Time 2 = 80/40 = 2 hours
Total distance = 120+80 = 200 km
Total time = 2+2 = 4 hours
Avg Speed = 200/4 = 50 km/h ✅
```

> Always fall back to: **Avg Speed = Total Distance / Total Time**

---

## 🔑 Master Formula (Works for ALL Cases!)

$$\text{Average Speed} = \frac{\text{Total Distance}}{\text{Total Time}}$$

> When in doubt, calculate Total Distance and Total Time separately, then divide!

---

## ⚡ Quick Decision Chart

```
Equal TIME? → Simple Average → (S1+S2)/2
Equal DISTANCE? → Harmonic Mean → 2S1S2/(S1+S2)
Neither? → Total Distance / Total Time
```

---

## 🚨 Exam Trap Summary

| What students do | What's correct |
|-----------------|----------------|
| Avg of 60 & 40 = 50 (equal distance) | ❌ Wrong! Use harmonic mean = 48 |
| Avg of 60 & 40 = 50 (equal time) | ✅ Correct! |

---

## 🔗 Connected Notes
- [[04_Weighted_Average]] — Speed average is weighted average in disguise
- [[03_Shortcut_Tricks]] — General average tricks
- [[10_Practice_Questions_With_Solutions]] — Practice Q31–Q35

---
*Tags: #speed #distance #harmonic-mean #average-speed #averages*
