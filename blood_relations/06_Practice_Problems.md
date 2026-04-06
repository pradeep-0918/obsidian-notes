# 06 — 💪 Practice Problems (Easy → Pro)

> **← [[05_Coded_Blood_Relations]] | Back to → [[Notes/00_INDEX]]**

---

## 🎯 How to Use This File

- **Solve each question on paper first** — don't peek at answers!
- Use the techniques from [[04_Shortcuts_and_Tricks]]
- For coded questions, refer to [[05_Coded_Blood_Relations]]
- Track your time: target **under 30 seconds per question**

---

## 🟢 LEVEL 1 — Easy (Questions 1–10)

---

**Q1.** Pointing to a man, a woman says, "His mother is the only daughter of my mother." How is the woman related to the man?

<details>
<summary>💡 Solution</summary>

"Only daughter of my mother" = The woman herself.
So man's mother = the woman → The woman is the man's **Mother**.

✅ **Answer: Mother**
</details>

---

**Q2.** A is the father of B. C is the daughter of A. D is the brother of E. E is the daughter of B. Who is the grandmother of D?

<details>
<summary>💡 Solution</summary>

Tree:
```
A
├── B (son)
│   └── E (daughter of B) = D's sister
│       D (brother of E = son of B)
└── C (daughter of A)
```
D is B's son → B is D's father → A is D's grandfather → A's wife is **Grandmother of D**.
But A's wife is not named. We know C is A's daughter.

Wait — who is D's grandmother? D is E's brother, E is daughter of B, B is son of A.
So D's grandmother = A's wife = B's mother. Not named in question.

✅ **Answer: Cannot be determined** (A's wife is not mentioned)

> ⚠️ Lesson: Always check if every person needed is actually named!
</details>

---

**Q3.** Ramu's mother is the wife of Sita's father. Sita has no brothers. How is Ramu related to Sita?

<details>
<summary>💡 Solution</summary>

- Sita's father's wife = Ramu's mother
- So Sita's father married Ramu's mother → They are step-siblings OR siblings
- Sita has no brothers → Ramu is not Sita's brother
- Most likely: Ramu is Sita's **step-brother** or the question means they have the same parents

✅ **Answer: Brother (or Step-brother)**
</details>

---

**Q4.** If A + B means A is the mother of B, and A − B means A is the brother of B, then what does P + Q − R mean?

<details>
<summary>💡 Solution</summary>

- P + Q → P is mother of Q
- Q − R → Q is brother of R
- Q is both son of P AND brother of R
- So R is also a child of P

Tree:
```
P (mother)
├── Q (son)
└── R (Q's sibling)
```
P is R's mother → **P is the mother of R**

✅ **Answer: P is the mother of R**
</details>

---

**Q5.** Mohan is the son of Arun's father's sister. How is Arun related to Mohan?

<details>
<summary>💡 Solution</summary>

- Arun's father's sister = Arun's **Paternal Aunt**
- Mohan is the son of Arun's Paternal Aunt
- So Mohan is Arun's **Cousin**

✅ **Answer: Cousin**
</details>

---

**Q6.** Introducing a girl, a boy said, "She is the daughter of the only son of my grandfather." How is the girl related to the boy?

<details>
<summary>💡 Solution</summary>

- Boy's grandfather's only son = Boy's **Father**
- Father's daughter = Boy's **Sister**

✅ **Answer: Sister**
</details>

---

**Q7.** P is Q's brother. R is Q's mother. S is R's father. T is S's mother. How is P related to T?

<details>
<summary>💡 Solution</summary>

Tree:
```
T (great-grandmother)
└── S (grandfather)
    └── R (mother)
        └── Q ── P (brother)
```
P is Q's brother → P is also R's son → R's mother is S's daughter → T is S's mother.
P is T's great-grandson.

✅ **Answer: Great-grandson**
</details>

---

**Q8.** A woman says, "The father of my son's wife is my father's son." What is the relationship between the woman's son and her father's son?

<details>
<summary>💡 Solution</summary>

- Woman's son's wife's father = Woman's father's son
- Woman's father's son = Woman's **Brother**
- So woman's son's wife's father = Woman's brother
- Woman's son married Woman's brother's daughter
- So: Woman's son married his **cousin**

The relationship between woman's son and her father's son:
- Her son is her brother's nephew → Her son and her brother's daughter are cousins

✅ **Son-in-law** (her father's son is her son's father-in-law)
</details>

---

**Q9.** Among five people A, B, C, D, E — B is A's daughter. C is B's son. D is C's father. E is C's mother. How is A related to E?

<details>
<summary>💡 Solution</summary>

- B is A's daughter → A is B's parent
- C is B's son → B is C's mother
- D is C's father → D is B's husband
- E is C's mother → E = B (since B is already C's mother)

