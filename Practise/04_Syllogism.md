# 🧩 Syllogism
### [← Direction Sense](03_Direction_Sense.md) | [🏠 Index](Notes/00_INDEX.md) | *(End of Series)*

---

## 🧒 What is Syllogism? (Feynman Explanation)

Imagine a game with three rules:
1. All cats are animals.
2. All animals need food.
3. So... do cats need food?

**YES!** That's Syllogism — using **logic** to reach conclusions from given statements.

> You're given 2 statements (premises) and asked if certain conclusions are **definitely true**, **definitely false**, or **possibly true**.

The magic tool? **Venn Diagrams** — circles inside circles! 🔵

---

## 🔵 The 4 Types of Statements (Most Important!)

| Type | Statement | Symbol | Meaning |
|------|-----------|--------|---------|
| **A** | All A are B | A → B | A is fully inside B |
| **E** | No A are B | A ✗ B | A and B don't touch |
| **I** | Some A are B | A ∩ B | A and B overlap |
| **O** | Some A are not B | A ≠ B | Part of A is outside B |

### Venn Diagram Pictures:

**All A are B (Type A):**
```
  ┌─────────────┐
  │      B      │
  │  ┌───────┐  │
  │  │   A   │  │
  │  └───────┘  │
  └─────────────┘
  (A is completely inside B)
```

**No A are B (Type E):**
```
  ┌───────┐    ┌───────┐
  │   A   │    │   B   │
  └───────┘    └───────┘
  (Separate circles, no overlap)
```

**Some A are B (Type I):**
```
  ┌─────┐
  │  A  │┌─────┐
  │   ┌─┼┤  B  │
  │   │ ││     │
  └───┼─┘└─────┘
      └─(overlap)
  (Partial overlap)
```

**Some A are not B (Type O):**
```
  ┌──────────┐
  │  A  ┌────┼─────┐
  │ [X] │    │  B  │
  │     └────┼─────┘
  └──────────┘
  (X part of A is outside B)
```

---

## 🟢 BASIC LEVEL

### Type 1 — All + All (Easiest!)

**Rule:**
```
All A are B + All B are C → All A are C ✅ (100% valid)
```

#### Example:
```
Statement 1: All dogs are animals.
Statement 2: All animals are living beings.
Conclusion: All dogs are living beings. → TRUE ✅
```

**Venn Diagram:**
```
┌──────────────────────────┐
│      Living Beings       │
│  ┌───────────────────┐   │
│  │      Animals      │   │
│  │  ┌─────────────┐  │   │
│  │  │    Dogs     │  │   │
│  │  └─────────────┘  │   │
│  └───────────────────┘   │
└──────────────────────────┘
```

---

### Type 2 — All + No

**Rule:**
```
All A are B + No B are C → No A are C ✅
```

#### Example:
```
Statement 1: All pens are books.
Statement 2: No books are chairs.
Conclusion: No pens are chairs. → TRUE ✅
```

---

### Type 3 — Some + All

**Rule:**
```
Some A are B + All B are C → Some A are C ✅
```

#### Example:
```
Statement 1: Some cats are dogs.
Statement 2: All dogs are birds.
Conclusion: Some cats are birds. → TRUE ✅
```

---

## 🟡 INTERMEDIATE LEVEL

### The Possibility Rule ⚡

When you're not 100% sure, the answer might be **"possibly true"**.

**Rule for Possibility:**
```
If a conclusion can be TRUE in ANY valid Venn diagram, it is POSSIBLE.
If a conclusion is FALSE in ALL valid Venn diagrams, it is IMPOSSIBLE.
```

#### Example:
```
Statement: Some A are B.
Conclusion: All A are B.

Is this POSSIBLE? YES — because "Some A are B" allows for the case where ALL A happen to be B.
```

---

### Type 4 — Some + No

**Rule:**
```
Some A are B + No B are C → Some A are not C ✅
```

#### Example:
```
Statement 1: Some flowers are trees.
Statement 2: No trees are buildings.
Conclusion: Some flowers are not buildings. → TRUE ✅
```

---

### The Complementary Pair Trick ⚡

In MCQs, two conclusions are sometimes given as a **complementary pair**:
- Conclusion 1: All A are B
- Conclusion 2: Some A are not B

**Rule:** In a complementary pair, **at least one must always be true**.
> This is called **Either/Or follows** — very common in exams!

---

## 🔴 ADVANCED LEVEL

### Type 5 — Three Statement Syllogism

Apply the rules **step by step** with 3 statements:

#### Example:
```
Statement 1: All A are B.
Statement 2: All B are C.
Statement 3: No C are D.

From 1+2: All A are C (All+All rule)
From above + 3: No A are D (All+No rule)

Final Conclusion: No A are D ✅
```

---

### Type 6 — No Valid Conclusion Cases

Sometimes **no conclusion follows**:

| Pattern | Result |
|---------|--------|
| Some A are B + Some B are C | ❌ No definite conclusion |
| Some A are B + No B are C | ✅ Some A are not C |
| All A are B + Some B are C | ❌ No definite conclusion about A and C |

