# 🔡 Coding-Decoding
> Part of [[02_Reasoning_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐ Easy | **Exam Weight:** High | **Time per Q:** 45 sec

---

## 🧠 Concept

A secret code replaces each letter/number with another using a fixed rule.
**Your job:** Find the RULE, then apply it.

---

## 📌 Key Patterns

### Pattern 1: Letter Shift (+n)
APPLE → BQQMF (each letter + 1)
> A+1=B, P+1=Q, P+1=Q, L+1=M, E+1=F ✓

### Pattern 2: Reverse Alphabet
A=Z, B=Y, C=X... (A↔Z, B↔Y, etc.)
> Position: A=1, Z=26; A's reverse = 26th = Z
> Formula: Reverse of letter at position p = letter at position (27−p)

### Pattern 3: Letter Position Numbers
A=1, B=2, ..., Z=26
FLOW → 6,12,15,23

### Pattern 4: Skip pattern
A→C→E (every other letter, +2 shift)

### Pattern 5: Mixed (letters + positions)
FLOW → 6M4P8 (F=6, L=position12 replaced, O=15...) — decode the specific rule

---

## 🛠️ Step-by-Step Approach

1. **Check +1, +2, +3 shifts** (most common)
2. **Check reverse alphabet**
3. **Try letter positions**
4. **Look for alternating patterns**
5. **Check if vowels/consonants treated differently**

---

## 💡 Quick Reference

```
A=1  B=2  C=3  D=4  E=5  F=6  G=7  H=8  I=9
J=10 K=11 L=12 M=13 N=14 O=15 P=16 Q=17 R=18
S=19 T=20 U=21 V=22 W=23 X=24 Y=25 Z=26

Reverse alphabet:
A↔Z  B↔Y  C↔X  D↔W  E↔V  F↔U  G↔T  H↔S  I↔R
J↔Q  K↔P  L↔O  M↔N
```

---

## 🧩 Practice Questions

### Medium
1. APPLE → BQQMF. ORANGE → ?
   > +1 shift: O+1=P, R+1=S, A+1=B, N+1=O, G+1=H, E+1=F → **PSBOHF**

2. STAR → UVBS. MOON → ?
   > S+2=U, T+2=V, A+2=C (but it shows B...) wait: S=19→U=21(+2), T=20→V=22(+2), A=1→B=2(+1)?
   > Actually: STAR backwards = RATS; then +1 each? R+1=S, A+1=B... no.
   > STAR→UVBS: S→U(+2), T→V(+2), A→B(+1), R→S(+1)? Inconsistent. 
   > Try: Reverse STAR = RATS; R→U? No. 
   > Try position: S=19,T=20,A=1,R=18; Code: U=21,V=22,B=2,S=19; Differences: +2,+2,+1,+1. Pattern!
   > MOON: M=13,O=15,O=15,N=14; Apply +2,+2,+1,+1: O=15,Q=17,P=16,O=15 → **OQPO**

3. KING → LJOH. QUEEN → ?
   > K+1=L, I+1=J, N+1=O, G+1=H → +1 shift
   > Q+1=R, U+1=V, E+1=F, E+1=F, N+1=O → **RVFFO**

4. SUN → TVO. MOON → ?
   > S+1=T, U+1=V, N+1=O → +1; M+1=N, O+1=P, O+1=P, N+1=O → **NPPO**

5. 12345 → 23456. 67890 → ?
   > Each digit +1; 6→7, 7→8, 8→9, 9→0, 0→1 → **78901**

### Difficult
6. COMPUTER → DPNAQVUF. Rule?
   > C→D(+1), O→P(+1), M→N(+1), P→A(??)... 
   > Actually let's verify: C=3→D=4(+1), O=15→P=16(+1), M=13→N=14(+1), P=16→A=1(reverse of P=16? 27-16=11=K, not A)...
   > Pattern: Even positions reversed? C(1)→D, O(2)→P, M(3)→N, P(4)→Q, U(5)→V, T(6)→U, E(7)→F, R(8)→S → This gives DNQUVFR... not matching.
   > Try: Rearrange COMPUTER by reversing pairs: CO→OC, MP→PM, UT→TU, ER→RE = OCPMTURE then +1 each? O+1=P,C+1=D,P+1=Q,M+1=N,T+1=U,U+1=V,R+1=S,E+1=F = PDQNUVSF... not matching.
   > **Strategy: In exam, try systematic +1/-1 first, note the pattern given.**

---

## 🔗 Related Topics
- [[LR_08_Series]] — Letter series uses same positional logic
- [[LR_07_Analogies]] — Letter analogy pattern recognition

---

*⬅️ [[02_Reasoning_Index]] | ➡️ [[LR_02_Blood_Relations]]*
