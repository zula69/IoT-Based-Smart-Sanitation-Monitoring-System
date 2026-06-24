<div align="center">

# 🚽 IoT-Based Smart Sanitation Monitoring System

### Real-time washroom air quality monitoring powered by ESP32, Firebase & an automated alert system

![Platform](https://img.shields.io/badge/Platform-ESP32-blue?style=for-the-badge&logo=espressif)
![Cloud](https://img.shields.io/badge/Cloud-Firebase-orange?style=for-the-badge&logo=firebase)
![Language](https://img.shields.io/badge/Firmware-Arduino%20C%2FC%2B%2B-teal?style=for-the-badge&logo=arduino)
![Web](https://img.shields.io/badge/Dashboard-HTML%2FCSS%2FPHP-blueviolet?style=for-the-badge&logo=html5)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen?style=for-the-badge)
![Team](https://img.shields.io/badge/Team-Group%204-red?style=for-the-badge)

> **CNT5015 – Computing Project | ICBT Kandy Campus (Cardiff Metropolitan University)**
> HDNTCS Batch 11 — Group 4

</div>

---

## 📌 Table of Contents

- [Overview](#-overview)
- [Problem Statement](#-problem-statement)
- [Objectives](#-objectives)
- [System Architecture](#-system-architecture)
- [Alert Mechanism](#-3-stage-alert-mechanism)
- [Skills Learned](#-skills-learned)
- [Tools & Components](#️-tools--components)
- [Step-by-Step Build](#-step-by-step-build)
- [Real-World Testing & Results](#-real-world-testing--results)
- [Challenges & Mitigations](#-challenges--mitigations)
- [Limitations](#-limitations)
- [Future Improvements](#-future-improvements)
- [Team](#-team)

---

## 🧩 Overview

University campuses handle thousands of students and staff daily, yet washroom hygiene is still managed through **fixed-schedule manual inspections** — a method that lacks real-time awareness and leads to delayed responses.

This project builds an **IoT-powered Smart Sanitation Monitoring System** that continuously monitors air quality, gas levels, temperature, and humidity inside campus washrooms. When conditions degrade, the system **automatically triggers LEDs, buzzers, and exhaust fans** while pushing live alerts to a web dashboard — no manual inspection needed.

```
Sensors → ESP32 → Wi-Fi → Firebase Realtime DB → Web Dashboard
Sensors → ESP32 → Wired → RGB LED Panel (washroom entrance)
```

---

## ❗ Problem Statement

| Issue | Impact |
|---|---|
| Manual-only inspections | Delayed response to unhygienic conditions |
| Fixed cleaning schedules | Resources wasted on unnecessary checks |
| No real-time gas/odor detection | Health risks from methane and ammonia exposure |
| No data collection | Cannot analyze trends or optimize schedules |

---

## 🎯 Objectives

- Monitor air quality, methane, temperature and humidity **continuously** using IoT sensors
- Classify environment into **Good / Moderate / Critical** states automatically
- Trigger **LED, buzzer, fan, and humidifier responses** based on sensor thresholds
- Push live sensor data and alerts to a **Firebase-backed web dashboard**
- Eliminate fixed-schedule inspections through **proactive automated alerting**
- Provide cleaning staff with an **intuitive, no-training-required** interface

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    SENSOR LAYER                         │
│  MQ-135 (NH3, benzene)  MQ-4 ( Methane)  DHT22(Temp/Hum)│
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│               ESP32 MICROCONTROLLER                     │
│  • Reads sensor data every cycle                        │
│  • Applies 3-stage threshold classification             │
│  • Controls LEDs, Buzzer, Fan Relay, Humidifier         │
│  • Transmits data to Firebase via Wi-Fi                 │
└──────┬────────────────────────────────────┬─────────────┘
       │ Wi-Fi                              │ Wired
       ▼                                   ▼
┌─────────────┐                    ┌──────────────────┐
│  Firebase   │                    │  RGB LED Panel   │
│  Realtime   │──────────────►    │  (Entrance)      │
│  Database   │                    │  Green/Yellow/Red│
└─────┬───────┘                    └──────────────────┘
      │
      ▼
┌─────────────────────────────────────────────────────────┐
│                 WEB DASHBOARD (PHP)                     │
│  • Live air quality gauge & sensor charts               │
│  • Per-bathroom status panels                           │
│  • Real-time notification feed                          │
│  • Sensor history (last 20 readings)                    │
└─────────────────────────────────────────────────────────┘
```

---

## 🚦 3-Stage Alert Mechanism

| State | Trigger Condition | LED | Buzzer | Fan | Humidifier | Dashboard |
|:---:|---|:---:|:---:|:---:|:---:|---|
| 🟢 **GOOD** | All readings normal | Green ON | OFF | OFF | OFF | Normal |
| 🟡 **MODERATE** | Slightly elevated gas detected | Yellow ON | OFF | ON (5 min) | OFF | Warning banner |
| 🔴 **CRITICAL** | Dangerous levels >20 seconds | Red ON | ON | ON | ON (post-reset) | CRITICAL ALERT + System Lock |

> ⏱️ **20-second delay filter** prevents false alarms from temporary odors like perfume or cleaning sprays.

---

## 🧠 Skills Learned

- IoT sensor integration and threshold-based automation using **ESP32**
- Real-time cloud data pipeline with **Firebase Realtime Database**
- Embedded firmware development in **Arduino C/C++**
- Web dashboard development using **HTML, CSS, PHP**
- Agile-based iterative **hardware-software co-development**
- System design: **DFDs, UML diagrams, use case diagrams, pseudocode**
- Multi-level testing: unit, integration, system, functional, and field testing
- Sensor calibration, noise filtering, and **false-alarm mitigation**

---

## 🛠️ Tools & Components

### Hardware

| Component | Model | Purpose |
|---|---|---|
| Microcontroller | ESP32 | Core processing + Wi-Fi transmission |
| Air Quality Sensor | MQ-135 | Detects ammonia, CO₂, VOCs |
| Gas Sensor | MQ-4 | Detects methane and similar gases |
| Temp/Humidity Sensor | DHT22 | Environmental condition monitoring |
| LED Indicators | RGB (Green/Yellow/Red) | Visual status display outside washroom |
| Alert System | Active Buzzer | Audible alarm in staff room on critical state |
| Ventilation | Relay + Exhaust Fan | Auto-activated on moderate/critical condition |
| Reset Button | Push Button | Manual recovery after critical event |

### Software

| Tool | Purpose |
|---|---|
| Arduino IDE | Firmware development and ESP32 flashing |
| Firebase Realtime Database | Live sensor data cloud storage and sync |
| HTML / CSS | Dashboard frontend design |
| PHP | Dashboard backend logic and Firebase integration |
| Google Forms | Requirement gathering via stakeholder surveys |

---

## 📋 Step-by-Step Build

### Step 1 — Hardware Wiring

Connect all sensors and actuators to the ESP32:

```
MQ-135  Analog Out  ──►  GPIO 34
MQ-4    Analog Out  ──►  GPIO 35
DHT22   Data        ──►  GPIO 4
Buzzer              ──►  GPIO 26
Fan Relay           ──►  GPIO 27
Red LED             ──►  GPIO 14
Yellow LED          ──►  GPIO 12
Green LED           ──►  GPIO 13
Humidifier Relay    ──►  GPIO 25
Reset Button        ──►  GPIO 0  (with 10kΩ pull-up resistor)
```

> ⚠️ Verify exact GPIO pins against your specific ESP32 board pinout before wiring.

---

### Step 2 — Firebase Setup

1. Go to [console.firebase.google.com](https://console.firebase.google.com) → **Create Project**
2. Enable **Realtime Database** → Start in test mode
3. Set database rules (development):

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

4. Copy your **API Key** and **Database URL** for the firmware.

---

### Step 3 — Flash ESP32 Firmware

Install required libraries via **Sketch → Manage Libraries**:

```
Firebase ESP Client       (by Mobizt)
DHT sensor library        (by Adafruit)
Adafruit Unified Sensor   (by Adafruit)
ArduinoJson               (by Benoit Blanchon)
```

Configure credentials in your sketch:

```cpp
#define WIFI_SSID      "YOUR_WIFI_SSID"
#define WIFI_PASSWORD  "YOUR_WIFI_PASSWORD"
#define API_KEY        "YOUR_FIREBASE_API_KEY"
#define DATABASE_URL   "https://your-project-default-rtdb.firebaseio.com"
```

Flash via: **Tools → Board: ESP32 Dev Module → Upload**

---

### Step 4 — Sensor Threshold Configuration

```cpp
// Moderate alert thresholds
#define MQ135_MODERATE   800
#define MQ4_MODERATE     900

// Critical alert thresholds
#define MQ135_CRITICAL   1100
#define MQ4_CRITICAL     1000

// Delay filter — prevent false alarms
#define ALERT_DELAY_MS   20000   // 20 seconds sustained before escalating

// Post-reset cooldown
#define COOLDOWN_MS      10000   // 10 seconds sensor input ignored after reset
```

---

### Step 5 — Web Dashboard Setup

```bash
git clone https://github.com/zula69/smart-sanitation-monitor
cd dashboard/
cp config.example.php config.php
```

Add Firebase credentials to `config.php`:

```php
<?php
define('FIREBASE_API_KEY',    'YOUR_API_KEY');
define('FIREBASE_DB_URL',     'https://your-project.firebaseio.com');
define('FIREBASE_PROJECT_ID', 'your-project-id');
?>
```

Dashboard features:
- Overall Air Quality Index gauge (color-coded)
- Live MQ-135, MQ-4, DHT22 readings per bathroom unit
- Sensor history charts (last 20 readings)
- Real-time notification feed with timestamps
- Per-bathroom status panels with fan/buzzer state indicators

---

### Step 6 — Reset & Post-Critical Recovery Flow

```
[Critical Alert Triggered]
        │
        ▼
  🔴 Red LED ON
  🔊 Buzzer ON
  💨 Fan ON
  🔒 System LOCKED — manual reset required
        │
  [Cleaning staff presses Reset Button]
        │
        ▼
  🔇 Buzzer OFF
  💨 Fan OFF
  💧 Humidifier ON  ──► Releases mist to neutralize remaining odors
  ⏱️  Cooldown (10s) ──► Sensor input ignored to prevent re-trigger
        │
        ▼
  🟢 Normal Monitoring Resumes
```

---

## 📊 Real-World Testing & Results

The system was deployed and tested in **3 real environments** to validate sensor accuracy and alert behavior.

### Recorded Sensor Data

| Location | MQ-135 | MQ-4 | Temp (°C) | Humidity (%) | Result |
|---|:---:|:---:|:---:|:---:|---|
| Personal Room | 579 | 448 | 30.6 | 82.6 | ✅ Clean air — no alerts |
| Study Room | 597 | 464 | 28.4 | 74.3 | ✅ Clean air — better ventilation |
| Cafeteria (perfume spray test) | 930 | **1090** | 30.8 | 85.7 | 🔴 Gas detected — buzzer triggered |

### Key Findings

- Personal and study rooms showed **stable baseline readings** — system correctly held green state
- Cafeteria perfume test caused MQ-4 to spike to **1090**, crossing the critical threshold and **activating the buzzer alert as expected**
- DHT22 captured clear temperature and humidity differences across locations, confirming environmental sensitivity
- System correctly **locked on critical condition** and **recovered cleanly** after reset with cooldown

> ✅ Multi-environment testing confirms accurate detection, correct alert escalation, and clean post-reset recovery.

---

## ⚠️ Challenges & Mitigations

| Challenge | How We Fixed It |
|---|---|
| False alarms from temporary odors (perfume, cleaning spray) | Added **20-second delay filter** — state only escalates if condition persists |
| Re-triggering immediately after reset due to lingering gas | Implemented **10-second cooldown mode** that ignores sensor input post-reset |
| Unstable sensor readings after critical events | Post-reset **humidifier** disperses mist, stabilizing air and returning sensors to baseline |
| MQ sensors reacting to non-target gases | Cross-validated both MQ-135 and MQ-4 — **both** must exceed threshold before critical state |
| ESP32 losing Wi-Fi connection on power cycle | Added **auto-reconnect loop** with retry logic in firmware |
| Dashboard data lag from polling | Switched to **Firebase Realtime Listener** for instant push updates |
| Inaccurate readings on startup | Added **startup warm-up delay** before first sensor read cycle |

---

## 🚧 Limitations

- **Wired Setup** — Sensor-to-ESP32 wiring limits physical placement flexibility and scalability
- **MQ Cross-Sensitivity** — MQ-135 and MQ-4 react to a wide range of gases including harmless ones
- **Single Unit Prototype** — Covers one bathroom; full campus deployment needs additional ESP32 nodes
- **Manual Calibration** — Sensors require recalibration in new environments for consistent accuracy
- **No Mobile Alerts** — Notifications are dashboard-only; no push alerts to staff phones

---

## 🚀 Future Improvements

- 📱 **Mobile App** — Dedicated Android/iOS app with real-time push notifications for cleaning staff
- 📡 **Fully Wireless** — Replace wired LED panel with Wi-Fi or BLE-connected smart display
- 🧴 **Handwash Level Monitoring** — Ultrasonic sensor to track soap/sanitizer levels
- 📊 **Analytics & Scheduling** — Historical data trends to automate and optimize cleaning rosters
- 🏢 **Multi-Floor / Multi-Bathroom** — Scalable dashboard view for full campus coverage
- 🤖 **ML-Based Anomaly Detection** — Train model on baseline data to reduce false positive rates

---

**Module:** CNT5015 – Computing Project
**Assessor:** Mr. Gajindu Bandara
**Submission:** May 2026

---

<div align="center">

*Built with real hardware. Tested in real environments. Deployed for real impact.*

</div>
