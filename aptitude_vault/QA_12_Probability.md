# 🎯 Probability
> Part of [[01_Quant_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐⭐⭐ Hard | **Exam Weight:** Medium

---

## 🧠 Concept

Probability = Likelihood of an event happening
$$P(Event) = \frac{\text{Favorable outcomes}}{\text{Total outcomes}}$$

Range: 0 ≤ P(E) ≤ 1
- P = 0: Impossible
- P = 1: Certain

---

## 📌 Key Formulas

### Complementary
$$P(\bar{A}) = 1 - P(A)$$

### Addition Rule
$$P(A \cup B) = P(A) + P(B) - P(A \cap B)$$

**Mutually exclusive (no overlap):**
$$P(A \cup B) = P(A) + P(B)$$

### Multiplication Rule
**Independent events:**
$$P(A \cap B) = P(A) \times P(B)$$

**Dependent (Conditional):**
$$P(A \cap B) = P(A) \times P(B|A)$$

### Conditional Probability
$$P(B|A) = \frac{P(A \cap B)}{P(A)}$$

---

## 🔥 Common Scenarios

| Scenario | Total Outcomes |
|---|---|
| 1 die | 6 |
| 2 dice | 36 |
| 1 coin | 2 |
| 2 coins | 4 |
| n coins | 2^n |
| Card draw (52 deck) | 52 |
| 2 cards without replacement | 52×51 |

### Cards Breakdown (52 cards)
- 4 suits × 13 cards each = 52
- Suits: Hearts, Diamonds (red), Clubs, Spades (black)
- Face cards: J, Q, K = 12 total
- Aces = 4

---

## 🧩 Practice Questions

### Easy
1. Die rolled. P(6)? → **1/6**
2. Card drawn. P(Ace)? → 4/52 = **1/13**
3. Bag: 5 red, 3 blue. P(red)? → **5/8**
4. Coin tossed 2 times. P(both heads)? → 1/4 → **1/4**
5. Die rolled. P(even)? → 3/6 = **1/2**

### Medium
6. 5R, 3B balls. Two drawn. P(both red)?
   > C(5,2)/C(8,2) = 10/28 = **5/14**
7. Two dice. P(sum=7)?
   > Favorable: (1,6),(2,5),(3,4),(4,3),(5,2),(6,1) = 6; P = 6/36 = **1/6**
8. 4R, 3B, 2G. Two drawn. P(one R, one B)?
   > C(4,1)×C(3,1)/C(9,2) = 12/36 = **1/3**

### Hard
9. 3 balls drawn from 5R,4B,3G. P(all different colors)?
   > C(5,1)×C(4,1)×C(3,1)/C(12,3) = 60/220 = **3/11**
10. P(sum=7 | at least one die shows 4)?
    > P(sum=7 AND one die=4) = P(4,3 or 3,4) = 2/36; P(at least one 4) = 1−(5/6)² = 11/36; P = 2/11

---

## 🔗 Related Topics
- [[QA_08_Permutations_Combinations]] — For counting favorable/total outcomes

---

*⬅️ [[QA_11_SI_CI]] | ➡️ [[QA_13_Percentage]]*
