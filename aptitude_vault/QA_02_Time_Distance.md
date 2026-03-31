# 🚗 Time & Distance
> Part of [[01_Quant_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Very High | **Time per Q:** 90 sec

---

## 🧠 Concept

### The Core Idea
Distance, Speed, and Time form a perfect triangle:
- If you go **faster**, you cover the **same distance** in **less time**
- If you have **more time**, you cover **more distance** at the **same speed**

**Real-world intuition:**
> You drive 120 km in 2 hours → your speed is 60 km/h.
> At 80 km/h, you'd cover 120 km in just 1.5 hours.

---

## 📌 Key Formulas

### 1. The Holy Trinity
$$\text{Speed} = \frac{\text{Distance}}{\text{Time}}$$
$$\text{Distance} = \text{Speed} \times \text{Time}$$
$$\text{Time} = \frac{\text{Distance}}{\text{Speed}}$$

### 2. Unit Conversions (CRITICAL)
$$\text{km/h} \rightarrow \text{m/s}: \times \frac{5}{18}$$
$$\text{m/s} \rightarrow \text{km/h}: \times \frac{18}{5}$$

> Memory trick: **5/18 looks like a fraction going down (km → m/s = smaller unit)**

### 3. Average Speed

**When time is equal:**
$$\text{Avg Speed} = \frac{v_1 + v_2}{2}$$

**When distance is equal (different speeds):**
$$\text{Avg Speed} = \frac{2v_1v_2}{v_1 + v_2}$$

**When neither is equal:**
$$\text{Avg Speed} = \frac{\text{Total Distance}}{\text{Total Time}}$$

### 4. Relative Speed
- **Moving in SAME direction:** Relative speed = |v₁ − v₂|
- **Moving in OPPOSITE directions:** Relative speed = v₁ + v₂

### 5. Meeting Time (Two people moving towards each other)
$$\text{Time to meet} = \frac{\text{Distance between them}}{v_1 + v_2}$$

### 6. Catching Up (Same direction, one behind the other)
$$\text{Time to catch up} = \frac{\text{Gap between them}}{v_1 - v_2}$$

---

## 🛠️ Problem Solving Approach

### Step 1: Identify the Type
Before solving, classify the problem:
- One person? → Simple Speed-Distance-Time
- Two people? → Relative speed
- Same/opposite directions? → Add or subtract speeds
- Average speed? → Use harmonic mean formula
- Starting late? → Find gap distance first

### Step 2: Convert Units
Always convert to consistent units before calculating.
> If speed is in km/h, time must be in hours, distance in km.

### Step 3: Use Relative Speed
For two-body problems, reduce to one-body by using relative speed.

### Step 4: Draw a Timeline or Track
For complex problems, draw the motion on a line with positions and times.

---

## 🔥 Types of Questions

### Type 1: Basic Speed Calculation
> "A car travels 240 km in 4 hours. Find speed."
- Speed = 240/4 = **60 km/h**

### Type 2: Two Objects — Opposite Directions
> "A and B start from two points 500 km apart, moving towards each other at 60 and 90 km/h. When do they meet?"
- Closing speed = 60 + 90 = 150 km/h
- Time = 500/150 = **3.33 hours**

### Type 3: Two Objects — Same Direction (Chase)
> "A starts at 50 km/h. B starts 1 hour later at 70 km/h. When does B catch A?"
- After 1 hour, A is 50 km ahead
- Gap closing rate = 70 - 50 = 20 km/h
- Time = 50/20 = **2.5 hours after B starts**

### Type 4: Average Speed
> "A travels 60 km at 30 km/h, returns at 20 km/h. Average speed?"
- Total distance = 120 km
- Time = 60/30 + 60/20 = 2 + 3 = 5 hours
- Avg Speed = 120/5 = **24 km/h**
- OR use formula: 2(30)(20)/(30+20) = 1200/50 = **24** ✓

### Type 5: Time Difference
> "If speed increases by 10 km/h, journey takes 2 hours less for 300 km. Find original speed."
- 300/v − 300/(v+10) = 2
- Solve: 300(v+10) − 300v = 2v(v+10)
- 3000 = 2v² + 20v → v² + 10v − 1500 = 0 → v = **30 km/h**

### Type 6: Missed Train
> "A walks at 4 km/h, misses train by 10 min. At 6 km/h, reaches 5 min early. Find distance."
- Let distance = d
- d/4 − d/6 = 15/60 hours
- 3d/12 − 2d/12 = 1/4
- d/12 = 1/4 → d = **3 km**

### Type 7: The Flying Bird Problem
> "Two trains 500 km apart, moving toward each other at 60 and 90 km/h. Bird flies at 200 km/h between them. How far does bird fly?"
- Trains meet in: 500/150 = 10/3 hours
- Bird flies for 10/3 hours at 200 km/h
- Distance = 200 × 10/3 = **666.67 km**
- 🎯 Key insight: Only time matters for bird, not direction!

---

## 💡 Shortcuts & Tricks

### Trick 1: The 5/18 Hack
- 36 km/h = 36 × 5/18 = **10 m/s**
- 15 m/s = 15 × 18/5 = **54 km/h**
> Pattern: km/h value × 5/18 = m/s value (smaller number)

### Trick 2: Average Speed Shortcut
For equal distance, avg speed is ALWAYS less than arithmetic mean:
> 60 km/h and 40 km/h → Avg = 2(60)(40)/100 = **48 km/h** (not 50!)

### Trick 3: Time Ratio = Inverse of Speed Ratio (for equal distance)
> If speeds are 3:4, times taken are 4:3

### Trick 4: Meeting Point
Two people A and B start simultaneously from points X and Y:
- They meet, continue, and reach Y and X respectively
- At meeting: A's time / B's time = B's total distance / A's total distance

### Trick 5: Pattern for "Faster by X"
> If you travel at (v + a) instead of v, time saved = a × total time / (v + a)

---

## ⚠️ Common Mistakes

1. **Not converting units:** Mixing km/h with minutes
   - ❌ 60 km/h for 30 minutes = 60 × 30 = 1800 km
   - ✅ 60 km/h for 30/60 = 0.5 hours = 30 km

2. **Using simple average for average speed when distances are equal**
   - ❌ Avg of 60 and 40 = 50 km/h
   - ✅ Use harmonic mean = 48 km/h

3. **Wrong relative speed direction:**
   - Same direction: difference, NOT sum
   - Opposite direction: sum, NOT difference

4. **Starting time confusion:**
   - "A started 2 hours earlier" means A has a head start of 2 hours × speed

5. **The bird problem:** Don't try to calculate the bird's individual trips — just multiply time × bird's speed

---

## 🧩 Practice Questions

### Easy (⭐)
1. A car travels 120 km in 2 hours. Speed in km/h?
   > **60 km/h**

2. A train covers 300 km at 75 km/h. Time taken?
   > **4 hours**

3. Convert 54 km/h to m/s.
   > 54 × 5/18 = **15 m/s**

4. A cyclist goes at 15 km/h for 2 hours. Distance?
   > **30 km**

5. A train 100 m long passes a pole in 5 sec. Speed in km/h?
   > Speed = 100/5 = 20 m/s = 20 × 18/5 = **72 km/h**

### Medium (⭐⭐)
6. Two cars start from same point in opposite directions at 40 and 60 km/h. Distance after 3 hours?
   > (40+60) × 3 = **300 km**

7. A travels 100 km at 50 km/h, returns at 40 km/h. Average speed?
   > 2(50)(40)/(50+40) = 4000/90 = **44.44 km/h**

8. A walks at 4 km/h and reaches 10 min late. At 6 km/h, reaches 5 min early. Distance?
   > d/4 − d/6 = 15/60 = 1/4; d(3−2)/12 = 1/4; d = **3 km**

9. A and B start from X and Y (200 km) towards each other at 40 and 60 km/h. Where do they meet?
   > Time = 200/100 = 2 hrs; A travels = 40×2 = **80 km from X**

10. A covers 300 km at v. At v+10, takes 1 hr less. Find v.
    > 300/v − 300/(v+10) = 1; 3000 = v(v+10); v = **50 km/h**

### Hard (⭐⭐⭐)
11. Two trains 500 km apart. One at 60 km/h, other at 80 km/h. Bird at 100 km/h between them. How far does bird fly?
    > Meet time = 500/140 = 25/7 hours; Bird = 100 × 25/7 = **357.14 km**

12. Man walks to station at 4 km/h, misses train by 5 min. At 5 km/h, catches train 5 min early. Find distance.
    > d/4 − d/5 = 10/60; d/20 = 1/6; d = **20/6 = 3.33 km**

13. Car travels 300 km. 10 km/h faster → 1 hour less. Find original speed.
    > 300/v − 300/(v+10) = 1; 3000 = v²+10v; v = **50 km/h**

14. Two cyclists 60 km apart towards each other at 15 and 20 km/h. Fly at 30 km/h between them. Total fly distance?
    > Meet time = 60/35 hours; Fly = 30 × 60/35 = **51.43 km**

15. Man travels 100 km at speed v, returns at v/2. Total time = 15 hours. Find v.
    > 100/v + 100/(v/2) = 15; 100/v + 200/v = 15; 300/v = 15; v = **20 km/h**

---

## 🔗 Related Topics

- [[QA_05_Train_Problems]] — Applies relative speed to crossing problems
- [[QA_03_Time_Work]] — Uses same formula family (rate × time = work)
- [[QA_01_Average]] — Average speed is a special average
- [[QA_09_Ratio_Proportion]] — Speed ratios, time ratios
- [[03_Formula_Sheet]] — All formulas

---

## 🎯 Pattern Recognition

**When you see:** "How much time does B take to catch A?"
→ **Think:** Find the gap, divide by closing speed

**When you see:** "Average speed for the whole journey"
→ **Think:** Total distance ÷ Total time (never average the speeds directly)

**When you see:** "A bird flies back and forth"
→ **Think:** Ignore back-and-forth, just find meeting time × bird speed

---

*⬅️ [[QA_01_Average]] | ➡️ [[QA_03_Time_Work]]*
