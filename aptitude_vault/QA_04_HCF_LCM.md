# 🔢 HCF & LCM
> Part of [[01_Quant_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐ Easy | **Exam Weight:** Medium | **Time per Q:** 60–90 sec

---

## 🧠 Concept

**HCF (Highest Common Factor):** Largest number that divides ALL given numbers exactly.
> HCF(12, 18) = 6 → Both divisible by 6, nothing bigger works.

**LCM (Least Common Multiple):** Smallest number divisible by ALL given numbers.
> LCM(4, 6) = 12 → First number both 4 and 6 divide into.

**Real-world:**
- HCF → Cutting into equal pieces (maximize size)
- LCM → When do events repeat together (bells ringing, etc.)

---

## 📌 Key Formulas

### Finding HCF
**Method: Prime Factorization**
> HCF(36, 48) = ?
> 36 = 2² × 3²; 48 = 2⁴ × 3¹
> HCF = 2² × 3¹ = **12**

**Take lowest powers of common primes.**

### Finding LCM
**Method: Prime Factorization**
> LCM(36, 48) = ?
> 36 = 2² × 3²; 48 = 2⁴ × 3¹
> LCM = 2⁴ × 3² = **144**

**Take highest powers of ALL primes.**

### Golden Relationship
$$\text{HCF} \times \text{LCM} = \text{Product of two numbers}$$
> ⚠️ Works ONLY for two numbers, not three+

### HCF & LCM Formula
$$\text{LCM} = \frac{\text{Product of numbers}}{\text{HCF}}$$

### Remainder-Based HCF
> "Find greatest number dividing 23, 35, 59 with same remainder."
- Differences: 35-23=12, 59-35=24, 59-23=36
- HCF(12, 24, 36) = **12**

---

## 🔥 Types of Questions

### Type 1: Direct HCF/LCM
> "Find HCF(72, 108, 180)"
- 72=2³×3², 108=2²×3³, 180=2²×3²×5
- HCF = 2²×3² = **36**

### Type 2: LCM with Remainder
> "Smallest number when divided by 15, 20, 25 gives remainder 5."
- LCM(15,20,25) = 300
- Answer = 300 + 5 = **305**

### Type 3: Bell/Runner Problems
> "Bells ring at intervals of 6, 8, 9 seconds. Ring together in?"
- LCM(6, 8, 9) = **72 seconds**

### Type 4: Given HCF/LCM, Find Number
> "HCF = 16, LCM = 240. One number = 48. Find other."
- Product = 16 × 240 = 3840
- Other number = 3840/48 = **80**

---

## 💡 Shortcuts

- **For 2 numbers:** LCM = (a × b) / HCF
- **Bells problem:** Always = LCM of intervals
- **Same remainder:** HCF of differences

---

## 🧩 Practice Questions (5 each)

### Easy
1. HCF(12, 18) = **6** | 2. LCM(6, 8) = **24** | 3. HCF(24, 36) = **12**
4. LCM(12, 15) = **60** | 5. HCF(16, 24) = **8**

### Medium
6. HCF(72, 108, 180) = **36**
7. LCM(12, 15, 18) = **180**
8. Two bells ring at 6 and 8 sec. Together after? **LCM = 24 sec**
9. HCF=12, LCM=72. One=24. Other? **36**
10. Smallest divisible by 20, 30, 40? LCM = **120**

### Hard
11. Greatest number dividing 391, 425, 527 with remainders 5, 7, 9?
    > Subtract remainders: 386, 418, 518; HCF(386,418,518) = **2** ... actually HCF = **18** (check: 386=2×193, 418=2×209, 518=2×259; HCF(193,209,259) → GCD(16,50)=2 hmm... let me recompute: 386=2×193, 418=2×11×19, 518=2×7×37 → HCF = 2... for remainder type: 391-5=386, 425-7=418, 527-9=518; differences: 418-386=32, 518-418=100; HCF(32,100)=4; Answer = **4**)
12. Three bells 12, 15, 18 sec. LCM = **180 sec = 3 min**; Times in 1 hour = 3600/180 = **20 times**
13. Two numbers HCF=16, LCM=240. One=48. Other = 240×16/48 = **80**

---

## 🔗 Related Topics
- [[QA_15_Number_System]] — Divisibility, factors
- [[QA_09_Ratio_Proportion]] — Ratio of HCF/LCM

---

*⬅️ [[QA_03_Time_Work]] | ➡️ [[QA_05_Train_Problems]]*
