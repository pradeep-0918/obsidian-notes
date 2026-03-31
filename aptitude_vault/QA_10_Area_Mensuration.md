# 📐 Area & Mensuration
> Part of [[01_Quant_Index]] | 🔙 [[aptitude_vault/00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** Medium

---

## 🧠 Concept

Mensuration = measuring shapes. Two categories:
- **2D:** Area (flat shapes) and Perimeter (boundary length)
- **3D:** Volume (space inside) and Surface Area (outer skin)

---

## 📌 Key Formulas

### 2D Shapes

| Shape | Area | Perimeter |
|---|---|---|
| Rectangle | l × b | 2(l+b) |
| Square | s² | 4s |
| Triangle | ½ × b × h | a+b+c |
| Circle | πr² | 2πr |
| Trapezoid | ½(a+b)×h | sum of sides |
| Parallelogram | b × h | 2(a+b) |

### Special Triangles
- **Equilateral:** Area = (√3/4)a² ; h = (√3/2)a
- **Right triangle:** Area = ½ × leg1 × leg2
- **Heron's formula:** Area = √[s(s-a)(s-b)(s-c)] where s=(a+b+c)/2

### 3D Shapes

| Shape | Volume | Surface Area |
|---|---|---|
| Cube | s³ | 6s² |
| Cuboid | l×b×h | 2(lb+bh+lh) |
| Cylinder | πr²h | 2πr(r+h) |
| Cone | ⅓πr²h | πr(r+l) where l=slant height |
| Sphere | (4/3)πr³ | 4πr² |

### Diagonal Formulas
- Rectangle: d = √(l²+b²)
- Square: d = s√2
- Cuboid: d = √(l²+b²+h²)

---

## 🔥 Types of Questions

### Type 1: Find area/perimeter from given dimensions
> "Rectangle: l=12, b=8. Area?" → 12×8 = **96 sq units**

### Type 2: Find dimension from area/perimeter
> "Square has perimeter 32. Area?" → Side = 8, Area = **64**

### Type 3: Combined shapes (subtract/add)
> "Area between square side 10 and inscribed circle?"
- Square area = 100; Circle: r=5, area = 25π = 78.5; Shaded = 100−78.5 = **21.5**

### Type 4: Ratio of areas when one dimension changes
> "If radius doubles, area becomes?" → Area ∝ r² → **4 times**

### Type 5: Sector area
$$\text{Sector Area} = \frac{\theta}{360} \times \pi r^2$$

---

## 💡 Key Shortcuts

- **Diagonal of square** = side × √2 ≈ side × 1.414
- **Area of equilateral triangle** side a = (√3/4)a²; if a=4: area = 4√3 ≈ 6.93
- When square's diagonal given: side = d/√2; area = d²/2
- **Inscribed circle in square:** r = side/2
- **Circumscribed circle around square:** r = side×√2/2 = side/√2

---

## 🧩 Practice Questions

### Easy
1. Rectangle 10×5. Area? → **50**
2. Square side 8. Area? → **64**
3. Triangle base 6, height 4. Area? → **12**
4. Circle radius 7, π=22/7. Area? → 22/7×49 = **154**
5. Square perimeter 24. Area? → Side=6, Area = **36**

### Medium
6. Rectangle perimeter 40, area 96. Dimensions?
   > 2(l+b)=40 → l+b=20; lb=96; l=12, b=8; **12×8**
7. Triangle sides 5,12,13. Area? → Right triangle! ½×5×12 = **30**
8. Sector radius 7, angle 60°, area? → 60/360 × 22/7 × 49 = **25.67**
9. Square diagonal 10. Area? → 10²/2 = **50**
10. Circle area 154. Radius? → 154=22/7×r²; r²=49; r=**7**

### Hard
11. Triangle sides 13, 14, 15. Area using Heron's?
    > s=21; Area=√(21×8×7×6) = √7056 = **84**
12. Rectangle inscribed in circle radius 5, area 24. Dimensions?
    > Diagonal=10; l²+b²=100; lb=24; (l+b)²=100+48=148; l+b=√148≈12.17... try: 6×4=24, 36+16=52≠100; 3×8=24, 9+64=73≠100; Hmm: solve l+b=s, lb=24; s²=100+2×24=148; l,b=roots of x²−√148 x+24=0 → **~6 and ~4 won't work; actual l,b ≈ 9.4 and 2.55**
13. Sector folded to cone, r=10, angle=120°. Cone base area?
    > Arc length = (120/360)×2π×10 = 20π/3; This becomes circumference of cone base; 2πR=20π/3; R=10/3; Base area = π(10/3)² = **100π/9 ≈ 34.9**

---

## 🔗 Related Topics
- [[QA_12_Probability]] — Area used in geometric probability
- [[QA_13_Percentage]] — % increase in area
- [[03_Formula_Sheet]] — All formulas

---

*⬅️ [[QA_09_Ratio_Proportion]] | ➡️ [[QA_11_SI_CI]]*
