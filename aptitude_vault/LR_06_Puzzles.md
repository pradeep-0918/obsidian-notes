# 🧩 Puzzles
> Part of [[02_Reasoning_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐⭐⭐ Hard | **Exam Weight:** High

---

## 🧠 Concept

Logic puzzles give you a set of clues and ask you to match multiple attributes (people ↔ jobs ↔ colors ↔ floors etc.).

**The Method: Always use a GRID TABLE.**

---

## 🛠️ The Grid Method

**Example:** 4 people (A, B, C, D) have jobs (doctor, engineer, lawyer, teacher) and pets (cat, dog, bird, fish).

Create a grid:

| Person | Job | Pet |
|---|---|---|
| A | ? | ? |
| B | ? | ? |
| C | ? | ? |
| D | ? | ? |

OR use a cross-reference grid:

| | Doctor | Engineer | Lawyer | Teacher |
|---|---|---|---|---|
| A | ? | ? | ? | ? |
| B | ? | ? | ? | ? |
| C | ? | ? | ? | ? |
| D | ? | ? | ? | ? |

Fill ✓ (yes) and ✗ (no) from clues. Each row/column has exactly one ✓.

---

## 🔥 Clue Types

1. **Direct:** "A is the doctor" → A=Doctor ✓, A≠ all others
2. **Negative:** "B is not the engineer" → B≠Engineer ✗
3. **Conditional:** "If A is married, then A is the teacher" → Use to eliminate
4. **Relative:** "The engineer is older than the lawyer" → Combined with age data
5. **Connecting:** "The doctor has a cat" → Links two attributes

---

## 🛠️ Step-by-Step

1. Create the grid
2. Apply **direct clues** immediately (fill ✓ and corresponding ✗)
3. Apply **negative clues** (fill ✗)
4. Look for cells where **only one option remains** in a row/column → fill ✓
5. Apply **conditional clues** using known ✓ values
6. Repeat until all cells filled

---

## 🧩 Practice Questions

### Easy Set
**People:** A, B, C, D | **Jobs:** Doctor, Lawyer, Teacher, Engineer

Clues: A is not a doctor. B is a lawyer. C is not a teacher.

Solution:
> B=Lawyer (direct)
> A≠Doctor, C≠Teacher
> Remaining jobs for A,C,D: Doctor, Teacher, Engineer
> A≠Doctor → A is Teacher or Engineer
> C≠Teacher → C is Doctor or Engineer
> D gets what's left.
> 
> Try: If C=Doctor → A and D have Teacher/Engineer; A can be either
> If A=Teacher → D=Engineer → **C=Doctor, A=Teacher, D=Engineer** ✓
> If A=Engineer → D=Teacher → **C=Doctor, A=Engineer, D=Teacher** ✓
> (Both valid without more clues — need additional constraint)

### Medium Set
**People:** A, B, C, D | **Professions:** Doctor, Lawyer, Engineer, Teacher | **Traits:** married/unmarried

Clues:
- A is not a doctor
- B is a lawyer
- C is not a teacher
- The engineer is not married
- A is married

Solution:
> B=Lawyer
> A≠Doctor; C≠Teacher; A is married → A≠Engineer (engineer is not married)
> A is married and not doctor and not engineer and B is lawyer → **A is Teacher**
> C≠Teacher(taken by A anyway), C is Doctor or Engineer
> D gets the remaining
> If C=Doctor → D=Engineer; D is not married (engineer not married); **C=Doctor, D=Engineer**
> Final: **A=Teacher, B=Lawyer, C=Doctor, D=Engineer**

### Hard Set
**People:** P,Q,R,S,T | **Sports:** Cricket, Football, Tennis, Badminton, Hockey

Clues:
- W plays cricket
- X does not play football
- Y is next to W (in ranking/age)
- Z is youngest
- V is oldest and plays hockey

(Refer to PDF Q4 in Puzzles Difficult section)

---

## 🔗 Related Topics
- [[LR_05_Seating_Arrangement]] — Grid method same approach
- [[LR_02_Blood_Relations]] — Sometimes embedded in puzzles

---

*⬅️ [[LR_05_Seating_Arrangement]] | ➡️ [[LR_07_Analogies]]*
