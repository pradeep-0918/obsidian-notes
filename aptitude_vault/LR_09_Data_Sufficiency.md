# 📊 Data Sufficiency
> Part of [[02_Reasoning_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Medium | **Time per Q:** 90 sec

---

## 🧠 Concept

Data Sufficiency questions ask: **"Is the given information ENOUGH to answer the question?"**

You are NOT asked to actually find the answer — only to determine if you CAN find it.

---

## 📌 Standard Answer Choices

Most exams use these options:
> (A) Statement 1 alone is sufficient
> (B) Statement 2 alone is sufficient
> (C) Both statements together are sufficient
> (D) Each statement alone is sufficient
> (E) Both statements together are NOT sufficient

---

## 🛠️ Step-by-Step Approach

### Step 1: Read the question carefully
Understand exactly what you need to find.

### Step 2: Test Statement 1 alone
- Ignore Statement 2 completely
- Can you get a **unique/definite** answer using only Statement 1?
- If YES → Note it
- If NO → Note it

### Step 3: Test Statement 2 alone
- Ignore Statement 1 completely
- Can you get a **unique/definite** answer using only Statement 2?

### Step 4: Test both together (if neither alone works)
- Use BOTH statements simultaneously
- Can you get a definite answer?

### Step 5: Select answer based on findings

---

## 🔑 Key Rule: "Sufficient" means UNIQUE answer

- If a statement gives you **multiple possible answers** → NOT sufficient
- If a statement gives you **exactly one answer** → SUFFICIENT
- "Sufficient" does NOT mean the answer is easy to compute — just that it EXISTS and is UNIQUE

---

## 🔥 Types of Questions

### Type 1: Algebraic (Find x)
> Is x > 5?
> (1) x + 2 = 7
> (2) x − 3 = 2

Statement 1: x = 5. Is 5 > 5? NO (5 is not > 5). Definite answer: No → Sufficient ✓
Statement 2: x = 5. Same answer → Sufficient ✓
**Answer: D (each alone sufficient)**

### Type 2: Geometric
> Is the shape a square?
> (1) All sides are equal
> (2) One angle is 90°

Statement 1: All sides equal → could be rhombus (not square). NOT sufficient ✗
Statement 2: One angle is 90° → could be rectangle or right triangle. NOT sufficient ✗
Both together: All sides equal + one 90° angle → **It's a square** → Sufficient ✓
**Answer: C**

### Type 3: Comparison
> Is A > B?
> (1) A = 10
> (2) B = 5

Statement 1: A=10, but we don't know B → NOT sufficient ✗
Statement 2: B=5, but we don't know A → NOT sufficient ✗
Both: A=10 > B=5 → YES, A > B → Sufficient ✓
**Answer: C**

### Type 4: Relational/Social
> Is A a doctor?
> (1) A works in a hospital
> (2) A has a medical degree

Statement 1: Hospital workers include nurses, admin, etc. NOT sufficient ✗
Statement 2: Medical degree → could be researcher, not practicing doctor. NOT sufficient ✗
Both: Medical degree + works in hospital → likely sufficient context
But strictly: Still could be a medical researcher at a hospital. Both together → **borderline — depends on exam context**

---

## 💡 Shortcuts & Tricks

### Trick 1: Yes/No Questions
For "Is X true?" questions — a statement is sufficient if it gives you a definitive YES or definitive NO. It doesn't have to be YES.

> "Is x > 0?"
> Statement: x = −5
> This gives definitive NO → **SUFFICIENT** ✓

### Trick 2: "Find the value" Questions
Statement must give exactly ONE numerical value.
> "x + y = 10" alone is NOT sufficient to find x (infinite solutions)
> "x + y = 10 AND x − y = 4" together → x=7, y=3 → SUFFICIENT

### Trick 3: Redundant Information Trap
Sometimes a statement gives information already implied by the question. Don't count it as new.

### Trick 4: Inequality Questions
For "Is x > 5?":
- Statement giving x = 3 → Definitive NO → Sufficient
- Statement giving x > 0 → Could be 3 or 7 → NOT sufficient

---

## ⚠️ Common Mistakes

1. **Solving for the actual answer instead of checking sufficiency**
   - Don't waste time computing — just check if unique answer exists

2. **Thinking "No" answers are insufficient**
   - A definitive NO is just as sufficient as a definitive YES

3. **Testing statements together first**
   - Always test each statement ALONE before combining

4. **Not being strict about uniqueness**
   - "x could be 5 or 7" means NOT sufficient

5. **Assumptions beyond the given**
   - Only use information explicitly stated in the question and statements

---

## 🧩 Practice Questions

### Easy
1. Is x > 5? (1) x + 2 = 7 (2) x − 3 = 2
   > S1: x=5. Is 5>5? No. Definite answer. ✓ | S2: x=5. Same. ✓ → **D**

2. Is A the eldest? (1) A is older than B (2) B is older than C
   > S1: A>B but don't know about C → NOT sufficient ✗ | S2: B>C, don't know about A ✗
   > Both: A>B>C → A is eldest → ✓ → **C**

3. Is x positive? (1) x > 0 (2) x < 10
   > S1: x>0 → YES, definite. ✓ | S2: x<10 → could be negative or positive. ✗ → **A**

4. Is x = 5? (1) x + 5 = 10 (2) x − 5 = 0
   > S1: x=5, definite. ✓ | S2: x=5, definite. ✓ → **D**

5. Is A taller than B? (1) A is 6 feet (2) B is 5 feet
   > S1: A=6, don't know B ✗ | S2: B=5, don't know A ✗ | Both: A=6>B=5 ✓ → **C**

6. Is the number divisible by 3? (1) Sum of digits is 12 (2) Number > 10
   > S1: Sum=12, divisible by 3 → YES, definite ✓ | S2: Just >10, not helpful ✗ → **A**

7. Is shape a square? (1) All sides equal (2) One angle 90°
   > S1: Could be rhombus ✗ | S2: Could be rectangle ✗ | Both: Square ✓ → **C**

8. Is x = y? (1) x + y = 10 (2) x − y = 0
   > S1: Many solutions (1+9, 2+8...) ✗ | S2: x=y, definite but don't know if equal to specific value... wait: Is x=y? → S2 says x−y=0 → x=y → YES ✓ → **B**

### Medium (from PDF)
9. Is the number even? (1) Divisible by 2 (2) Greater than 10
   > S1: Divisible by 2 = even, definite ✓ | S2: Many evens and odds >10 ✗ → **A**

10. Is x negative? (1) x < 0 (2) x > −10
    > S1: x<0 → YES, definite ✓ | S2: x could be −5 (negative) or 5 (positive) ✗ → **A**

11. Is the number divisible by 5? (1) Ends in 0 (2) Greater than 10
    > S1: Ends in 0 → divisible by 5, definite ✓ | S2: Not helpful alone ✗ → **A**

12. Is x > y? (1) x = 10 (2) y = 5
    > S1: x=10, don't know y ✗ | S2: y=5, don't know x ✗ | Both: 10>5 ✓ → **C**

13. Is triangle right-angled? (1) One angle is 90° (2) Two sides are equal
    > S1: Yes, has 90° → right-angled, definite ✓ | S2: Two equal sides → isoceles, could or couldn't be right ✗ → **A**

14. Is A a doctor? (1) Works in hospital (2) Has medical degree
    > S1: Could be nurse etc. ✗ | S2: Could be researcher ✗ | Both: Hospital + degree → **likely C in exam context**

15. Is number odd? (1) Not divisible by 2 (2) Less than 100
    > S1: Not div by 2 = odd, definite ✓ | S2: Could be any ✗ → **A**

---

## 🔗 Related Topics

- [[LR_04_Syllogism]] — Similar logical evaluation approach
- [[QA_15_Number_System]] — Number properties for type 1 questions
- [[02_Reasoning_Index]] — Reasoning overview

---

*⬅️ [[LR_08_Series]] | ➡️ [[02_Reasoning_Index]]*
