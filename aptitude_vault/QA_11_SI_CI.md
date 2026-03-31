# 💳 Simple Interest & Compound Interest
> Part of [[01_Quant_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Very High | **Time per Q:** 90 sec

---

## 🧠 Concept

**Simple Interest (SI):** Interest calculated on original principal every year. Linear growth.
**Compound Interest (CI):** Interest added to principal, then interest on interest. Exponential growth.

> SI: Same ₹100 interest every year on ₹1000 at 10%.
> CI: Year 1 = ₹100, Year 2 = ₹110, Year 3 = ₹121... Growing each year!

---

## 📌 Key Formulas

### Simple Interest
$$SI = \frac{P \times R \times T}{100}$$
$$A = P + SI = P\left(1 + \frac{RT}{100}\right)$$

Where: P = Principal, R = Rate (%), T = Time (years)

### Compound Interest
$$A = P\left(1 + \frac{R}{100}\right)^T$$
$$CI = A - P = P\left[\left(1 + \frac{R}{100}\right)^T - 1\right]$$

### When compounded more frequently
- **Half-yearly (2× per year):** A = P(1 + R/200)^(2T)
- **Quarterly (4× per year):** A = P(1 + R/400)^(4T)

### Difference between CI and SI (important!)
For 2 years:
$$CI - SI = P\left(\frac{R}{100}\right)^2$$

For 3 years:
$$CI - SI = P\left(\frac{R}{100}\right)^2 \times \left(3 + \frac{R}{100}\right)$$

---

## 🛠️ Problem Solving Approach

### When to Use Which Formula?
- "Simple interest" → P×R×T/100
- "Compound interest annually" → P(1+R/100)^T
- "Half-yearly" → Replace R with R/2, T with 2T
- "Amount = 2P" (doubles) → use formulas to find T or R

### The Installment Method
For equal annual installments x paying off a loan P at r%:
$$P = x\left[\frac{1}{1+r} + \frac{1}{(1+r)^2} + ... + \frac{1}{(1+r)^n}\right]$$

---

## 🔥 Types of Questions

### Type 1: Find SI or CI directly
> "P=₹1000, R=5%, T=3 years. SI?"
> SI = 1000×5×3/100 = **₹150**

### Type 2: Find P, R, or T given SI
> "SI = ₹500, R=10%, T=5 years. P?"
> P = SI×100/(R×T) = 500×100/50 = **₹1000**

### Type 3: Amount doubles/triples
> "At SI, amount doubles in 10 years. Find rate."
> SI = P (doubles means SI = P); P = P×R×10/100 → R = **10%**

> "At CI, doubles in 5 years. Find rate."
> P(1+R/100)^5 = 2P → (1+R/100)^5 = 2 → R = 2^(1/5)−1 = **~14.87%**

### Type 4: CI vs SI difference
> "CI − SI for 2 years on ₹5000 at 4% = ?"
> = 5000 × (4/100)² = 5000 × 0.0016 = **₹8**

### Type 5: Sum doubles at CI
> "Sum doubles in x years. In how many years does it become 8 times?"
> If doubles in x years: 2^1 in x years; 2^3 in 3x years → **3x years**

---

## 💡 Shortcuts & Tricks

### Trick 1: The Rule of 72 (Approximate doubling time)
Years to double ≈ 72 ÷ R%
> At 8%: doubles in ≈ 72/8 = 9 years

### Trick 2: CI for small rates (2-year shortcut)
CI for 2 years = SI for 2 years + Interest on first year's interest
> P=1000, R=10%, T=2: SI=200; CI = 200 + 10 = **210**

### Trick 3: Multiplier for CI
- 10% for 2 years: × (1.1)² = × 1.21 → Amount = 1.21P
- 20% for 3 years: × (1.2)³ = × 1.728 → Amount = 1.728P

### Trick 4: Effective Annual Rate for half-yearly
> R = 10% p.a. compounded half-yearly: Effective = (1+5/100)²−1 = 10.25%

### Trick 5: When n years and amount known, find r
> Directly substitute: A = P(1+r/100)^n and solve for r

---

## ⚠️ Common Mistakes

1. **Mixing up SI and CI formulas**
   - SI grows linearly; CI grows exponentially

2. **Not adjusting for half-yearly/quarterly**
   - Half-yearly: R→R/2, T→2T

3. **CI − SI ≠ 0 for n≥2**
   - They're equal only for T=1

4. **Finding rate from "doubles in n years" — using SI formula for CI question**

5. **Principal confusion:**
   - "Amount = ₹1200, SI = ₹200" → Principal = 1200−200 = ₹1000

---

## 🧩 Practice Questions

### Easy (⭐)
1. P=₹1000, R=5%, T=2. SI? → 1000×5×2/100 = **₹100**
2. P=₹2000, R=4%, T=2. CI? → 2000×(1.04)²−2000 = 2000×1.0816−2000 = **₹163.20**
3. SI=₹500, R=10%, T=2. P? → P=500×100/20 = **₹2500**
4. P=₹3000, R=6%, T=3. SI? → 3000×6×3/100 = **₹540**
5. CI-SI for 2yrs, P=₹5000, R=4%? → 5000×(0.04)² = **₹8**

### Medium (⭐⭐)
6. Sum doubles in 10 years SI. Rate? → SI=P; P=P×R×10/100; R = **10%**
7. P=₹5000, R=4%, half-yearly, T=2 years. CI?
   > A = 5000×(1.02)^4 = 5000×1.0824 = 5412.16; CI = **₹412.16**
8. CI for 3 years at 10% on ₹10000?
   > 10000×(1.1)³ = 10000×1.331 = 13310; CI = **₹3310**
9. Sum triples in 20 years SI. Rate? → SI=2P; 2P=P×R×20/100; R = **10%**
10. A = ₹10000, SI = ₹2000, T=4 years. Rate?
    > R = 2000×100/(8000×4) = **6.25%**

### Hard (⭐⭐⭐)
11. CI doubles in 5 years. In how many years does it become 8 times?
    > 2P in 5 years; 8P = 2³P in 15 years = **15 years**

12. P = ₹10000. SI at 7% for 3 years = ₹2100. CI at same rate and time?
    > CI = 10000×(1.07)³ − 10000 = 10000×1.225043−10000 = **₹2250.43**

13. Sum = ₹5000 at CI. Amount after 2 years = ₹7100. Rate?
    > 5000(1+r)² = 7100; (1+r)² = 1.42; 1+r = 1.1916; r ≈ **19.16%**

14. Invest ₹4000 at CI for 4 years, amount = ₹5832. Rate?
    > (1+r)^4 = 5832/4000 = 1.458; 1+r = 1.458^(0.25) ≈ 1.1; r = **10%**

15. P=₹10000 at 6% CI for 2 yrs vs 3 yrs. Difference in CI?
    > CI2 = 10000(1.06²−1) = 1236; CI3 = 10000(1.06³−1) = 1910.16; Diff = **₹674.16**

---

## 🔗 Related Topics

- [[QA_13_Percentage]] — All interest uses % computation
- [[QA_07_Profit_Loss]] — Same % logic applied to buying/selling
- [[QA_01_Average]] — Average rate of return concepts
- [[03_Formula_Sheet]] — All formulas

---

*⬅️ [[QA_10_Area_Mensuration]] | ➡️ [[QA_12_Probability]]*
