# LED Cube Project — Architecture Decisions

**Date:** 2026-03-16  
**Status:** Locked baseline for design phase

## 1. Purpose

This document freezes the main technical decisions for the first full version of the 8×8×8 LED cube portfolio project. All following work — requirements, calculations, schematic, PCB, firmware, BOM, verification, and README — must follow this baseline unless this document is formally revised.

## 2. Locked decisions

### 2.1 Microcontroller
The main controller will be an **ESP32 module** mounted on the custom PCB.

**Decision:** Use ESP32 as the main MCU.  
**Reason:** It provides enough performance for multiplexing, animation handling, and wireless control, while also including BLE without extra hardware.

---

### 2.2 User control method
The primary control interface will be a **smartphone over BLE**.

**Decision:** Use BLE smartphone control as the main user interface.  
**Reason:** This fits the portfolio goal better than a purely wired interface and removes the need for a separate Bluetooth module.

**Note:**  
USB/UART is still allowed for:
- firmware upload
- debugging
- development testing

It is **not** the main end-user control method.

---

### 2.3 Animation and data storage
Animations will be stored in the **internal flash memory of the ESP32** for the first revision.

**Decision:** No external EEPROM, SD card, or other external memory in revision 1.  
**Reason:** This reduces hardware complexity, PCB routing effort, failure points, and firmware scope in the first full build.

---

### 2.4 Display type
The cube will be a **monochrome 8×8×8 LED cube**.

**Decision:** Use one LED color only in revision 1.  
**Reason:** A monochrome cube is significantly more realistic for a clean first custom hardware project and keeps the electrical, firmware, and power design under control.

---

### 2.5 Power architecture
The system will use a **5 V main supply** with local regulation for the ESP32.

**Decision:**  
- Main input supply: **5 V DC**
- Logic rail for ESP32: **3.3 V**
- External power adapter target: **5 V / 5 A minimum**

**Reason:**  
5 V is standard and practical for LED driving.  
A dedicated 3.3 V rail is required for the ESP32.  
5 A gives reasonable design margin for multiplexed operation, driver losses, startup conditions, and brightness tuning.

---

### 2.6 Brightness strategy
Brightness will be controlled through **multiplexing timing and firmware brightness control**, not by pushing the LEDs near extreme limits.

**Decision:** Design for a stable, presentable brightness level rather than maximum possible brightness.  
**Reason:** This improves reliability, reduces thermal stress, and keeps the first revision easier to validate.

---

### 2.7 Cost target
The project will stay within a controlled student-level budget.

**Decision:** Target core electronics cost of **about €100 or less**, excluding:
- tools
- shipping
- optional enclosure
- spare parts bought for safety margin

**Reason:** The project is intended as a portfolio build, but it should still remain realistic and defensible as a student engineering project.

---

### 2.8 PCB strategy
The project will use **one main custom PCB** as the base of the cube.

**Decision:** The PCB will contain:
- ESP32 module
- LED driving circuitry
- layer switching circuitry
- power distribution
- programming/debug interface
- connectors/test points as needed

**Reason:** This gives the project a stronger engineering and portfolio value than a breadboard or multi-board prototype and creates a cleaner final build.

---

## 4. Practical interpretation of these decisions

Based on the locked decisions above, the project is now defined as:

> A portfolio-grade **8×8×8 monochrome LED cube** built on a **single custom PCB**, controlled by an **ESP32** with **BLE smartphone control**, powered from **5 V**, and storing animations in **ESP32 internal flash**.

## 5. Explicitly out of scope for revision 1

The following features are **not part of revision 1 unless added by a later revision document**:

- RGB LEDs
- external animation memory
- Wi-Fi cloud control
- full custom mobile app as a required deliverable
- battery power
- modular multi-PCB architecture
- audio-reactive features
- microphone input
- USB as the main user control interface

These may be added later only after the first version is working.

## 6. Immediate consequences for the next project documents

The next documents must assume:

1. **ESP32-based firmware architecture**
2. **BLE communication path**
3. **single-board electrical design**
4. **5 V power input and 3.3 V logic regulation**
5. **monochrome LED drive calculations**
6. **no external memory subsystem**

## 7. Next engineering tasks derived from this decision

The next design steps are:

1. Write the **requirements document**
2. Create the **high-level architecture/block diagram**
3. Do **LED current and power calculations**
4. Choose the **driver topology**
5. Build **BOM v1**
6. Start the **schematic**

## 8. Revision rule

This document is considered the active baseline.  
Any major change to MCU, power system, display type, memory strategy, or PCB strategy must be recorded as a new revision before design work continues.