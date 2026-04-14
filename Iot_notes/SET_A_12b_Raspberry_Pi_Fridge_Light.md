# 🔵 SET A | Q12b — Automatic Refrigerator Light System Using Raspberry Pi

> **🧠 Feynman Tip:** This project is exactly like your **real fridge** — when you open the door, the light turns ON. When you close it, the light turns OFF. We're just building that using a Raspberry Pi + sensor + LED!

> **📐 1-4-7 Rule:** 1 System (Auto Fridge Light) → 4 Components → 7 Code Steps

---

## 📌 1. Introduction / Definition

An **Automatic Refrigerator Light System** is a simple IoT project that uses:
- A **door sensor (magnetic switch or LDR)** to detect if the fridge door is open or closed
- A **Raspberry Pi** to process the input and control the output
- An **LED or relay** to turn the light ON or OFF automatically

> 🔑 **Core Principle:** `Door OPEN → Light ON` | `Door CLOSED → Light OFF`

This eliminates the need for manual switching and is a **practical smart home automation** example.

---

## 📌 2. Components Required

| Component | Purpose |
|-----------|---------|
| Raspberry Pi (any model) | Central controller |
| PIR Sensor or Magnetic Reed Switch | Detect door open/close |
| LDR (Light Dependent Resistor) | Alternative: light detection inside fridge |
| LED or Relay Module | Represent/control the fridge light |
| Resistors (330Ω) | Current limiting for LED |
| Jumper Wires & Breadboard | Circuit connections |
| Python (RPi.GPIO library) | Programming language |

---

## 📌 3. Sub-topic A — Raspberry Pi GPIO Overview

> 🧠 **Feynman:** GPIO pins = **Raspberry Pi's hands** — some hands READ (input), some hands WRITE (output).

```
Raspberry Pi GPIO Layout (Key Pins)
┌──────────────────────────────┐
│  3.3V │ 5V   │ GND  │ GPIO  │
│  Pin1 │ Pin2 │ Pin6 │ Pins  │
│                              │
│  GPIO4  - Input (Sensor)    │
│  GPIO17 - Output (LED)      │
└──────────────────────────────┘
```

### Pin Modes
| Mode | Description |
|------|-------------|
| `GPIO.IN` | Read from sensor |
| `GPIO.OUT` | Write to LED/Relay |
| `GPIO.BCM` | Use Broadcom pin numbering |
| `GPIO.BOARD` | Use physical pin numbers |

---

## 📌 4. Sub-topic B — Circuit Diagram

```
CIRCUIT CONNECTIONS:
═══════════════════

Magnetic Reed Switch (Door Sensor):
  One end ──► GPIO 4 (Pin 7)
  Other end ──► GND (Pin 6)
  [Internal Pull-up enabled in code]

LED (Fridge Light):
  Anode (+) ──► 330Ω Resistor ──► GPIO 17 (Pin 11)
  Cathode (-) ──► GND (Pin 6)

Raspberry Pi
     ┌──────────────────────┐
     │  GPIO4 ←── [DOOR     │
     │             SENSOR]  │
     │                      │
     │  GPIO17 ──► [330Ω]──►│LED
     │                      │
     │  GND ────────────────┤(both GND)
     └──────────────────────┘
```

**Sensor Logic:**
- Door **OPEN** → Magnetic contact **BROKEN** → GPIO reads **HIGH**
- Door **CLOSED** → Magnetic contact **CONNECTED** → GPIO reads **LOW**

---

## 📌 5. Sub-topic C — Python Code

