# 🟢 SET B | Q12b — Raspberry Pi Architecture + IoT Applications

> **🧠 Feynman Tip:** Raspberry Pi is like a **tiny full laptop** 💻 squeezed onto a credit-card-sized board. Unlike Arduino (which only runs one simple program), Raspberry Pi runs a full **Linux operating system** — so it's like the difference between a calculator and a computer.

> **📐 1-4-7 Rule:** 1 Platform (RPi) → 4 Architecture Layers → 7 IoT Applications

---

## 📌 1. Introduction / Definition

**Raspberry Pi** is a **credit-card-sized single-board computer (SBC)** developed by the Raspberry Pi Foundation (UK) that runs a full **Linux-based operating system** and is widely used in IoT, education, robotics, and prototyping.

> 🔑 **Key Difference from Arduino:**

| Feature | Arduino | Raspberry Pi |
|---------|---------|-------------|
| Type | Microcontroller | Microcomputer |
| OS | None (bare metal) | Linux (Raspberry Pi OS) |
| Language | C/C++ | Python, Java, C++, etc. |
| Processing | 16 MHz | 1.5 GHz (Pi 4) |
| RAM | 2 KB | 1–8 GB |
| Storage | 32 KB Flash | MicroSD card |
| Cost | ~$5–25 | ~$35–80 |
| Best For | Simple sensors | Complex IoT, ML, Video |

---

## 📌 2. Raspberry Pi Hardware Architecture — Diagram

```
╔══════════════════════════════════════════════════════╗
║          RASPBERRY PI 4 ARCHITECTURE                ║
╠══════════════════════════════════════════════════════╣
║                                                      ║
║  ┌─────────────────────────────────────────────┐   ║
║  │        SoC (System on Chip)                 │   ║
║  │   Broadcom BCM2711                          │   ║
║  │   ┌─────────┐  ┌──────────┐  ┌──────────┐ │   ║
║  │   │ARM CPU  │  │  GPU     │  │ DSP/VPU  │ │   ║
║  │   │4-core   │  │VideoCore │  │(Video    │ │   ║
║  │   │1.5 GHz  │  │  IV      │  │ decode)  │ │   ║
║  │   └─────────┘  └──────────┘  └──────────┘ │   ║
║  └─────────────────────────────────────────────┘   ║
║          │               │                          ║
║  ┌───────▼──────┐  ┌─────▼──────┐                  ║
║  │  RAM (LPDDR) │  │  Storage   │                  ║
║  │  1/2/4/8 GB  │  │  MicroSD   │                  ║
║  └──────────────┘  └────────────┘                  ║
║                                                      ║
║  I/O INTERFACES:                                    ║
║  [40-pin GPIO][USB A×4][HDMI×2][Ethernet][CSI][DSI]║
║  [Wi-Fi 802.11ac][Bluetooth 5.0][USB-C Power]      ║
╚══════════════════════════════════════════════════════╝
```

---

## 📌 3. Sub-topic A — Core Hardware Components

> 🧠 **Feynman:** Each Raspberry Pi component = a department in a company office.

### 3.1 System on Chip (SoC)

| Pi Model | SoC | CPU | Speed |
|----------|-----|-----|-------|
| Pi Zero 2W | BCM2710A1 | ARM Cortex-A53, 4-core | 1 GHz |
| Pi 3B+ | BCM2837B0 | ARM Cortex-A53, 4-core | 1.4 GHz |
| Pi 4B | BCM2711 | ARM Cortex-A72, 4-core | 1.5–1.8 GHz |
| Pi 5 | BCM2712 | ARM Cortex-A76, 4-core | 2.4 GHz |

### 3.2 Memory (RAM)

- **LPDDR4** RAM: Options of 1GB, 2GB, 4GB, 8GB (Pi 4)
- RAM is **on the SoC** (integrated)
- More RAM → can run more applications simultaneously

### 3.3 Storage

- No built-in storage — uses **MicroSD card**
- Typical: **8–32 GB MicroSD** with Raspberry Pi OS
- Pi 4 also supports **USB 3.0 SSD boot** for faster storage

### 3.4 GPU (VideoCore)

- Handles **video playback, OpenGL, display output**
- Shared memory with CPU
- Supports **4K video decoding** (Pi 4)

---

## 📌 4. Sub-topic B — GPIO (General Purpose Input/Output) Pins

> 🧠 **Feynman:** GPIO = **Raspberry Pi's hands** 🤲 — how it reaches out and touches the physical world.

### GPIO Pin Layout (40-pin header)

```
Physical Pin Layout (40 pins):
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PIN 1  = 3.3V   │  PIN 2  = 5V
PIN 3  = GPIO2  │  PIN 4  = 5V
PIN 5  = GPIO3  │  PIN 6  = GND
...
PIN 11 = GPIO17 │  PIN 12 = GPIO18 (PWM)
PIN 13 = GPIO27 │  PIN 14 = GND
...
PIN 40 = GPIO21
```

### GPIO Capabilities

