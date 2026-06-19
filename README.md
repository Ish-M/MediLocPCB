# Medi-Zero V1: Production-Grade Smart Medication Dispenser Mainboard

Medi-Zero is an automated, smart medication dispensing system designed to help seniors and patients seamlessly manage complex medication schedules. This repository houses the hardware design files for the **Medi-Zero V1 Mainboard** - a custom, production-ready 4-layer Printed Circuit Board Assembly (PCBA) designed in EasyEDA Pro.

Moving away from fragile prototype wiring and modular breakout boards, V1 integrates silicon directly onto the PCB to optimize for cost, reliability, thermal efficiency, and electromagnetic immunity (EMI).

## Visuals & Layout

### 3D PCB Render
<img width="2160" height="1632" alt="3D_PCB1_2026-06-14" src="https://github.com/user-attachments/assets/bbee2852-6371-451a-b311-2db79200c891" />

### 2D Copper Layout & Routing
<img width="700" height="760" alt="3D_PCB1_TopDown_2026-06-14" src="https://github.com/user-attachments/assets/27be7a1e-39f9-4951-ba14-7ec792716ad8" />

## System Architecture

The mainboard relies on a strictly partitioned power and data infrastructure to ensure that high-frequency digital lines, sensitive analog audio paths, and noisy motor switching environments coexist without cross-interference.

```text
12V DC Input (Barrel Jack)
  │
  ├──► [Protection Front-End] (PTC Fuse ── TVS Diode ── P-Ch MOSFET) ──► Raw +12V Motor Rail
  │                                                                       │
  ▼                                                                       ▼
[TPS54302 Synchronous Buck Converter]                               [TMC2209-LA Stepper Driver]
  │                                                                       │
  ├──► +5V High-Current Rail                                              ▼
  │     ├──► MG90S Locking Servo                                     NEMA 17 Stepper Motor
  │     └──► MAX98357A I2S Audio Amplifier ──► 8Ω Speaker
  │
  ▼
[AP2112K-3.3 Ultra-Low Noise LDO]
  │
  └──► +3.3V Pristine Logic Rail
        ├──► ESP32-S3-WROOM-1 Core MCU
        ├──► INMP441 I2S MEMS Microphone
        ├──► SSD1309 OLED Display (SPI Header)
        └──► Hardware-Debounced Microswitches
