# 🧪 Mixtures & Alligations
> Part of [[01_Quant_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Medium

---

## 🧠 Concept

**Mixture:** Combining two or more things with different concentrations/prices.
**Alligation:** A shortcut to find the ratio in which two ingredients are mixed to get a desired average.

---

## 📌 Key Formulas

### The Alligation Cross (Visual Method)
```
Cheaper (C)       Dearer (D)
           \     /
            \   /
             \ /
            Mean (M)
           /   \
          /     \
    (D-M)         (M-C)
```
**Ratio of Cheaper : Dearer = (D−M) : (M−C)**

### Replacement/Dilution Formula
After n replacements of volume v from total V:
$$\text{Final concentration} = \left(1 - \frac{v}{V}\right)^n \times \text{Initial concentration}$$

---

## 🔥 Types of Questions

### Type 1: Find mixing ratio
> "₹5/kg and ₹8/kg mixed to get ₹6/kg mixture. Ratio?"
- Using alligation: (8−6) : (6−5) = 2:1
- **Mix in ratio 2:1** (cheaper : dearer)

### Type 2: Mixture with water
> "40L mixture: milk:water = 3:1. Add water to make 2:3. Water added?"
- Milk = 30L (stays constant); Final = 30/(water+10)... milk:water = 2:3 → water = 45L; Add = 45−10 = **35L**

### Type 3: Repeated replacement
> "20L milk. Replace 4L with water, twice. Final milk?"
- After each: milk = 20×(1−4/20) = 20×0.8 = 16
- After twice: 20×(0.8)² = **12.8L**

---

## 💡 Alligation Shortcut (Always works)

Draw the cross, put cheaper price top-left, dearer top-right, mean at center.
Bottom = differences crossed over.
**Ratio = Right difference : Left difference**

---

## 🧩 Practice Questions

### Easy
1. Milk:water = 3:2. Total 50L. Milk? → 30L; Water? → **20L**
2. Rice ₹5/kg and ₹7/kg mixed in 2:3. Cost? → (2×5+3×7)/5 = (10+21)/5 = **₹6.20/kg**
3. 20% alcohol in 50L. Alcohol? → **10L**
4. 4L:1L mix (25L total). How much of each? → 20L and **5L**
5. Sugar:water = 1:4. Water = 10L. Sugar? → **2.5L**

### Medium
6. 40L mix, ratio 3:1. Add water → 2:1. Water added?
   > Milk = 30L; Final milk:water = 2:1; 30/water = 2; water = 15; Added = 15−10 = **5L**
7. ₹10/kg and ₹15/kg → ₹12/kg mixture. Ratio?
   > (15−12):(12−10) = 3:2 → **3:2**
8. 20% acid, 80L. Add water → 16%. Added?
   > 0.2×80 = 16L acid; 16/(80+x) = 0.16; 16 = 12.8+0.16x; 3.2 = 0.16x; x = **20L**

### Hard
9. Mix replaced 3 times, 5L from 25L. Initial 100% milk. Final milk%?
   > (1−5/25)³ = (0.8)³ = 0.512 = **51.2%**
10. 3 types: ₹5, ₹8, ₹12. Mix in 4:3:2. Cost?
    > (4×5 + 3×8 + 2×12)/(4+3+2) = (20+24+24)/9 = 68/9 = **₹7.56/kg**

---

## 🔗 Related Topics
- [[QA_09_Ratio_Proportion]] — Mixing ratios
- [[QA_13_Percentage]] — Concentration percentages
- [[QA_07_Profit_Loss]] — Cost price mixing

---

*⬅️ [[QA_13_Percentage]] | ➡️ [[QA_15_Number_System]]*
