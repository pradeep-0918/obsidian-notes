# 💰 Profit & Loss
> Part of [[01_Quant_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Very High | **Time per Q:** 90 sec

---

## 🧠 Concept

**The Story:**
- You **buy** something (Cost Price = CP)
- You **sell** it (Selling Price = SP)
- If SP > CP → **Profit**
- If SP < CP → **Loss**

**Real-world intuition:**
> You buy a shirt for ₹400, sell for ₹500. Profit = ₹100.
> Profit % = 100/400 × 100 = 25%.

---

## 📌 Key Formulas

### Basic
$$\text{Profit} = SP - CP \quad \text{(when SP > CP)}$$
$$\text{Loss} = CP - SP \quad \text{(when CP > SP)}$$
$$\text{Profit \%} = \frac{Profit}{CP} \times 100$$
$$\text{Loss \%} = \frac{Loss}{CP} \times 100$$

> ⚠️ **Profit% and Loss% are ALWAYS calculated on CP, not SP!**

### Finding SP when CP and % known
$$SP = CP \times \frac{100 + \text{Profit\%}}{100}$$
$$SP = CP \times \frac{100 - \text{Loss\%}}{100}$$

### Finding CP when SP and % known
$$CP = SP \times \frac{100}{100 + \text{Profit\%}}$$
$$CP = SP \times \frac{100}{100 - \text{Loss\%}}$$

### Marked Price & Discount
$$SP = MP \times \frac{100 - \text{Discount\%}}{100}$$
$$\text{Profit on MP} = \text{Mark\%} - \text{Discount\%} \approx \text{Net Profit\%}$$

### Exact net profit when both markup and discount given
$$\text{Net\%} = \left(\frac{100 + m}{100}\right) \times \left(\frac{100 - d}{100}\right) \times 100 - 100$$
Where m = markup%, d = discount%

---

## 🛠️ Problem Solving Approach

### The Multiplier Method (FASTEST)
Convert percentages to multipliers:
- 20% profit → SP = 1.2 × CP
- 15% loss → SP = 0.85 × CP
- 25% discount on MP → SP = 0.75 × MP

**Example:** CP = 200, 30% profit. SP?
> SP = 200 × 1.3 = **₹260** ✓ (no formula needed)

### The 100 Trick
Assume CP = 100 whenever CP is not given:
> "Profit is 20%. SP = ?"
> Assume CP = 100, then SP = 120.
> If actual SP = 240, then actual CP = 200.

---

## 🔥 Types of Questions

### Type 1: Find SP, CP, or Profit%
> "CP = 200, Profit = 25%. SP?"
> SP = 200 × 1.25 = **₹250**

### Type 2: Two articles — one profit, one loss
> "Two articles for ₹100 each. One at 10% profit, one at 10% loss. Overall?"
> CP1 = 100, SP1 = 110; CP2 = 100, SP2 = 90
> Total CP = 200, Total SP = 200 → **No profit, no loss**
> ⚠️ But if SAME selling price: always a loss!

### Type 3: Same SP, one profit one loss
> "Two articles sold at ₹200 each. One at 20% profit, one at 20% loss."
> CP1 = 200/1.2 = 166.67; CP2 = 200/0.8 = 250
> Total CP = 416.67, Total SP = 400 → **Loss!**
> Formula: Loss% = (common%/10)² = (20/10)² = **4%**

### Type 4: Markup and Discount
> "Article marked 40% above CP, sold at 10% discount. Profit%?"
> Assume CP = 100, MP = 140, SP = 140 × 0.9 = 126
> Profit% = 26%
> Formula: Net% = (1.4)(0.9) × 100 − 100 = 126 − 100 = **26%**

### Type 5: Dishonest Dealer (Uses faulty weights)
> "Dealer says he sells at CP but uses 800g instead of 1000g. Profit%?"
> He gives 800g but charges for 1000g
> Profit% = (100/800) × 100 = **25%**

### Type 6: Serial (Successive) Discounts
> "30% and 20% successive discounts = ?"
> Net discount = 1 − (0.7 × 0.8) = 1 − 0.56 = 44%
> NOT 50%!

---

## 💡 Shortcuts & Tricks

### Trick 1: The Multiplier Chain
Chain discounts: MP × (1 − d1) × (1 − d2) × ... = SP

### Trick 2: Equivalent Single Discount
Two discounts a% and b%: Equivalent = a + b − ab/100
> 20% and 10% → 20+10−2 = **28%** (not 30%)

### Trick 3: Find CP directly when SP and profit% given
CP = SP / (1 + profit%) → works in decimal directly
> SP = 240, profit = 20% → CP = 240/1.2 = **200**

### Trick 4: When you get profit = loss% on SP
> If profit% on CP = loss% on SP, then: CP/SP = SP/CP? No...
> This means the markup=20%: if P% on CP = L% on SP, then CP is marked up by P/(100-P)×100%

### Trick 5: Loss with same SP formula
$$\text{Loss\%} = \left(\frac{\text{common \%}}{10}\right)^2$$
> Works ONLY when two articles have same SP, one bought at x% profit and other at x% loss

---

## ⚠️ Common Mistakes

1. **Calculating % on SP instead of CP**
   - Profit% = Profit/CP × 100, NOT Profit/SP × 100

2. **Adding successive discounts**
   - 20% + 30% ≠ 50%. Use: 1−(0.8)(0.7) = 44%

3. **Same % profit and loss = break even (wrong)**
   - Same SP → always LOSS
   - Same CP → break even (profit gain = loss amount)

4. **Markup% ≠ Profit%**
   - Markup is on CP, but discount is on MP (marked price)

5. **Forgetting "marked price" concept:**
   - MP > CP → MP = CP × (1 + markup%)
   - SP = MP × (1 − discount%)
   - Actual profit = SP − CP

---

## 🧩 Practice Questions

### Easy (⭐)
1. CP = ₹100, SP = ₹120. Profit%? → **20%**
2. SP = ₹200, 25% profit. CP? → 200/1.25 = **₹160**
3. CP = ₹50, 10% loss. SP? → 50 × 0.9 = **₹45**
4. CP = ₹80, SP = ₹100. Profit? → **₹20 = 25%**
5. SP = ₹150, 10% loss. CP? → 150/0.9 = **₹166.67**

### Medium (⭐⭐)
6. Marked 30% above CP, sold at 10% discount. Profit%?
   > CP=100, MP=130, SP=117. Profit = **17%**

7. Two articles ₹200 each. One 20% profit, one 20% loss. Net?
   > CP1=200/1.2=166.67; CP2=200/0.8=250; Total CP=416.67; SP=400; Loss = 16.67; Loss% = **4%**
   > (Or use formula: (20/10)² = 4%)

8. Successive discounts 20% and 15%. Single equivalent?
   > 20+15−3 = **32%** (or: 1−0.8×0.85 = 1−0.68 = 32%)

9. SP = ₹240, 20% profit. If sold at ₹200, profit/loss%?
   > CP = 240/1.2 = 200. SP = CP → **No profit/loss (0%)**

10. Dishonest dealer uses 800g for 1000g, sells at CP. Profit%?
    > (200/800) × 100 = **25%**

### Hard (⭐⭐⭐)
11. Two articles CP ₹300 each. One 30% profit, one 20% loss. Net profit/loss%?
    > SP1=390, SP2=240; Total SP=630; Total CP=600; Profit=30; Profit% = **5%**

12. Article sold at 10% profit. If CP+SP both increase by ₹50, profit% becomes 8%. Original CP?
    > Let CP=c; Old profit=0.1c; New: SP+50=1.1c+50; New CP=c+50; New profit%=(0.1c)/(c+50)=8/100; 10c=8c+400; 2c=400; CP = **₹200**

13. Markup 25%, discount given, profit=12.5%. Discount%?
    > SP = CP×1.25×(1−d/100)=CP×1.125; 1.25(1−d/100)=1.125; 1−d/100=0.9; d = **10%**

14. 3 items for ₹400 each. Sold at 25% profit, 20% loss, and at CP. Net profit?
    > SP1=500, SP2=320, SP3=400; Total SP=1220; Total CP=1200; Profit = **₹20 = 1.67%**

15. A buys at ₹200, sells to B at 20% profit. B sells to C at 25% profit. C pays?
    > A→B: 200×1.2=240; B→C: 240×1.25 = **₹300**

---

## 🔗 Related Topics

- [[QA_13_Percentage]] — All profit/loss uses percentages
- [[QA_11_SI_CI]] — Interest also uses % on principal (like CP)
- [[QA_14_Mixtures_Alligations]] — Cost price mixing
- [[QA_09_Ratio_Proportion]] — CP:SP ratios
- [[03_Formula_Sheet]] — All formulas

---

## 🎯 Pattern Recognition

**When you see "same SP, profit% = loss%"** → Use loss% = (x/10)²
**When you see "successive discounts"** → Multiply (1-d1)(1-d2)
**When you see "dishonest dealer"** → Profit = (error/true-error) × 100
**When you see "mark up then discount"** → Multiply the two multipliers

---

*⬅️ [[QA_06_Ages]] | ➡️ [[QA_08_Permutations_Combinations]]*
