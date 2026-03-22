# 📊 Average
> Part of [[01_Quant_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐ Easy | **Exam Weight:** High | **Time per Q:** 45–90 sec

---

## 🧠 Concept

### What is an Average?
An **average** (or arithmetic mean) is a single number that represents the "center" of a group of numbers. It answers: *"If everyone had the same amount, what would that amount be?"*

**Real-world intuition:**
> You and 4 friends scored 70, 75, 80, 85, 90 in a test.
> If all 5 of you scored equally, you'd each get **80**.
> That 80 is the average.

### Core Formula
$$\text{Average} = \frac{\text{Sum of all values}}{\text{Number of values}}$$

Or rearranged:
- **Sum** = Average × Number of values
- **Number** = Sum ÷ Average

---

## 📌 Key Formulas

### 1. Basic Average
$$\bar{x} = \frac{x_1 + x_2 + ... + x_n}{n}$$

### 2. Average of First n Natural Numbers
$$\text{Avg} = \frac{n+1}{2}$$
> Example: Avg of 1–10 = (10+1)/2 = **5.5**

### 3. Average of First n Even Numbers
$$\text{Avg} = n + 1$$
> Example: Avg of first 5 even numbers (2,4,6,8,10) = 5+1 = **6**

### 4. Average of First n Odd Numbers
$$\text{Avg} = n$$
> Example: Avg of first 5 odd numbers (1,3,5,7,9) = **5**

### 5. Average of Consecutive Numbers
$$\text{Avg} = \frac{\text{First} + \text{Last}}{2}$$
> Example: Avg of 10, 11, 12, 13, 14 = (10+14)/2 = **12**

### 6. Weighted Average
$$\text{Weighted Avg} = \frac{n_1\bar{x}_1 + n_2\bar{x}_2}{n_1 + n_2}$$
> Used when combining two groups with different averages

### 7. Effect of Adding/Removing a Number
$$\text{New Sum} = \text{Old Avg} \times \text{Old Count} \pm \text{New Number}$$
$$\text{New Avg} = \frac{\text{New Sum}}{\text{New Count}}$$

### 8. Average Speed (when equal distances)
$$\text{Avg Speed} = \frac{2 \times v_1 \times v_2}{v_1 + v_2}$$
> ⚠️ This is the harmonic mean — NOT the simple average of speeds!

---

## 🛠️ Problem Solving Approach

### Step-by-Step Strategy

**Type 1: Find the average**
1. Add all numbers
2. Divide by count
3. Done ✓

**Type 2: A new number is added, find new average**
1. Old Sum = Old Avg × Old Count
2. New Sum = Old Sum + New Number
3. New Avg = New Sum ÷ New Count

**Type 3: A number is removed, find new average**
1. Old Sum = Old Avg × Old Count
2. New Sum = Old Sum − Removed Number
3. New Avg = New Sum ÷ (Old Count − 1)

**Type 4: Replacement — one number replaced by another**
1. Change in sum = New number − Old number
2. Change in average = Change in sum ÷ Total count
3. New Avg = Old Avg ± (Change / n)

**Type 5: Average increases/decreases — find the replaced number**
1. If avg increases by X with n members: removed number = new number − (n × X)
2. Rearrange: Old number = New number − n × (change in avg)

---

## 🔥 Types of Questions

### Type A: Direct Calculation
> "Find the average of 12, 18, 24, 30, 36"
- *Just add and divide*

### Type B: Missing Number
> "Average of 5 numbers is 20. Four of them are 18, 22, 24, 16. Find the fifth."
- Sum needed = 5 × 20 = 100
- Known sum = 18+22+24+16 = 80
- Fifth = 100 − 80 = **20**

### Type C: New Member Joins
> "Average age of 8 students = 12. A new student of age 18 joins. New average?"
- Old Sum = 8 × 12 = 96
- New Sum = 96 + 18 = 114
- New Avg = 114 ÷ 9 = **12.67**

### Type D: Wrong Value Corrected
> "Average of 10 numbers = 50. One number was misread as 25 instead of 52. Correct average?"
- Old Sum = 10 × 50 = 500
- Corrected Sum = 500 − 25 + 52 = 527
- Correct Avg = 527 ÷ 10 = **52.7**

### Type E: Two Groups Combined
> "Class A (30 students) avg = 70, Class B (20 students) avg = 80. Combined avg?"
- Sum A = 30 × 70 = 2100
- Sum B = 20 × 80 = 1600
- Combined = (2100 + 1600) ÷ 50 = **74**

### Type F: Average of Averages (Weighted)
> Never simply average the averages unless groups are equal size!

---

## 💡 Shortcuts & Tricks

### Trick 1: Deviation Method (Fastest for Calculation)
Instead of calculating exact sum, pick an assumed mean and work with deviations:
> Numbers: 48, 52, 54, 46, 50
> Assumed mean = 50
> Deviations: -2, +2, +4, -4, 0
> Sum of deviations = 0
> Actual avg = 50 + (0/5) = **50**

*Much faster than adding 48+52+54+46+50!*

### Trick 2: The "Shift" Trick
If all numbers change by the same amount, the average changes by the same amount.
> If each person gets ₹5 more, the average salary increases by ₹5.

### Trick 3: Average of Equally Spaced Numbers
For any arithmetic sequence (equal gaps), avg = middle term (for odd count) or avg of two middle terms (for even count).
> 3, 6, 9, 12, 15 → avg = 9 (middle)
> 3, 6, 9, 12 → avg = (6+9)/2 = 7.5

### Trick 4: Quick Age Problems
> "Present average age of a group is A. n years ago, average was B."
> Check: B = A − n (always true for fixed groups)

### Trick 5: When Average Increases by X after Adding a Number
> New number = Old average + (n+1) × X
> Where n = original count, X = increase in average

---

## ⚠️ Common Mistakes

1. **Averaging averages without weighting**
   - ❌ Avg of (60 and 80) = 70 — WRONG if group sizes differ
   - ✅ Always use weighted formula

2. **Wrong count after adding/removing**
   - ❌ Forgetting to change n from 5 to 6 after addition
   - ✅ Always update n

3. **Misreading "n years ago" vs "n years hence"**
   - "5 years ago" means subtract 5 from current ages
   - "5 years hence" means add 5 to current ages

4. **Speed average: using simple mean instead of harmonic mean**
   - ❌ Avg speed = (60+80)/2 = 70 — WRONG
   - ✅ Avg speed = 2(60)(80)/(60+80) = 68.57

5. **Not checking if "average" or "total" is being asked**

---

## 🧩 Practice Questions

### Easy (⭐)
1. Find the average of the first 10 natural numbers.
   > **Answer:** (10+1)/2 = **5.5**

2. Average of 5 numbers is 20. What is their sum?
   > **Answer:** 5 × 20 = **100**

3. A student scored 80, 85, 90, 95 in four subjects. What is the average?
   > **Answer:** (80+85+90+95)/4 = 350/4 = **87.5**

4. Average of 5 consecutive even numbers is 20. Find the smallest.
   > *Consecutive even: n-4, n-2, n, n+2, n+4. Middle = avg = 20*
   > Smallest = 20 - 4 = **16**

5. Average of 6 numbers is 30. One number is 36. What is the average of the rest?
   > Sum = 180; Remaining sum = 180 - 36 = 144; Avg = 144/5 = **28.8**

### Medium (⭐⭐)
6. Average of 5 numbers is 30. A new number is added; average becomes 32. Find the new number.
   > Old sum = 150; New sum = 32×6 = 192; New number = 192-150 = **42**

7. Average score of 8 students is 70. Two students with scores 60 and 80 are removed. Find new average.
   > Sum = 560; Removed = 140; New sum = 420; New avg = 420/6 = **70**

8. Average of 10 numbers is 50. If two numbers are replaced by 40 and 60, the average becomes 52. Find the sum of the replaced numbers.
   > Old sum = 500; New sum = 52×10 = 520; Sum of new = 100; So old sum = 100 - (520-500) = 100-20 = **80**

9. Average of 8 numbers is 40. A new number is added; average becomes 42. Find the new number.
   > Old sum = 320; New sum = 42×9 = 378; New number = **58**

10. Average of 7 numbers is 20. If 28 is replaced by 14, find new average.
    > Sum = 140; New sum = 140 - 28 + 14 = 126; New avg = 126/7 = **18**

### Hard (⭐⭐⭐)
11. Average of 10 numbers is 50. Two removed → avg becomes 52. Two more removed → avg becomes 54. Find average of the four removed numbers.
    > Initial sum = 500; After removing 2: sum = 52×8 = 416; First pair sum = 84
    > After removing 2 more: sum = 54×6 = 324; Second pair sum = 92
    > Total removed = 84+92 = 176; Avg of 4 = 176/4 = **44**

12. Average of 12 numbers is 36. Four replaced; average increases by 2. Original four sum to 140. Find new four's sum.
    > New avg = 38; New total sum = 456; Old total = 432; Difference = 24
    > New four sum = 140 + 24 = **164**

13. Average of 9 numbers is 50. Three (avg 45) removed; two (avg 60) added. Find new average.
    > Old sum = 450; Removed sum = 135; Added sum = 120; New sum = 435; Count = 8; Avg = **54.375**

14. Average of 7 numbers is 30. Each increased by 10% then decreased by 5%. Find new average.
    > New avg = 30 × 1.10 × 0.95 = 30 × 1.045 = **31.35**

15. Average of 8 numbers is 25. Two (avg 20) replaced by two (avg 30); then two (avg 50) removed. Find new average.
    > Sum = 200; After replacement: 200 - 40 + 60 = 220; After removal: 220 - 100 = 120; Count = 6; Avg = **20**

---

## 🔗 Related Topics

- [[QA_09_Ratio_Proportion]] — Weighted average uses ratio thinking
- [[QA_06_Ages]] — Most age problems are average problems in disguise
- [[QA_02_Time_Distance]] — Average speed formula is a special average
- [[QA_14_Mixtures_Alligations]] — Alligation is visual average
- [[QA_13_Percentage]] — Percentage change in averages
- [[03_Formula_Sheet]] — All formulas in one place

---

## 🎯 Exam Pattern Notes

**TCS/Infosys style:** Weighted average + new member joins type
**AMCAT style:** Consecutive numbers average
**Bank PO style:** Wrong value corrected type + group merge

> 💡 **Golden Rule:** In average problems, always find the **total sum** first. Everything else follows from sum ÷ count.

---

*⬅️ [[01_Quant_Index]] | ➡️ [[QA_02_Time_Distance]]*