Wait: E is C's mother AND B is C's mother → **E = B**

A is B's parent → A is E's parent → **A is E's mother or father**

✅ **Answer: Mother or Father (Parent)** — Gender of A not given so "Cannot be fully determined" but A is E's parent.
</details>

---

**Q10.** Which of the following means "M is the maternal uncle of N"?
- (a) N is the brother of P, P is the son of M
- (b) M is the brother of Q, Q is the mother of N
- (c) M is the son of R, R is the mother of N
- (d) M is the father of Q, Q is the sister of N

<details>
<summary>💡 Solution</summary>

Maternal uncle = Mother's brother.
Check option (b): M is brother of Q, Q is mother of N → M is N's mother's brother = **Maternal Uncle** ✅

✅ **Answer: (b)**
</details>

---

## 🟡 LEVEL 2 — Medium (Questions 11–20)

---

**Q11.** Read carefully:
- A is the father of C
- B is the wife of A
- E is the son of D
- D is the daughter of C

How is B related to E?

<details>
<summary>💡 Solution</summary>

Tree:
```
B ══ A
     └── C
         └── D (daughter of C)
             └── E (son of D)
```
B is wife of A → B is C's mother → B is D's grandmother → B is E's great-grandmother.

✅ **Answer: Great-grandmother**
</details>

---

**Q12.** Coded: A @ B = A is mother of B | A # B = A is husband of B | A % B = A is sister of B
If P @ Q # R % S, how is P related to S?

<details>
<summary>💡 Solution</summary>

- P @ Q → P is mother of Q
- Q # R → Q is husband of R (Q is male)
- R % S → R is sister of S

Tree:
```
P (female - mother)
└── Q (male)
    └── married to R (female)
    R ── S (R's sibling)
```
P is Q's mother. R is S's sister. Q is married to R.
P's relation to S: P is Q's mother, Q is R's husband, R is S's sister.
P is S's **mother-in-law's... wait: P is Q's mother. R is Q's wife. S is R's sibling.
P is Q's mother → P is R's mother-in-law → P is S's (sibling-in-law's mother)

No standard term. ✅ **Answer: Cannot be determined / No direct standard relation**

> But if exam asks "P is related to S as?" — check options. Closest: Mother-in-law's mother level.
</details>

---

**Q13.** There are six members: A, B, C, D, E, F.
- C is the sister of A
- A is married to D
- D is the son of B
- E is the father of C
- F is the mother of D

How many female members are there?

<details>
<summary>💡 Solution</summary>

- A married to D → one is M, one is F
- D is son of B → D is **Male**
- So A is **Female** (since A married D who is male)
- C is sister of A → C is **Female**
- E is father of C → E is **Male**
- F is mother of D → F is **Female**
- B: D is son of B → B is parent of D. F is mother of D → B could be father → **B is Male**

Females: A, C, F → **3 females**

✅ **Answer: 3**
</details>

---

**Q14.** Pointing to a photograph, Suresh says, "The lady in the photograph is my nephew's maternal grandmother." How is the lady related to Suresh's sister who has no other sibling?

<details>
<summary>💡 Solution</summary>

- Suresh's sister has no other sibling → Suresh's nephew = Sister's son
- Nephew's maternal grandmother = Nephew's mother's mother = Sister's mother
- Sister's mother = Suresh's **mother** (since they're siblings)

The lady is Suresh's mother. Suresh's sister's mother = **Mother**

✅ **Answer: Mother**
</details>

---

**Q15.** If "A ★ B" means A is the brother of B, "A ♦ B" means A is the father of B, and "A ♣ B" means A is the daughter of B, then what does X ★ Y ♦ Z ♣ W mean for the relation between X and W?

<details>
<summary>💡 Solution</summary>

- X ★ Y → X is brother of Y (X = Male)
- Y ♦ Z → Y is father of Z
- Z ♣ W → Z is daughter of W

Tree:
```
W
└── Z (daughter of W)
    ↑ But Y is also father of Z
    
So: Y and W are Z's parents → Y ══ W (married couple)
But X is brother of Y...

Final tree:
    W ══ Y
         └── Z
    X (brother of Y)
```
X is Y's brother. Y is Z's father. Z is W's daughter. Y and W are married (both Z's parents).

X's relation to W: X is Y's brother, Y is W's husband → X is W's **Brother-in-law**

