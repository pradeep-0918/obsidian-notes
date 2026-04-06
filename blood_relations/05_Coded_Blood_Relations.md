# 05 — 🔐 Coded Blood Relations

> **← [[04_Shortcuts_and_Tricks]] | Next → [[06_Practice_Problems]]**

---

## 🤔 What Are Coded Blood Relations?

Instead of saying "A is the father of B", the exam gives you:
> **"A + B"** where **+** means "is the father of"

Your job: **Decode the symbols → Build the tree → Find the relation**

---

## 🔑 The Most Common Symbol Sets

### Symbol Set 1 — Classic TCS/Infosys Style

| Symbol | Meaning |
|--------|---------|
| **A + B** | A is the Father of B |
| **A − B** | A is the Wife of B |
| **A × B** | A is the Brother of B |
| **A ÷ B** | A is the Daughter of B |

---

### Symbol Set 2 — AMCAT/eLitmus Style

| Symbol | Meaning |
|--------|---------|
| **A @ B** | A is the Mother of B |
| **A $ B** | A is the Husband of B |
| **A # B** | A is the Sister of B |
| **A % B** | A is the Son of B |

---

### Symbol Set 3 — Bank PO Style (Arrow-Based)

| Symbol | Meaning |
|--------|---------|
| **A → B** | A is the parent of B |
| **A ← B** | A is the child of B |
| **A ↔ B** | A and B are siblings |
| **A ↑ B** | A is the spouse of B |

> ⚠️ **Important**: Every exam defines its OWN symbols. **Always read the key first!**
> The symbols above are just common examples. Never assume.

---

## 🛠️ 3-Step Method for Coded Questions

### Step 1: READ THE KEY
Copy the symbol-meaning table on your scratch paper.

### Step 2: SUBSTITUTE
Replace every symbol with its word meaning.

### Step 3: DRAW THE TREE
Build the family tree from the word version, then answer.

---

## 🔥 Solved Examples — Full Walkthrough

### Example 1 (Easy)

> **Key:** A + B = A is father of B | A − B = A is wife of B | A × B = A is brother of B
>
> **Q: If P + Q × R, how is P related to R?**

**Step 1: Substitute**
- P + Q → P is father of Q
- Q × R → Q is brother of R

**Step 2: Draw**
```
    P (father)
    │
    Q ──── R (brothers)
```

**Step 3: Answer**
- P is father of Q
- Q is brother of R
- Therefore P is also **father of R**

✅ **P is the Father of R**

---

### Example 2 (Medium)

> **Key:** A + B = A is father of B | A − B = A is wife of B | A × B = A is brother of B | A ÷ B = A is daughter of B
>
> **Q: If M − N + O ÷ P, how is M related to P?**

**Step 1: Substitute**
- M − N → M is wife of N
- N + O → N is father of O
- O ÷ P → O is daughter of P

**Step 2: Draw**
```
      P
      │
      O (daughter of P)
      │ (wait — O is also child of N?)

Hmm, O ÷ P means O is daughter of P
N + O means N is father of O

So BOTH N and P are parents of O!
→ N and P are O's parents → N and P are married!
But M − N means M is wife of N...

Contradiction! Let me re-read...

Actually: N + O = N is father of O, O ÷ P = O is daughter of P
This means P is O's OTHER parent... but O can have only 2 parents.
→ P = O's Mother (since N is Father)

Tree:
    N (M) ══ M (F)          P = O's mother
         │              
         O (F)              But N + O and O ÷ P → N and P are O's parents

CORRECTED TREE:
    N ══════════ P
    │
    O
And M − N means M is wife of N → M = N's wife = P's... 
Wait, N can't have two wives. 

So P must be the FATHER of O (O ÷ P), and N is also father? No.

Re-read: O ÷ P = O is daughter of P → P is O's parent (could be father or mother)

Since N is already O's father → P is O's MOTHER

And M − N means M is N's wife → M = O's mother too? 

Contradiction → Answer: Cannot be determined / Data inconsistent
```

> ⚠️ **Lesson**: If you get contradictions, check your reading. If it still contradicts → "Data inconsistent / Cannot be determined"

---

### Example 3 (Pro Level — Long Chain)

> **Key:** P = father, Q = mother, R = brother, S = sister, T = son, U = daughter
>
> **Q: If A P B Q C R D, then how is A related to D?**
> *(Here letters between names represent relation)*

**Step 1: Substitute**
- A P B → A is father of B
- B Q C → B is mother of C (Wait — B is male as son of A. Can B be a mother?)

> ⚠️ **Key Insight**: In these questions, check if gender matches the relation!
> If B is A's son (male), B CANNOT be a mother. → "Data inconsistent"

> But if the question says B Q C means "B's mother is C" (reverse reading), then re-decode.

**Always check if the relation direction is A→B or A←B in the key!**

---

## 🧩 Common Symbol Patterns to Memorize

| Pattern | What It Means | Quick Answer |
|---------|--------------|-------------|
| A + B + C | A→father→B→father→C | A is grandfather of C |
| A − B + C | A is wife of B, B is father of C | A is mother of C |
| A × B + C | A is brother of B, B is father of C | A is uncle of C |
| A ÷ B × C | A is daughter of B, B is brother of C | C is A's uncle |
| A + B ÷ C | A is father of B, B is daughter of C | A = C (same person) or A is spouse of C |

---

## 🔁 The Reverse Reading Trap

> Some exams define symbols as "**A + B means B is the father of A**" (reversed!)

**Always double-check the direction!**

"A + B means A is the father of B" → A is ABOVE B in tree
"A + B means B is the father of A" → B is ABOVE A in tree

These give COMPLETELY different trees. Read twice.

---

## 📌 Quick Recap

```
CODED QUESTIONS → 3 Steps:
1. Read the key → write it down
2. Substitute every symbol
3. Draw the tree → find the relation

TRAPS:
✗ Don't assume symbols — always read the key
✗ Check gender matches the assigned relation
✗ Watch for reverse direction definitions
✓ When stuck → "Cannot be determined"
```

---

> **Next → [[06_Practice_Problems]]** — 30 questions, from Easy to Pro! 💪

*Back to → [[Notes/00_INDEX]]*