```python
# Automatic Refrigerator Light System
# Raspberry Pi + Magnetic Door Sensor + LED

import RPi.GPIO as GPIO    # Import GPIO library
import time                # Import time for delays

# ── PIN SETUP ──────────────────────────────
DOOR_SENSOR = 4    # GPIO 4 = door sensor (input)
LED_PIN = 17       # GPIO 17 = LED / fridge light (output)

# ── INITIALIZATION ──────────────────────────
GPIO.setmode(GPIO.BCM)           # Use Broadcom numbering
GPIO.setup(DOOR_SENSOR, GPIO.IN, pull_up_down=GPIO.PUD_UP)
                                 # Enable internal pull-up resistor
GPIO.setup(LED_PIN, GPIO.OUT)    # Set LED pin as output
GPIO.output(LED_PIN, GPIO.LOW)   # Start with LED OFF

print("🔵 Fridge Light System Running...")

# ── MAIN LOOP ────────────────────────────────
try:
    while True:
        door_status = GPIO.input(DOOR_SENSOR)  # Read door sensor

        if door_status == GPIO.HIGH:           # Door is OPEN
            GPIO.output(LED_PIN, GPIO.HIGH)    # Turn LED ON
            print("🚪 Door OPEN  → 💡 Light ON")

        else:                                  # Door is CLOSED
            GPIO.output(LED_PIN, GPIO.LOW)     # Turn LED OFF
            print("🚪 Door CLOSED → 💡 Light OFF")

        time.sleep(0.1)                        # Check every 100ms

# ── CLEAN EXIT ────────────────────────────────
except KeyboardInterrupt:
    print("\n⛔ Stopping system...")
    GPIO.cleanup()             # Reset all GPIO pins
    print("✅ GPIO Cleaned Up. Goodbye!")
```

---

## 📌 6. Sub-topic D — Code Explanation (Line by Line)

```
Step 1: import RPi.GPIO → Load the GPIO control library
Step 2: import time     → Load time functions (delay)
Step 3: setmode(BCM)    → Use Broadcom GPIO numbering system
Step 4: setup(SENSOR, IN, PUD_UP) → Door pin = input with pull-up
Step 5: setup(LED, OUT) → LED pin = output
Step 6: while True:     → Run forever (IoT always-on)
Step 7: GPIO.input()    → Read door sensor state
  → HIGH = Open  → LED ON
  → LOW  = Closed → LED OFF
Step 8: GPIO.cleanup()  → Safety: reset pins on exit
```

---

## 📌 7. System Flow Diagram

```
         START
           │
    [Initialize GPIO]
    [Set Sensor = IN]
    [Set LED = OUT]
           │
           ▼
    ┌─────────────┐
 ┌─►│  Read Door  │◄─────────────┐
 │  │  Sensor     │              │
 │  └──────┬──────┘              │
 │         │                     │
 │    Door Open?                 │
 │    ┌────┴────┐                │
 │  YES        NO                │
 │    │         │                │
 │  [LED ON]  [LED OFF]          │
 │    │         │                │
 │    └────┬────┘                │
 │         │                     │
 │    [Delay 100ms]              │
 └─────────┴───────────────────-─┘
```

---

## 📌 8. Enhancement Ideas

| Enhancement | How |
|-------------|-----|
| Temperature alert | Add DHT11 sensor, send SMS if temp too high |
| Energy monitor | Count hours light is ON, log to cloud |
| Remote alert | If door open > 2 mins → Send WhatsApp/email alert |
| Camera | Add Pi Camera to detect if fridge is full |

---

## 📌 9. Advantages & Disadvantages

### ✅ Advantages

| # | Advantage |
|---|-----------|
| 1 | Fully automated — no manual switching |
| 2 | Energy saving — light only ON when needed |
| 3 | Easy to build with basic components |
| 4 | Expandable to other home automation tasks |
| 5 | Python code is simple and readable |

### ❌ Disadvantages

| # | Disadvantage |
|---|--------------|
| 1 | Raspberry Pi consumes more power than Arduino |
| 2 | Sensor may malfunction if exposed to fridge moisture |
| 3 | Requires stable power supply |
| 4 | Overkill for just a light — Arduino is simpler for this |

---

## 📌 10. Conclusion

> 🎯 **One-line Summary:** The automatic refrigerator light system demonstrates how **Raspberry Pi's GPIO, a door sensor, and Python logic** can automate a simple real-world task — turning a light ON/OFF based on door state.

- This is a **foundational IoT project** that can be scaled to complex smart home systems
- Raspberry Pi's **Linux-based OS and Python support** make it powerful for automation
- GPIO management with `RPi.GPIO` is the **core skill** for all Raspberry Pi IoT projects

---

*📝 Tags: #RaspberryPi #GPIO #Python #AutomationProject #IoT #FridgeLight #SetA #Part-B*