✅ **Answer: X is the brother-in-law of W**
</details>

---

**Q16–Q20** — Mixed rapid-fire (answers given directly for speed practice):

| Q | Question | Answer |
|---|----------|--------|
| Q16 | "My mother's brother's only sister" — who is she? | Your Mother |
| Q17 | "Her father's mother's husband" — who is he? | Her Paternal Grandfather |
| Q18 | "His wife's sister's husband" — general relation? | Brother-in-law |
| Q19 | "The son of my father's father" — who? | Father / Uncle |
| Q20 | "My sister's son's wife's mother-in-law" | Your sister's mother = Your mother (if sister has no step-family) |

---

## 🔴 LEVEL 3 — Pro (Questions 21–30)

---

**Q21.** There are 7 people in a family: A, B, C, D, E, F, G
- A and B are a married couple
- C is the son of B
- D is the brother of A
- E is the daughter of C
- F is the wife of D
- G is the grandfather of E and husband of B's mother

Find: How is G related to A?

<details>
<summary>💡 Solution</summary>

Tree building:
- A ══ B (married)
- C is son of B → C is also A's son (assuming)
- D is brother of A → D and A are siblings
- E is daughter of C → E is A's granddaughter
- F is wife of D → F and D are married
- G is grandfather of E → G is parent of C's parent → G is A's or B's parent
- G is husband of B's mother → G ══ B's mother → G is B's father

So G is B's father. B is A's wife. G is A's **Father-in-law**.

✅ **Answer: Father-in-law**
</details>

---

**Q22.** Coded (Hard):
Key: M÷N = M is son of N | M+N = M is wife of N | M×N = M is daughter of N | M−N = M is brother of N

If A÷B+C×D−E, how is A related to E?

<details>
<summary>💡 Solution</summary>

- A÷B → A is son of B (A=Male, B=parent)
- B+C → B is wife of C (B=Female — ✅ consistent, C=Male)
- C×D → C is daughter of D (C=Female — ❌ contradiction! C was male above)

Contradiction in the chain → **Data inconsistent / Cannot be determined**

✅ **Answer: The data is inconsistent**
</details>

---

**Q23–Q30** — Exam-Style Questions (solve independently, check with [[04_Shortcuts_and_Tricks]]):

| Q | Question | Answer |
|---|----------|--------|
| Q23 | A is B's sister. C is B's mother. D is C's father. E is D's mother. How is A related to D? | Granddaughter |
| Q24 | Pointing to Kiran, Smita said, "She is the daughter of my grandfather's only son." How is Smita related to Kiran? | Sister |
| Q25 | If X+Y means X is the father of Y, X-Y means X is the sister of Y, X*Y means X is the wife of Y, then Z*Y+A-B. How is Z related to B? | Mother |
| Q26 | A family has grandfather, grandmother, 2 sons and their wives, and 4 grandchildren. How many females at minimum? | 3 (grandmother + 2 daughters-in-law; grandchildren gender unspecified) |
| Q27 | "He is the son of the man who married my mother's only sister's daughter." Who is "he" to the speaker? | Cousin's son = 1st cousin once removed |
| Q28 | Rahul says "This girl is the wife of the grandson of my mother." Who is Rahul to the girl? | Grandfather-in-law |
| Q29 | In a family, A is the mother-in-law of B. B is the sister-in-law of C. C is the brother of D. D is the son of A. How is B related to D? | Sister-in-law |
| Q30 | "My father's wife's father's wife" — what relation? | Maternal great-grandmother (Mother's mother) — trick: father's wife = mother, mother's father's wife = maternal grandmother |

---

## 📊 Score Tracker

| Level | Questions | Your Score |
|-------|-----------|-----------|
| 🟢 Easy | Q1–Q10 | __ / 10 |
| 🟡 Medium | Q11–Q20 | __ / 10 |
| 🔴 Pro | Q21–Q30 | __ / 10 |
| **Total** | | **__ / 30** |

---

## 🎯 Score Interpretation

| Score | Level | Action |
|-------|-------|--------|
| 0–10 | Beginner | Re-read [[01_Basics]] and [[02_Family_Tree_Chart]] |
| 11–20 | Intermediate | Review [[04_Shortcuts_and_Tricks]] |
| 21–25 | Advanced | Practice timed questions |
| 26–30 | Pro | You're exam-ready! 🏆 |

---

> 🎉 **Congratulations! You've completed the full Blood Relations vault!**

*Return to → [[Notes/00_INDEX]] | Shortcuts → [[04_Shortcuts_and_Tricks]]*
