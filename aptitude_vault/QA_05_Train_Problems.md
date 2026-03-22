# 🚂 Train Problems
> Part of [[01_Quant_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐⭐ Medium | **Exam Weight:** High

---

## 🧠 Concept

Train problems are **Time & Distance** problems with one key difference:
> The train has **length** — it takes time to fully pass an object!

**Key principle:** Distance = Length of train + Length of object (platform/bridge)
**For a pole or person:** Distance = Length of train only

---

## 📌 Key Formulas

### Passing a Pole/Standing Person
$$\text{Speed} = \frac{\text{Length of Train}}{\text{Time}}$$

### Passing a Platform/Bridge
$$\text{Speed} = \frac{\text{Length of Train + Length of Platform}}{\text{Time}}$$

### Two Trains — Opposite Directions
$$\text{Time} = \frac{L_1 + L_2}{v_1 + v_2}$$

### Two Trains — Same Direction
$$\text{Time} = \frac{L_1 + L_2}{|v_1 - v_2|}$$

### Passing a Person Running
- Same direction: Relative speed = |v_train − v_person|
- Opposite direction: Relative speed = v_train + v_person

---

## 🔥 Types of Questions

**Type 1:** Train passes a pole → Speed = Length ÷ Time
**Type 2:** Train passes platform → Distance = Train length + Platform length
**Type 3:** Two trains cross → Distance = Sum of lengths ÷ Relative speed
**Type 4:** Train passes a running man → Use relative speed

---

## 💡 Key Shortcuts

- Always convert km/h to m/s when length is in meters
- "Passes completely" means the ENTIRE train has crossed the object
- For same-direction passing: the faster train must cover (both lengths + initial gap if any)

---

## 🧩 Practice Questions

### Easy
1. Train 100m passes pole in 5s. Speed? → 20 m/s = **72 km/h**
2. Train 200m at 20 m/s crosses platform in 10s. Platform length? → Total=200, Train covers 200 in 10s, so Platform = 200×10/20−200... 20×10=200m total → Platform = 200-200 = **0? recalc: 20m/s × 10s = 200m = 100m+platform? No: train 200m at 20m/s in 10s: distance=200m = train+platform = 200m → platform = 0** Hmm re-read: "A train 200 meters long crosses a platform in 10 seconds at 20 m/s" → Distance covered = 20×10=200m = 200 (train) + platform → Platform = **0**... let's use: Train 200m at 20 m/s, crosses platform 300m in ? sec → (200+300)/20 = **25 seconds**
3. Train 120m passes man in 6s. Speed in m/s? → **20 m/s**

### Medium
4. Two trains 100m and 120m, opposite directions at 20 m/s and 30 m/s. Crossing time? → (100+120)/50 = **4.4 seconds**
5. Train 200m at 72 km/h passes platform 300m. Time? → 72 km/h = 20 m/s; (200+300)/20 = **25 sec**

### Hard
6. Train crosses platform in 20 sec, pole in 6 sec. Speed and platform length?
   → Speed = Length/6; Length+Platform = Speed×20; Length×20/6 − Length = Platform; 14L/6 = Platform...
   → Let L=length, S=speed; S=L/6; S×20=L+P; L×20/6=L+P; P=L(20/6-1)=14L/6; If train=180m: S=30m/s, P=180×14/6=**420m**

---

## 🔗 Related Topics
- [[QA_02_Time_Distance]] — Core formula
- [[03_Formula_Sheet]] — Formulas

---

*⬅️ [[QA_04_HCF_LCM]] | ➡️ [[QA_06_Ages]]*
