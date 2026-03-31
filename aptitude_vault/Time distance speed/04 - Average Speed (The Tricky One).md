# 04 — Average Speed: The Tricky One 🎭

> **Back:** [[03 - Unit Conversion (The Secret Weapon)]] | **Home:** [[🏠 HOME - Time Speed Distance Vault]]
> **Next:** [[05 - Relative Speed]]

---

## 🧒 The Big Misconception

> ❌ "Average speed = (Speed1 + Speed2) / 2"

**This is WRONG 90% of the time!**

Let me prove it with a story.

---

## 🚗 The Pizza Delivery Story

Ravi delivers a pizza going **60 km/h** (in a rush!).
He comes back at **20 km/h** (stuck in traffic).
Shop to customer = 30 km each way.

**What's his average speed for the whole trip?**

❌ Wrong answer (what most people say): (60 + 20) / 2 = **40 km/h**

✅ Let's calculate properly:
- Time going: 30/60 = **0.5 hours**
- Time returning: 30/20 = **1.5 hours**
- Total distance: 60 km
- Total time: 2 hours

**Average Speed = 60 ÷ 2 = 30 km/h** ← NOT 40!

> 🔑 Why? He spent MORE time at the slower speed, so the slow speed "weighs" more.

---

## 📐 The Real Formula

$$\text{Average Speed} = \frac{\text{Total Distance}}{\text{Total Time}}$$

This is always true. No exceptions.

---

## ⚡ Special Case: Same Distance Both Ways

When **both distances are equal** (going and coming back), use the **Harmonic Mean**:

$$\text{Average Speed} = \frac{2 \times S_1 \times S_2}{S_1 + S_2}$$

> "Two speeds, same distance" → Harmonic Mean!

### Proof:
Let distance each way = d

Time₁ = d/S₁, Time₂ = d/S₂

Avg Speed = 2d / (d/S₁ + d/S₂) = 2d / d(1/S₁ + 1/S₂) = **2S₁S₂ / (S₁+S₂)** ✅

---

## 🧪 Solved Examples

### Example 1 — Same Distance (Use Harmonic Mean)
> A person goes at 40 km/h and returns at 60 km/h. Average speed?

Avg = (2 × 40 × 60) / (40 + 60)
Avg = 4800 / 100
Avg = **48 km/h** ✅

> Notice: 48 is less than (40+60)/2 = 50. The harmonic mean is ALWAYS less than the arithmetic mean!

---

### Example 2 — 3 Speeds (Equal Distance Each)
> A person travels at 20, 30, and 40 km/h for equal distances. Average speed?

$$\text{Avg} = \frac{3}{\frac{1}{20} + \frac{1}{30} + \frac{1}{40}}$$

= 3 / (6/120 + 4/120 + 3/120)
= 3 / (13/120)
= 3 × 120/13
= **360/13 ≈ 27.69 km/h** ✅

---

### Example 3 — Equal TIME (NOT Distance!)
> A person spends equal time at 40 km/h and 60 km/h. Average speed?

When **time is equal**, average speed = arithmetic mean!

Avg = (40 + 60) / 2 = **50 km/h** ✅

> 🎯 KEY INSIGHT: Equal time → Simple average. Equal distance → Harmonic mean.

---

### Example 4 — Different Distances
> First 100 km at 50 km/h, next 200 km at 100 km/h. Average speed?

Time₁ = 100/50 = 2 hours
Time₂ = 200/100 = 2 hours
Total distance = 300 km
Total time = 4 hours

Avg Speed = 300/4 = **75 km/h** ✅

---

## 🔑 Decision Tree — Which Formula to Use?

```
Is the distance the same for all legs?
├── YES → Use Harmonic Mean: 2S₁S₂/(S₁+S₂)
│         (or 3S₁S₂S₃/(S₁S₂+S₂S₃+S₁S₃) for 3 speeds)
└── NO  → Is the time the same for all legs?
          ├── YES → Simple Average: (S₁+S₂)/2
          └── NO  → Calculate: Total D ÷ Total T
```

---

## ⚡ Lightning Tricks

### Trick 1: Quick Harmonic Mean
> When one speed is double the other (e.g., 30 and 60):
> Avg = 2/3 of the faster speed = 2/3 × 60 = **40 km/h**

### Trick 2: Quick Check
> Harmonic Mean is ALWAYS closer to the **slower** speed. If your answer is closer to the faster speed — it's wrong!

### Trick 3: Percentage Change Trick
> If speed increases by 25% (×5/4), time decreases by 20% (×4/5).
> (Reciprocal percentages for inverse proportion!)

---

## ✅ Self-Test

1. Car goes at 30 km/h, returns at 60 km/h. Average speed?
2. 3 equal stages: 10, 20, 30 km/h. Average speed?
3. First half time at 40 km/h, second half time at 80 km/h. Average?
4. First 60 km at 30 km/h, next 60 km at 60 km/h. Average?

> **Answers:**
> 1) 40 km/h (harmonic: 2×30×60/90 = 40)
> 2) 360/61 ≈ 5.9... wait: 3/(1/10+1/20+1/30) = 3/(6+3+2/60) = 180/11 ≈ 16.36 km/h
> 3) 60 km/h (equal time → simple average)
> 4) 40 km/h (harmonic mean)

---

> 🔗 **Next:** [[05 - Relative Speed]] — When TWO things are moving at once!

*Tags: #averagespeed #harmonic #tricky #formula #TSD*
