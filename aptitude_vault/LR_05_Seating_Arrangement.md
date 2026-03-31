# 🪑 Seating Arrangement
> Part of [[02_Reasoning_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐⭐⭐ Hard | **Exam Weight:** Very High | **Time per Q:** 3–5 min (set)

---

## 🧠 Concept

Seating arrangement questions give clues about positions of people and ask you to determine:
- Who sits where?
- Who is to the left/right of whom?
- Who is opposite whom?

**Golden Rule: BUILD A TABLE/GRID FIRST. Place definite clues first. Eliminate impossible options.**

---

## 📌 Key Types

### Type 1: Linear (Row) Arrangement
People sit in a straight line.
- "Left" and "Right" are absolute directions.
- Draw: Position 1 (leftmost) → Position n (rightmost)

### Type 2: Circular Arrangement
People sit around a circular table.
- "Left" = anti-clockwise direction when looking from above
- "Right" = clockwise direction when looking from above
- "Opposite" = person diametrically across

### Type 3: Double Row / Facing Each Other
Two rows facing each other.
- Row 1 faces North, Row 2 faces South.
- The person "opposite" in the other row is directly facing you.

---

## 🛠️ Step-by-Step Approach

### For Linear Arrangement:
1. Draw a row with numbered positions
2. Place people with **definite positions** first (e.g., "A is at position 3")
3. Use **relative clues** next (e.g., "B is immediately left of C")
4. Use **negative clues** last (e.g., "D is not at the ends")
5. Test and eliminate until one valid arrangement exists

### For Circular Arrangement:
1. Draw a circle with n chairs
2. Fix one person first (as reference point — place A at top)
3. Fill neighbors using clues
4. Work clockwise or counter-clockwise consistently
5. Verify all clues are satisfied

---

## 🔥 Clue Types and How to Use Them

### Definite Position Clues (Use FIRST)
> "A sits at position 3" / "A sits at the center"
- Directly place A on the diagram

### Immediate Neighbor Clues (Use SECOND)
> "B sits immediately to the right of C"
- BC are adjacent pair; place them together

### Gap Clues (Use SECOND)
> "Exactly 2 people sit between A and B"
- Count positions; multiple possibilities may exist — list all

### Opposite Clues (Use for Circular)
> "A is opposite to B"
- In 6-person table: A and B are 3 seats apart

### Negative Clues (Use LAST)
> "A is not at the ends"
- Eliminate end positions for A; use to confirm among remaining options

---

## 💡 Key Shortcuts

### For 6-Person Circular Table
- "Opposite" means 3 seats apart
- If A is at position 1, opposite = position 4
- Immediate neighbors of position 1 = positions 2 and 6

### For 5-Person Circular Table
- No exact "opposite" (odd number)
- "Two seats to the left" = 2 anti-clockwise

### For Row with n People
- "Immediately to the left/right" = adjacent
- "Second to the left of A" = 2 positions left of A
- "At the extreme left end" = position 1

---

## ⚠️ Common Mistakes

1. **Left/Right confusion in circular**
   - In circular, LEFT of A (when A faces center) = anti-clockwise from A
   - But "to the right of A when facing North" = East

2. **Not fixing reference point in circular**
   - Always fix one person first, then fill around

3. **Skipping negative clues until stuck**
   - They're often the key to resolving ambiguity

4. **Confusing "between" in linear vs circular**
   - Linear: Between A and B = positions strictly between them on the line
   - Circular: Can go either way (clockwise or anti-clockwise) — use the count that satisfies clues

5. **Verification:** Always re-check ALL clues against your final arrangement!

---

## 🧩 Practice Questions

### Linear - Medium
**Set 1:** Five people A, B, C, D, E sit in a row.
- A is to the left of B
- C is to the right of D
- E is not at the ends

1. Who is in the middle?
2. Who could be at position 1?

**Solution:**
> E is not at ends → E is in position 2, 3, or 4.
> A...B (A left of B) and D...C (D left of C)
> Possible: D, A, E, C/B... let's try: D, A, E, B, C? Check: A left of B ✓, C right of D ✓, E not at ends ✓
> **Middle = E, Position 1 = D**

**Set 2:** Six people P, Q, R, S, T, U sit in a row.
- P is to the left of Q
- R is to the right of S
- T is not at the ends
- U is at position 3

3. Who is in the middle positions?
4. Who cannot be at position 1?

**Solution:**
> U at position 3. T not at 1 or 6. P...Q and S...R.
> Try: P, S, U, R, T, Q? Check: P left of Q ✓, R right of S ✓, T not at ends ✓
> **U and R in middle area; T, U, P cannot be at position 1 (T:not ends, U:fixed at 3, P: P<Q)**

---

### Circular - Medium
**Set 3:** Six people P, Q, R, S, T, U sit around circular table.
- P is opposite Q
- R is to the left of S

5. Who is opposite R?
6. Who is to the left of P?

**Solution:**
> 6 people → Positions 1–6; P opposite Q → P at 1, Q at 4.
> R is left of S (R, S are adjacent with R one position CCW from S)
> Possible placement: Fill remaining {R,S,T,U} at positions 2,3,5,6
> If R at 2, S at 3 (R is left of S, i.e., R=2 and S=3 going clockwise): T and U at 5,6
> Opposite R(position 2) = position 5 → **T or U is opposite R**
> **T or U is to the left of P** (position 6 is left of position 1)

---

### Complex Arrangement - Hard
**Set 4:** Six people A, B, C, D, E, F sit around circular table.
- A is opposite B
- C is to the left of D
- E is not next to F
- F is not next to A

Find: Who is opposite E?

**Solution:**
> Fix A at position 1; B opposite = position 4.
> Remaining: C, D, E, F at positions 2, 3, 5, 6.
> C is left of D: clockwise order has C before D, so (C,D) = (2,3) or (5,6)
> E is not next to F; F is not next to A (positions 2 and 6 are next to A at 1)
> → F is not at 2 or 6.
> If C,D at 2,3: Remaining E,F at 5,6. F not at 6 (next to A at 1, going 6→1). F at 5; E at 6.
>   Check E not next to F: E at 6, F at 5 → they ARE next to each other! ❌
> If C,D at 5,6: Remaining E,F at 2,3. F not at 2 (next to A at 1). F at 3; E at 2.
>   Check E not next to F: E at 2, F at 3 → they ARE next to each other! ❌
> Try C,D at 3,5 (not adjacent, with gap): C left of D means clockwise C comes before D...
>   Positions clockwise: 1(A),2,3,4(B),5,6. C at 3, D at 5 means going 3→4→5 (C is 2 left of D) — "left of" doesn't mean immediately left.
>   "C is to the left of D" could mean anywhere left (CCW) of D.
>   C at 2, D at 5: F at 3 or 6. F not at 6. F at 3. E at 6.
>   E(6) next to F(3)? No — 6 is adjacent to 1 and 5; F is at 3 ✓. F(3) next to A(1)? 3 is adjacent to 2 and 4 ✓
>   **Valid: A=1, C=2, F=3, B=4, D=5, E=6**
>   Opposite E(6) = position 3 → **F is opposite E**

---

## 🔗 Related Topics

- [[LR_06_Puzzles]] — Similar systematic approach needed
- [[LR_02_Blood_Relations]] — Sometimes combined with seating
- [[02_Reasoning_Index]] — Reasoning overview

---

*⬅️ [[LR_04_Syllogism]] | ➡️ [[LR_06_Puzzles]]*
