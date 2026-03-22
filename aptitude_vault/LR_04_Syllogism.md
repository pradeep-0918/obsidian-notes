# 🔵 Syllogism
> Part of [[02_Reasoning_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Very High | **Time per Q:** 60 sec

---

## 🧠 Concept

Syllogism tests **logical deduction** — given statements, determine which conclusions MUST be true.

**Key principle:** You must accept statements as TRUE (even if they seem false in real life) and check if conclusions NECESSARILY follow.

> "All cats are dogs. All dogs are fish." → Even though absurd, "All cats are fish" is TRUE logically!

---

## 📌 The Four Statement Types

| Type | Form | Symbol | Venn Diagram |
|---|---|---|---|
| Universal Positive | All A are B | A+ | A circle completely inside B |
| Universal Negative | No A is B | A− | A and B circles separated |
| Particular Positive | Some A are B | I | A and B partially overlapping |
| Particular Negative | Some A are not B | O | A partially outside B |

---

## 🛠️ The Venn Diagram Method

### How to Draw

**"All A are B":**
```
  ┌──────────────┐
  │      B       │
  │  ┌───────┐   │
  │  │   A   │   │
  │  └───────┘   │
  └──────────────┘
```
→ A is a subset of B. Every A is inside B.

**"No A is B":**
```
  ┌───────┐    ┌───────┐
  │   A   │    │   B   │
  └───────┘    └───────┘
```
→ A and B are completely separate.

**"Some A are B":**
```
  ┌──────────────┐
  │  A  │  B    │
  │   ██│██     │
  └──────────────┘
```
→ A and B overlap at least partially.

**"Some A are not B":**
```
  ┌─────────┐
  │  A   │B │
  │  ██  ├──┤
  │      │  │
  └─────────┘
```
→ Part of A is outside B.

---

## 🔥 Conclusion Rules (CRITICAL)

### Rule 1: All + All → All
> All A are B. All B are C. → **All A are C.** ✓

### Rule 2: All + No → No
> All A are B. No B is C. → **No A is C.** ✓

### Rule 3: Some + All → Some
> Some A are B. All B are C. → **Some A are C.** ✓

### Rule 4: Some + No → Some not
> Some A are B. No B is C. → **Some A are not C.** ✓

### Rule 5: All + Some → No definite conclusion
> All A are B. Some B are C. → **Cannot conclude anything definite about A and C.**

### Rule 6: Some + Some → No definite conclusion
> Some A are B. Some B are C. → **Cannot conclude A and C are related.**

### Rule 7: No + No → No definite conclusion
> No A is B. No B is C. → **Cannot conclude about A and C.**

---

## 💡 Quick Validity Checklist

For a conclusion to be valid:
1. Draw Venn diagrams for all statements
2. Check: Does the conclusion ALWAYS hold for ALL possible valid diagrams?
3. If even ONE valid diagram violates the conclusion → conclusion is FALSE
4. "Possibility" conclusions (Some A may be C) → True if not contradicted

---

## 🔥 Types of Conclusions

### Type 1: Direct Conclusion
> Statements: All books are pens. Some pens are pencils.
> Conclusion: Some books are pencils.
> **Answer: Invalid** (Rule 5: All+Some → No conclusion)

### Type 2: Complementary Pair
> Conclusions: I. Some A are B. II. No A is B.
> If neither is definitively true, check if they form a complementary pair (one must be true):
> Some A are B AND No A are B are complementary → **One must follow** (called "Either-Or")

### Type 3: Reverse Conclusion
> Statement: All A are B. Conclusion: All B are A.
> **Invalid!** Just because all A are in B doesn't mean all B are A.

### Type 4: Combination
> All A are B. All A are C. → Some B are C? 
> **Valid!** (Since A is in both, B and C overlap through A at minimum)

---

## ⚠️ Common Mistakes

1. **Real-world interference**
   - "All men are dogs" seems wrong, but in syllogism, accept as true.

2. **Reversing "All"**
   - "All A are B" does NOT mean "All B are A"

3. **Confusing "Some" and "All"**
   - "Some A are B" means at least one, possibly all

4. **Not checking BOTH conclusions**
   - In either-or questions, both might be wrong, or exactly one right

5. **Forgetting indirect conclusions**
   - Chain through multiple statements to reach conclusion

---

## 🧩 Practice Questions

### Medium
1. All books are pens. Some pens are pencils.
   Conclusions: I. Some books are pencils. II. Some pencils are books.
   > I: Books-pens-pencils: Some(not All) pens are pencils, but books are all-pens; Can't guarantee books touch pencils → **Invalid**
   > II: Since some pens are pencils and all books are pens: books are a subset of pens. Some pencils overlap with pens but not necessarily books → **Invalid**
   > **Neither follows**

2. All cats are dogs. All dogs are animals.
   Conclusions: I. All cats are animals. II. All animals are dogs.
   > I: All cats→dogs→animals, so all cats are animals → **Valid (follows)**
   > II: We only know dogs are animals, but animals may not all be dogs → **Invalid**

3. Some A are B. All B are C. Some C are D.
   Conclusions: I. Some A are C. II. Some D are A.
   > I: Some A are B, All B are C → Some A are C → **Valid**
   > II: Some C are D, Some A are C → A and D may or may not overlap → **Invalid**

4. All roses are flowers. Some flowers are red.
   Conclusions: I. Some roses are red. II. Some red are flowers.
   > I: All roses are flowers, some flowers are red — roses might be in the non-red part → **Invalid**
   > II: Some flowers are red → reversing "some" → Some red are flowers → **Valid**

5. No pen is marker. Some markers are blue.
   Conclusions: I. No pen is blue. II. Some blue are not pens.
   > I: Pens and markers are separate. Some markers are blue. But blue things could still include pens (pens and blue are not restricted) → **Invalid**
   > II: Since some markers are blue, and no marker is a pen, those blue markers are not pens → **Valid**

6. All students are educated. Some educated are teachers.
   I. Some students are teachers. II. All educated are students.
   > I: Students are educated; some educated are teachers; teachers could be in the non-student educated part → **Invalid**
   > II: All students are educated, but educated doesn't have to be all students → **Invalid**
   > **Neither follows**

### Difficult
1. Some A are B. All B are C. Some C are D. No D is E.
   Conclusions: I. Some A are D. II. Some C are not E.
   > I: Some A→B→C; Some C are D. A and D: A is a subset of C via B, but only "some C are D" — might not include the A-part → **Invalid (not definite)**
   > II: Some C are D, No D is E → Some C (those that are D) are not E → **Valid**

2. All X are Y. Some Y are Z. No Z is W. Conclusions: I. No X is W. II. Some Y are not W.
   > I: All X are Y; Some Y are Z; No Z is W. X⊆Y. Some Y are Z which are not W. But X might be in the Y-not-Z portion → **Invalid (cannot conclude No X is W)**
   > II: Some Y are Z, No Z is W → those Z-type Y are not W → Some Y are not W → **Valid**

3. No X is Y. Some Y are Z. All Z are W.
   I. No X is Z. II. Some W are Y.
   > I: X and Y are separate. Z overlaps with Y. But X and Z: Not directly related — X could overlap with Z if Z extends beyond Y... but wait "Some Y are Z" doesn't mean all Z are Y. X and Z might still intersect. → **Invalid (uncertain)**
   > II: Some Y are Z, All Z are W → Some Y are Z→W, so Some Y are W → Some W are Y (reverse of some) → **Valid**

---

## 🔗 Related Topics

- [[LR_09_Data_Sufficiency]] — Uses similar logical evaluation
- [[LR_06_Puzzles]] — Complex logic applies
- [[02_Reasoning_Index]] — Reasoning overview

---

*⬅️ [[LR_03_Direction_Sense]] | ➡️ [[LR_05_Seating_Arrangement]]*