| Feature | Description |
|---------|-------------|
| Digital I/O | Read/Write HIGH (3.3V) or LOW (0V) |
| PWM | Hardware PWM on GPIO12, 13, 18, 19 |
| I2C | SDA (GPIO2), SCL (GPIO3) |
| SPI | MOSI(10), MISO(9), SCLK(11), CE0(8) |
| UART | TXD(GPIO14), RXD(GPIO15) |
| 3.3V max | Do NOT connect 5V directly! |

---

## 📌 5. Sub-topic C — Software Architecture

> 🧠 **Feynman:** Raspberry Pi OS is like **Windows for the Pi** — but open-source, lightweight, and built for hardware control.

### Software Stack

```
┌──────────────────────────────────────┐
│        USER APPLICATION              │  ← Python, Java, Node.js
├──────────────────────────────────────┤
│        LIBRARIES                     │  ← RPi.GPIO, OpenCV, Flask
├──────────────────────────────────────┤
│     RASPBERRY PI OS (Linux)          │  ← Debian-based OS
├──────────────────────────────────────┤
│        LINUX KERNEL                  │  ← Hardware abstraction
├──────────────────────────────────────┤
│        HARDWARE (SoC + GPIO)         │  ← Physical board
└──────────────────────────────────────┘
```

### Key Software Tools for IoT

| Tool | Purpose |
|------|---------|
| **Python + RPi.GPIO** | GPIO control |
| **Node-RED** | Visual IoT flow programming |
| **MQTT (Mosquitto)** | IoT messaging broker |
| **Flask/Django** | Web server for IoT dashboard |
| **OpenCV** | Computer vision (camera) |
| **TensorFlow Lite** | Edge AI/ML inference |

---

## 📌 6. Sub-topic D — IoT Applications of Raspberry Pi

> 🧠 **Feynman:** Raspberry Pi is the **Swiss Army Knife** 🔧 of IoT — powerful enough for almost any connected project.

### Application 1: Smart Home Server

```
[Raspberry Pi Hub]
      │
 Node-RED + MQTT
      │
┌─────┼──────────┐
[Smart  [Temp    [Motion
 Lights] Monitor]  Alert]
```

### Application 2: Security Camera System

```
[Pi Camera Module] → [Raspberry Pi] → [Motion Detection]
                                            │
                               [Record Video to SD/NAS]
                               [Send alert email/SMS]
```

### Application 3: Remote Weather Station

```
[DHT22 Temp/Humidity] ──┐
[BMP280 Pressure]  ──────►[Raspberry Pi]──►[Cloud (ThingSpeak)]
[Rain Sensor]      ──┘              │
                                    ▼
                              [Web Dashboard]
```

### All IoT Application Areas

| Application | Raspberry Pi Role |
|-------------|-----------------|
| **Home Automation** | MQTT hub, Node-RED controller |
| **Security System** | Face recognition, CCTV recording |
| **Weather Station** | Multi-sensor data logging |
| **Smart Agriculture** | Soil monitoring, automated irrigation |
| **Robotics** | Motor control + camera + AI |
| **Healthcare** | Patient vital monitoring system |
| **Edge AI** | Running ML models locally (TensorFlow Lite) |

---

## 📌 7. Connectivity Features

| Interface | Specification (Pi 4) |
|-----------|---------------------|
| **Wi-Fi** | 802.11ac (2.4 + 5 GHz dual-band) |
| **Bluetooth** | 5.0 + BLE |
| **Ethernet** | Gigabit (1 Gbps) |
| **USB** | 2× USB 3.0, 2× USB 2.0 |
| **HDMI** | 2× micro-HDMI (4K@60fps) |
| **Camera** | CSI port (15-pin ribbon) |
| **Display** | DSI port (touchscreen) |

---

## 📌 8. Advantages & Disadvantages

### ✅ Advantages

| # | Advantage |
|---|-----------|
| 1 | Full Linux OS — run any Linux software |
| 2 | Multi-language support (Python, Java, C++, Node.js) |
| 3 | Built-in Wi-Fi, Bluetooth, Ethernet |
| 4 | 40-pin GPIO for hardware interfacing |
| 5 | Large community and extensive libraries |
| 6 | Can run ML models at the edge |
| 7 | HDMI output — can drive a monitor/display |

### ❌ Disadvantages

| # | Disadvantage |
|---|--------------|
| 1 | Higher power consumption than Arduino |
| 2 | Needs SD card — slower and can corrupt |
| 3 | Requires boot time (~30 seconds) |
| 4 | GPIO is 3.3V only — careful with 5V sensors |
| 5 | More expensive ($35–80) vs. Arduino (~$5) |

---

## 📌 9. Conclusion

> 🎯 **One-line Summary:** Raspberry Pi is a **credit-card Linux computer** with a **powerful SoC, 40-pin GPIO, built-in Wi-Fi/Bluetooth**, and support for Python and ML — making it the **most versatile IoT platform** for complex, connected, and intelligent applications.

- Where Arduino handles **simple automation**, Raspberry Pi handles **complex intelligence**
- Together, **Arduino + Raspberry Pi** = complete IoT solution (sensors + intelligence)
- Pi 5 (2023) brings even more power — cementing its role as the **edge computing champion**

---

*📝 Tags: #RaspberryPi #Architecture #GPIO #IoT #SBC #SetB #Part-B*
