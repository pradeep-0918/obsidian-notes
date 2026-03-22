# 🧭 Direction Sense
> Part of [[02_Reasoning_Index]] | 🔙 [[00_Master_Index]]

**Difficulty:** ⭐⭐ Easy | **Exam Weight:** Medium | **Time per Q:** 60–90 sec

---

## 🧠 Concept

Direction problems trace a path from a starting point through turns and distances, then ask:
- What direction is the person facing?
- What is the straight-line distance from start to end?
- Where are two people relative to each other?

**Golden Rule: ALWAYS DRAW. Never solve in your head.**

---

## 📌 The Compass Rose

```
           North (N)
               ↑
               |
West (W) ←────┼────→ East (E)
               |
               ↓
           South (S)

NW ↖    ↗ NE
   ↙    ↘
SW ↙    ↘ SE
```

### Turning Rules
- **Turn Right from North** → Face East
- **Turn Left from North** → Face West
- **Turn Right from East** → Face South
- **Turn Left from East** → Face North

| Facing | Turn Right | Turn Left | About Turn (180°) |
|---|---|---|---|
| North | East | West | South |
| East | South | North | West |
| South | West | East | North |
| West | North | South | East |

---

## 🛠️ Problem Solving Approach

### Step 1: Set up coordinates
- Start at origin (0, 0)
- North = +Y, South = −Y
- East = +X, West = −X

### Step 2: Track each move
Convert each step into (x, y) displacement:
- "Walk 5 km North" → (0, +5)
- "Walk 3 km East" → (+3, 0)
- "Walk 4 km South" → (0, −4)

### Step 3: Find final position
Add all displacements: Final = (Σx, Σy)

### Step 4: Find distance from origin
$$\text{Distance} = \sqrt{(\Sigma x)^2 + (\Sigma y)^2}$$

### Step 5: Find direction facing
Based on the LAST turn made in the problem.

---

## 🔥 Types of Questions

### Type 1: What direction is he facing?
> "Man walks North, turns right, walks, turns left. Facing?"
- North → right turn → East; East → left turn → North
- **Facing North**

### Type 2: Distance from start
> "Walk 3N, turn right 4E, turn right 3S, turn right 2W. Distance from start?"
- Net East-West = 4−2 = 2E; Net North-South = 3−3 = 0
- Distance = √(4+0) = **2 km East**

### Type 3: Shadow problems
- Sun rises in East, sets in West
- **Morning shadows** → point West (sun is East)
- **Evening shadows** → point East (sun is West)

### Type 4: Facing another person
> "A faces B. A's right = B's left."
> "If A faces North and looks at B, B is South of A, and B faces South (toward A)."

---

## 💡 Shortcuts & Tricks

### The Shortcut Grid Method
For any path, track only:
1. Total North/South displacement (net N−S)
2. Total East/West displacement (net E−W)
3. Final distance = √(N−S² + E−W²)

### Common Distance Pattern: The 3-4-5 Triangle
- If net displacement = 3 in one axis, 4 in another → distance = **5**
- If net = 5 and 12 → distance = **13**
- If net = 8 and 6 → distance = **10**

### 45° Turns
- Turning 45° clockwise: N→NE→E→SE→S→SW→W→NW→N
- Turning 135° clockwise: N→SE (skip 3 positions)

### The Clockwise Order (memorize!)
N → NE → E → SE → S → SW → W → NW → N (clockwise)
Turning 90° clockwise from N = E
Turning 90° anti-clockwise from N = W

---

## ⚠️ Common Mistakes

1. **Confusing left/right with East/West**
   - Left and right depend on which direction you're FACING
   - "Turn right while facing North" = turn East

2. **Not drawing the diagram**
   - Missing a turn or direction in mental calculation

3. **Using km/h when asked for direction**
   - Sometimes distance and speed given — check what's asked

4. **Shadow direction confusion**
   - Sun in East (morning) → your shadow goes West (behind you)
   - Sun in West (evening) → your shadow goes East

5. **Displacement vs Distance**
   - Displacement = straight-line start to end
   - Distance = total path traveled
   - Usually asked: displacement (straight line)

---

## 🧩 Practice Questions

### Medium
1. Man walks 5N, turns right 3, turns left 4. Direction?
   > 5N → right = East → 3E → left = North → 4N → **Facing North**

2. Car: 10E, turn South 8, turn West 6. Distance from start?
   > Net E-W = 10−6 = 4E; Net N-S = 0−8 = 8S; Distance = √(16+64) = √80 = **4√5 ≈ 8.94 km**

3. Walk 6W, turn right 4, turn left 5. Direction?
   > 6W → right from West = North → 4N → left from North = West → 5W → **Facing West**

4. Man: 7S, turn left 4, turn right 3. Final direction?
   > 7S → left from South = East → 4E → right from East = South → 3S → **Facing South**

5. Car: 12N, turn left 5, turn right 8. Distance from start?
   > Net N-S = 12+8=20N; Net E-W = 5W; Distance = √(400+25) = √425 = **5√17 ≈ 20.6 km**

6. Walk 8E, turn right 6, turn left 4. Direction?
   > 8E → right from East = South → 6S → left from South = East → 4E → **Facing East**

7. Walk 10W, turn right 7, turn left 3. Direction?
   > 10W → right from West = North → 7N → left from North = West → 3W → **Facing West**

8. Walk 15S, turn left 10, turn right 5. Distance from start?
   > 15S, left=East, 10E, right=South, 5S → Net S=20, Net E=10; Distance = √(400+100) = √500 = **10√5 ≈ 22.4 km**

### Difficult
1. Walk 10E, turn 90° clockwise, walk 8, turn 135° anticlockwise, walk 6. Direction?
   > Start facing East; 90° CW from East = South; walk 8S; 135° ACW from South...
   > ACW from South: S→SE→E→NE = 135° → now facing NE (Northeast)
   > Walk 6 in NE direction → **Facing NE (Northeast)**

2. Walk 15N, turn 45° right, walk 10, turn 90° left, walk 5. Distance from start?
   > 15N; 45° right from N = NE; walk 10 NE = (+7.07, +7.07); 90° left from NE = NW; walk 5 NW = (−3.54, +3.54)
   > Total: x = 0+7.07−3.54 = 3.53; y = 15+7.07+3.54 = 25.61
   > Distance = √(3.53²+25.61²) ≈ √(12.5+656) ≈ **√668.5 ≈ 25.86 km**

3. Walk 18N, turn 90° left, walk 10, turn 135° right, walk 7. Distance?
   > 18N; left from N = West; 10W; 135° right from West... 
   > CW from West: W→NW→N→NE = 135° → facing NE; walk 7 NE=(+4.95,+4.95)
   > Net: x=−10+4.95=−5.05W; y=18+4.95=22.95N
   > Distance = √(25.5+526.7) = √552.2 ≈ **23.5 km**

---

## 🔗 Related Topics

- [[LR_05_Seating_Arrangement]] — Spatial positioning
- [[LR_06_Puzzles]] — Combined with direction in complex puzzles
- [[02_Reasoning_Index]] — Reasoning overview

---

*⬅️ [[LR_02_Blood_Relations]] | ➡️ [[LR_04_Syllogism]]*
