# 🔵 SET A | Q11a — Arduino Programming Structure

> **🧠 Feynman Tip:** Imagine Arduino as a **robot chef** 🍳 — it has a **setup kitchen** (done once) and a **cooking loop** (done forever). Every instruction you give is a recipe step it follows in order.

> **📐 1-4-7 Rule:** 1 Big Idea → 4 Sub-topics → 7 Key Points per topic

---

## 📌 1. Introduction / Definition

**Arduino** is an **open-source microcontroller platform** used to build interactive electronic projects. It combines hardware (a physical board) and software (Arduino IDE) to read inputs (sensors, buttons) and control outputs (LEDs, motors).

> 🔑 **Key Idea:** Arduino programming follows a **fixed structure** — every sketch (program) has exactly **two mandatory functions**: `setup()` and `loop()`.

- Arduino language is based on **C/C++**
- Programs are called **"Sketches"**
- Files are saved with `.ino` extension
- Runs on boards like Arduino **Uno, Mega, Nano**

---

## 📌 2. Basic Arduino Program Architecture

```
┌─────────────────────────────────┐
│        Arduino Sketch           │
├─────────────────────────────────┤
│  1. Libraries & Global Variables│
│     #include, int, const        │
├─────────────────────────────────┤
│  2. setup() — Runs ONCE         │
│     pinMode, Serial.begin       │
├─────────────────────────────────┤
│  3. loop() — Runs FOREVER       │
│     Read → Process → Output     │
└─────────────────────────────────┘
```

> 🧠 **Feynman Analogy:** `setup()` = Opening a restaurant (done once). `loop()` = Serving customers (done again and again forever).

---

## 📌 3. Sub-topic A — `setup()` Function

- Executes **only once** when the board powers on or resets
- Used to **initialize** pins, serial communication, libraries

```cpp
void setup() {
  pinMode(13, OUTPUT);     // Set pin 13 as output
  Serial.begin(9600);      // Start serial at 9600 baud
}
```

| Task | Function Used |
|------|--------------|
| Set pin mode | `pinMode(pin, MODE)` |
| Start serial | `Serial.begin(baud)` |
| Initialize LCD | `lcd.begin()` |

---

## 📌 4. Sub-topic B — `loop()` Function

- Executes **continuously and forever** after `setup()`
- Contains the **main logic**: read sensor → process → act

```cpp
void loop() {
  int val = digitalRead(2);   // Read input from pin 2
  if (val == HIGH) {
    digitalWrite(13, HIGH);   // Turn ON LED
  } else {
    digitalWrite(13, LOW);    // Turn OFF LED
  }
  delay(500);                 // Wait 500 milliseconds
}
```

> 📐 **1-4-7 Tip:** Loop = **Read → Decide → Act → Wait** — 4 steps every loop follows.

---

## 📌 5. Sub-topic C — Key Programming Elements

### 5.1 Variables & Data Types

| Type | Use | Example |
|------|-----|---------|
| `int` | Whole numbers | `int led = 13;` |
| `float` | Decimal numbers | `float temp = 36.5;` |
| `boolean` | True/False | `boolean state = true;` |
| `String` | Text | `String name = "IoT";` |

### 5.2 Control Structures

```cpp
// If-Else
if (temp > 30) { fan = ON; } else { fan = OFF; }

// For Loop
for (int i = 0; i < 5; i++) { blink(); }

// While Loop
while (sensorReading > 100) { alert(); }
```

### 5.3 Digital & Analog Functions

| Function | Purpose |
|----------|---------|
| `digitalWrite(pin, HIGH/LOW)` | Write digital output |
| `digitalRead(pin)` | Read digital input |
| `analogWrite(pin, 0-255)` | PWM output |
| `analogRead(pin)` | Read analog (0–1023) |
| `delay(ms)` | Pause in milliseconds |

---

## 📌 6. Diagram — Arduino Program Flow

```
       [ Power ON / Reset ]
              │
              ▼
       ┌─────────────┐
       │  setup()    │  ← Runs ONCE
       │  Init Pins  │
       │  Init Serial│
       └──────┬──────┘
              │
              ▼
       ┌─────────────┐
    ┌─►│   loop()    │  ← Runs FOREVER
    │  │  Read Input │
    │  │  Process    │
    │  │  Write Out  │
    │  └──────┬──────┘
    │         │
    └─────────┘
```

### 📱 Application Example — LED Blink

```cpp
void setup() {
  pinMode(13, OUTPUT);   // LED pin
}

void loop() {
  digitalWrite(13, HIGH);  // LED ON
  delay(1000);             // Wait 1 sec
  digitalWrite(13, LOW);   // LED OFF
  delay(1000);             // Wait 1 sec
}
```

---

## 📌 7. Advantages & Disadvantages

### ✅ Advantages

| # | Advantage |
|---|-----------|
| 1 | Simple structure — easy for beginners |
| 2 | Open-source, free IDE |
| 3 | Large community & library support |
| 4 | Works with hundreds of sensors & modules |
| 5 | Cross-platform (Windows, Mac, Linux) |

### ❌ Disadvantages

| # | Disadvantage |
|---|--------------|
| 1 | Limited processing power (8-bit, 16 MHz) |
| 2 | Small memory (32KB flash, 2KB RAM on Uno) |
| 3 | No OS — cannot multitask |
| 4 | Single-threaded — `loop()` blocks everything |
| 5 | Not suitable for complex applications |

---

## 📌 8. Conclusion

> 🎯 **One-line Summary:** Arduino programming is built on two pillars — **`setup()` for initialization** and **`loop()` for continuous operation** — making it an ideal, beginner-friendly platform for IoT and embedded system development.

- The structured nature (global → setup → loop) makes code **predictable and easy to debug**
- Arduino bridges the gap between **hardware and software** in IoT projects
- It is the **backbone of rapid prototyping** in smart systems

---

*📝 Tags: #Arduino #IoT #Programming #Microcontroller #SetA #Part-B*