#### ⚡ KEY RULE: "Some + Some = No Conclusion"

---

## ⚡ MASTER SYLLOGISM RULES TABLE

| Statement 1 | Statement 2 | Conclusion | Valid? |
|-------------|-------------|-----------|--------|
| All A → B | All B → C | All A → C | ✅ YES |
| All A → B | No B → C | No A → C | ✅ YES |
| All A → B | Some B → C | Some A → C | ❌ NO |
| Some A → B | All B → C | Some A → C | ✅ YES |
| Some A → B | No B → C | Some A ≠ C | ✅ YES |
| Some A → B | Some B → C | Nothing | ❌ NO |
| No A → B | All B → C | Some C ≠ A | ✅ YES |
| No A → B | Some B → C | Some C ≠ A | ✅ YES |

### ⚡ Memory Shortcut: "SoSo = No Go"
> **Some + Some = No Conclusion** (always!)

---

## 🔁 Conversion Rules (Reversing Statements)

Sometimes you need to **reverse** (convert) a statement to form new conclusions.

| Original | Converted | Type |
|----------|-----------|------|
| All A are B | Some B are A | ✅ Valid |
| No A are B | No B are A | ✅ Valid |
| Some A are B | Some B are A | ✅ Valid |
| Some A are not B | **Cannot be converted** | ❌ Invalid |

#### Example:
```
Given: All cats are animals.
Converted: Some animals are cats. ✅ (Valid conversion)
```

---

## 🎯 EXAM STRATEGY — Step by Step

```
Step 1: Read both statements carefully.
Step 2: Identify the TYPE (All/No/Some/Some-not).
Step 3: Find the COMMON TERM (the middle link).
Step 4: Apply the RULE from the table above.
Step 5: Check each conclusion against the result.
Step 6: If unsure → draw a VENN DIAGRAM!
```

---

## 🧪 Practice Questions

### 🟢 Basic
1. All birds are animals. All animals breathe. Conclusion: All birds breathe. (True/False?)
2. No pens are books. All books are papers. Conclusion: No pens are papers. (True/False?)

### 🟡 Intermediate
3. Some tables are chairs. All chairs are wooden. What follows?
   - (a) Some tables are wooden
   - (b) All tables are wooden
   - (c) No tables are wooden

4. Some apples are oranges. No oranges are grapes. Which conclusion follows?
   - (a) Some apples are grapes
   - (b) Some apples are not grapes
   - (c) No apples are grapes

### 🔴 Advanced
5. All X are Y. Some Y are Z. No Z are W.
   Conclusions:
   - (a) Some X are Z
   - (b) No Y are W
   - (c) Some Y are not W
   - Which follow?

---

## 📝 Answers

1. **TRUE** — All+All → All A are C ✅
2. **FALSE** — No A→B + All B→C → Some C are not A (not "No pens are papers")
3. **(a) Some tables are wooden** — Some+All → Some A are C ✅
4. **(b) Some apples are not grapes** — Some A→B + No B→C → Some A not C ✅
5. 
   - (a) Some X are Z — All+Some ❌ No definite (cannot conclude)
   - (b) No Y are W — Not valid (No Z are W ≠ No Y are W)
   - (c) Some Y are not W — ✅ YES (Some Y are Z, No Z are W → Some Y are not W)

---

## 📊 All Rules at a Glance (1-Page Cheatsheet)

```
┌─────────────────────────────────────────────────┐
│           SYLLOGISM QUICK CHEATSHEET            │
├─────────────────────────────────────────────────┤
│ All + All   = All          ✅                   │
│ All + No    = No           ✅                   │
│ All + Some  = No conclusion ❌                  │
│ Some + All  = Some         ✅                   │
│ Some + No   = Some-Not     ✅                   │
│ Some + Some = No conclusion ❌ (SoSo = No Go)   │
│ No + All    = Some-Not (reversed) ✅            │
│ No + Some   = Some-Not (reversed) ✅            │
├─────────────────────────────────────────────────┤
│ CONVERSION:                                     │
│ All A→B  becomes  Some B→A  ✅                  │
│ No A→B   becomes  No B→A   ✅                   │
│ Some A→B becomes  Some B→A ✅                   │
│ Some A not B = CANNOT convert ❌                │
├─────────────────────────────────────────────────┤
│ COMPLEMENTARY PAIR:                             │
│ "All A are B" + "Some A are not B"              │
│ → Either-or follows ✅                          │
└─────────────────────────────────────────────────┘
```

---

## 🔗 Navigation

| ← Previous | 🏠 Home | Next → |
|------------|---------|--------|
| [Direction Sense](03_Direction_Sense.md) | [Index](Notes/00_INDEX.md) | *(You've completed all topics! 🎉)* |

---

> 🏆 **Congratulations!** You've completed the full Aptitude Master Notes series!
> Now go back to [the Index](Notes/00_INDEX.md) and review your weak topics. Practice daily and you'll solve these in seconds! 💪
