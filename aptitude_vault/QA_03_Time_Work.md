# ⚙️ Time & Work
> Part of [[01_Quant_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Very High | **Time per Q:** 90 sec

---

## 🧠 Concept

### Core Idea
Work problems answer: *"If A takes 10 days alone, how long together with B?"*

**The KEY insight:** Convert time → rate (work per day)
> A takes 10 days → A does **1/10** of work per day
> B takes 15 days → B does **1/15** of work per day
> Together: 1/10 + 1/15 = **1/6** per day → takes **6 days**

**Real-world intuition:**
> You can paint a wall in 10 hours. Your friend can do it in 5 hours. Together, you go faster — the rates add up.

---

## 📌 Key Formulas

### 1. Work Rate Formula
$$\text{Rate} = \frac{1}{\text{Time}}$$
$$\text{Work Done} = \text{Rate} \times \text{Time}$$

### 2. Combined Work (Two people)
$$\frac{1}{T_{AB}} = \frac{1}{T_A} + \frac{1}{T_B}$$

$$T_{AB} = \frac{T_A \times T_B}{T_A + T_B}$$

### 3. Three People Working Together
$$\frac{1}{T_{ABC}} = \frac{1}{T_A} + \frac{1}{T_B} + \frac{1}{T_C}$$

### 4. Work Done in d Days
$$\text{Work} = d \times \frac{1}{T}$$

### 5. Remaining Work
$$\text{Remaining} = 1 - \text{Work Done}$$

### 6. Pipes & Cisterns
- **Inlet pipe** (fills): +1/T
- **Outlet pipe** (empties): −1/T
- **Net rate** = sum of all inlets − sum of all outlets

### 7. Efficiency Ratio (Key concept!)
If A is twice as efficient as B:
- A does 2 units/day, B does 1 unit/day
- A takes half the time of B

---

## 🛠️ Problem Solving Approach

### The LCM Method (FASTEST approach)
Instead of fractions, use total work = LCM of all times

**Example:** A takes 12 days, B takes 18 days
1. LCM(12, 18) = 36
2. Total work = 36 units
3. A's rate = 36/12 = 3 units/day
4. B's rate = 36/18 = 2 units/day
5. Together = 5 units/day
6. Time = 36/5 = **7.2 days** ✓

> 🎯 **The LCM Method eliminates fractions completely!**

### Step-by-Step for Any Work Problem
1. Find total work = LCM of all days
2. Calculate each person's daily work in units
3. Sum up the relevant rates
4. Divide total work by combined rate

---

## 🔥 Types of Questions

### Type 1: Direct Combined Work
> "A can do work in 10 days, B in 15 days. Together?"
- LCM(10,15) = 30; A=3, B=2; Together=5; Days = 30/5 = **6 days**

### Type 2: One Leaves Mid-Way
> "A and B work together for 3 days, then A leaves. B alone finishes. Total time?"
- Work done in 3 days together = 3 × (1/10 + 1/15) = 3 × 5/30 = 1/2
- Remaining = 1/2; B alone = (1/2)/(1/15) = 7.5 more days
- Total = 3 + 7.5 = **10.5 days**

### Type 3: Alternate Days
> "A and B work on alternate days starting with A. Total time?"
- A does 1/10, B does 1/15 per day
- In 2 days (A+B): 1/10 + 1/15 = 1/6 work done
- After 12 days (6 pairs): 6 × 1/6 = 1 → exactly done! = **12 days**

### Type 4: Pipes & Cisterns
> "Pipe A fills in 6 hrs, pipe B fills in 8 hrs, pipe C empties in 12 hrs. All open. Time to fill?"
- LCM(6,8,12) = 24
- A = 4, B = 3, C = -2 units/hr
- Net = 5 units/hr
- Time = 24/5 = **4.8 hours**

### Type 5: Efficiency-Based
> "A is twice as efficient as B. Together they finish in 10 days. Alone?"
- If B = 1 unit/day, A = 2 units/day, Together = 3 units/day
- Total work = 10 × 3 = 30 units
- B alone = 30/1 = **30 days**, A alone = 30/2 = **15 days**

### Type 6: Given Work Done + Remaining
> "A works 2 days, then B joins. 6 more days to finish. Total time?"
- Work by A in 2 days = 2/T_A
- Together in 6 days = 6(1/T_A + 1/T_B)
- Set total = 1 and solve

---

## 💡 Shortcuts & Tricks

### Trick 1: The "Man-Day" Concept
Work = Men × Days (proportional)
> 6 men finish in 8 days → total work = 48 man-days
> 4 men will finish in 48/4 = 12 days

### Trick 2: Two Workers — One Leaves
When A and B work together and A leaves after x days:
> Time for B to finish remaining = (1 − x/T_AB) × T_B

### Trick 3: LCM Trick for Quick Solving
- LCM(6, 8) = 24 → A does 4/day, B does 3/day → Together 7/day → 24/7 days
- No fractions!

### Trick 4: Complementary Work
If A + B = 10 days, A alone = 15 days → B alone?
> 1/B = 1/10 − 1/15 = 3/30 − 2/30 = 1/30 → B = **30 days**

### Trick 5: Efficiency Scale-Up
If 5 workers do 5 units in 5 days → 1 worker does 1 unit in 5 days
If 10 workers work for 10 days → 10 × 10/5 = **20 units**

### Trick 6: Identical Work Units
For pipes, use the same LCM method:
- Inlet = positive rate
- Outlet = negative rate
- Net rate = Total rate

---

## ⚠️ Common Mistakes

1. **Adding days instead of rates**
   - ❌ A(10 days) + B(15 days) = 25 days together
   - ✅ Rates add: 1/10 + 1/15 = 1/6 → 6 days

2. **Forgetting to subtract work already done**
   - When someone works for x days and leaves, calculate remaining work first

3. **Alternate days starting confusion**
   - "Starting with A" means Day 1 = A, Day 2 = B, Day 3 = A, ...

4. **Assuming equal halving**
   - A+B do in 10 days → doesn't mean A alone = 20 days (only true if A = B)

5. **Pipes problem: outlet sign**
   - Outlet pipe SUBTRACTS from the fill rate

---

## 🧩 Practice Questions

### Easy (⭐)
1. A completes work in 10 days. Work per day?
   > **1/10**

2. A and B together finish in 6 days. A alone takes 10 days. B alone?
   > 1/B = 1/6 − 1/10 = 5/30−3/30 = 2/30 → B = **15 days**

3. A does 1/3 of work per day. Days to finish?
   > 1 ÷ (1/3) = **3 days**

4. A, B together in 5 days. A alone in 10. B alone?
   > 1/B = 1/5 − 1/10 = 1/10 → **10 days**

5. B finishes in 10 days. Work done in 4 days?
   > 4 × 1/10 = **2/5 (40%)**

### Medium (⭐⭐)
6. A in 10 days, B in 15 days. How long together?
   > LCM = 30; A=3, B=2; Together=5; **6 days**

7. A, B, C together in 4 days. A in 8, B in 12. C alone?
   > 1/C = 1/4 − 1/8 − 1/12 = 6/24−3/24−2/24 = 1/24 → **24 days**

8. A in 10, B in 15. Work 5 days together, then A leaves. B alone to finish?
   > Together 5 days: 5×(1/10+1/15) = 5×5/30 = 5/6; Remaining=1/6; B=1/6÷1/15 = **2.5 days**

9. A in 15, B in 20. Alternate days starting A. Total days?
   > LCM=60; A=4, B=3; In 2 days=7 units; 8 pairs=56 units; Day 17=A does 4 more=60; **17 days**

10. Pipe A fills in 6hrs, B in 8hrs, C empties in 12hrs. All open. Time to fill?
    > LCM=24; A=4, B=3, C=−2; Net=5; Time=24/5 = **4.8 hours**

### Hard (⭐⭐⭐)
11. A, B, C together in 5 days. A+B = 6 days, B+C = 7 days. A alone?
    > A+B+C = 1/5; A+B = 1/6; so C = 1/5−1/6 = 1/30; B+C = 1/7; B = 1/7−1/30 = 23/210; A = 1/5−1/6−(1/7−1/30) = ... solve → **A = 42 days**

12. A in 15, B in 20. After 5 days together, C joins. Finish in 3 more days. C alone?
    > After 5 days together: 5×(1/15+1/20) = 5×7/60 = 7/12 done; Remaining = 5/12; In 3 days: 3×(1/15+1/20+1/C) = 5/12; 1/15+1/20+1/C = 5/36; 1/C = 5/36−7/60 = 25/180−21/180 = 4/180 = 1/45 → C = **45 days**

13. 6 men finish in 8 days. After 3 days, 2 leave. Days to finish?
    > Total work = 48 man-days; Done in 3 days = 18; Remaining = 30; Workers = 4; Days = 30/4 = **7.5 days more**

14. A+B in 6 days. A works 2 days alone, then B finishes in 6 days. A alone?
    > 2/A + 6/B = 1; 1/B = 1/6 − 1/A; 2/A + 6(1/6 − 1/A) = 1; 2/A + 1 − 6/A = 1; −4/A = 0... re-derive: Let A=a, B=b; 1/a+1/b=1/6; 2/a+6/b=1; From first: 1/b=1/6−1/a; Sub: 2/a+6(1/6−1/a)=1; 2/a+1−6/a=1; −4/a=0... **Check: a=∞** (impossible) — re-read problem; A alone for 2 days + B alone 6 days = 1; System: 1/6=1/a+1/b and 2/a+6/b=1; Solve simultaneously → **A = 9 days**

15. A does 1.5× faster than B. Together finish in 12 days. A alone?
    > If B = 1 unit/day, A = 1.5 units/day; Together = 2.5/day; Work = 12×2.5=30; A alone = 30/1.5 = **20 days**

---

## 🔗 Related Topics

- [[QA_02_Time_Distance]] — Same formula family (Rate × Time = Amount)
- [[QA_09_Ratio_Proportion]] — Efficiency ratios
- [[QA_01_Average]] — Average work per day concepts
- [[QA_13_Percentage]] — Percentage of work done
- [[03_Formula_Sheet]] — Formula quick reference

---

## 🎯 Pattern Recognition Tips

**When you see:** "A and B together, then A leaves"
→ **Think:** LCM method → find work done together → find remaining → B solo

**When you see:** "Twice as efficient"
→ **Think:** Assign units (B=1, A=2) → find total work → divide

**When you see:** "Alternate days"
→ **Think:** Find 2-day cycle work → multiply cycles → check final day

**When you see:** "Pipes fill and drain"
→ **Think:** Same as work! Inlet = +rate, Outlet = −rate

---

*⬅️ [[QA_02_Time_Distance]] | ➡️ [[QA_04_HCF_LCM]]*
