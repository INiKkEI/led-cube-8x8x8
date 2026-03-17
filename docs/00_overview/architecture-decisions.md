# LED Cube Project — Architecture Decisions

**Date:** 2026-03-17  
**Revision:** 1  
**Status:** Locked baseline for revision 1 implementation

## 1. Purpose

This document freezes the top-level technical decisions for revision 1 of the 8×8×8 LED cube project.

From this point forward, requirements, schematic work, PCB work, firmware structure, verification planning, and repository documentation must follow this baseline unless this document is formally revised.

The goal is to remove repeated top-level direction changes and allow implementation to proceed on a stable architecture.

---

## 2. Project identity for revision 1

Revision 1 is defined as:

> A portfolio-grade **8×8×8 monochrome LED cube** built on **one custom ESP32-based control PCB**, using **time-multiplexed layer scanning**, powered from **5 V**, controlled primarily by **BLE from a smartphone**, and storing animations in **ESP32 internal flash**.

This statement is the baseline technical direction for the project.

---

## 3. Locked architecture decisions

### 3.1 Cube size and display type

**Decision:**  
Use a **monochrome 8×8×8 LED cube**.

**Locked meaning:**  
- 512 LEDs total
- one LED color only in revision 1
- no RGB implementation in revision 1

**Reason:**  
This keeps the first full custom build realistic, easier to validate, and much more manageable in hardware, power, routing, and firmware complexity.

---

### 3.2 Main controller

**Decision:**  
Use an **ESP32 module** as the main controller on the custom PCB.

**Locked meaning:**  
- ESP32 is the only central MCU for revision 1
- no secondary application MCU is planned
- wireless capability comes from the ESP32 itself

**Reason:**  
The ESP32 provides sufficient processing headroom, flexible peripherals, and BLE support without adding extra wireless hardware.

---

### 3.3 User control method

**Decision:**  
Use **BLE smartphone control** as the primary end-user control interface.

**Locked meaning:**  
- BLE is the intended user-facing control path
- USB/UART may be used for programming, debugging, logging, and bring-up
- USB/UART is not the main user interface

**Reason:**  
This matches the portfolio goal better than a wired-only interface and keeps the architecture modern without requiring extra communication hardware.

---

### 3.4 Animation and configuration storage

**Decision:**  
Store revision 1 animations and configuration data in **ESP32 internal flash only**.

**Locked meaning:**  
- no external EEPROM
- no SD card
- no external flash memory as a required subsystem in revision 1

**Reason:**  
This reduces hardware scope, firmware scope, routing complexity, and failure points in the first implementation.

---

### 3.5 Display driving method

**Decision:**  
Use a **multiplexed scan architecture with one active cube layer at a time**.

**Locked meaning:**  
- the cube is not driven as 512 continuously powered LEDs
- the display is refreshed slice-by-slice
- one layer is enabled at a time while line data is applied for that layer
- a full image is created by repeated fast refresh of all 8 layers

**Reason:**  
This is the practical architecture for a 512-LED cube because it reduces simultaneous current demand, pin-count pressure, and overall implementation complexity.

---

### 3.6 Driver-stage architecture

**Decision:**  
Use a **dedicated electrical driver stage** between the ESP32 and the LED cube.

**Locked meaning:**  
- the ESP32 must not directly drive the cube power paths
- LED line control and layer switching are handled by dedicated driver circuitry
- the architecture includes:
  - a **line-driving stage** for the active slice data
  - a **layer-switching stage** for layer enable control
- exact IC/transistor part numbers remain implementation-level choices, but the topology itself is now fixed

**Reason:**  
This protects the ESP32 from LED load currents, improves electrical robustness, and removes the remaining top-level ambiguity that would otherwise block schematic and firmware implementation.

---

### 3.7 Power architecture

**Decision:**  
Use a **5 V main input supply** with local **3.3 V regulation** for the ESP32 and low-voltage logic.

**Locked meaning:**  
- main system input: **5 V DC**
- logic rail: **3.3 V**
- display power and logic power must be treated as separate layout/current domains even if they come from the same external source
- design must include local decoupling and bulk capacitance sized for multiplexed current peaks
- external adapter baseline: **5 V / 5 A minimum**

**Reason:**  
5 V is practical for LED drive circuitry, while the ESP32 requires 3.3 V logic. Separating logic and display-current behavior improves stability and reduces noise-related problems.

---

### 3.8 Brightness strategy

**Decision:**  
Control brightness through **refresh timing / dwell time / firmware brightness control**, not by pushing the LEDs to extreme operating conditions.

**Locked meaning:**  
- brightness is achieved by controlled multiplex timing
- reliability and stable operation are preferred over maximum possible brightness
- thermal and current margins take priority over aggressive drive settings

**Reason:**  
This gives a more robust first revision and reduces the risk of thermal stress, unstable operation, and difficult validation.

---

### 3.9 PCB strategy

**Decision:**  
Use **one main custom PCB** as the system base.

**Locked meaning:**  
The main PCB will contain, as applicable:
- ESP32 module
- power input and regulation
- LED line driver circuitry
- layer switching circuitry
- programming/debug access
- connectors and test points required for bring-up and integration

**Reason:**  
A single-board design gives stronger engineering value, better integration, and a cleaner final portfolio result than a breadboard or multi-module prototype.

---

### 3.10 Firmware architecture

**Decision:**  
Use a **layered firmware structure**.

**Locked meaning:**  
Revision 1 firmware must be separated into at least these responsibilities:
- hardware abstraction / low-level IO
- refresh / scan engine
- voxel or frame representation
- animation logic
- communication / control handling
- diagnostics / test patterns

**Additional rule:**  
The scan-refresh path is time-critical and must remain separated from animation and BLE handling.

**Reason:**  
This keeps the firmware maintainable, testable, and much easier to debug during bring-up.

---

### 3.11 Bring-up and validation support

**Decision:**  
Revision 1 must support **diagnostic and test-pattern modes** independently of the final BLE control flow.

**Locked meaning:**  
Implementation must allow:
- basic power-up verification
- line/layer test patterns
- mapping verification
- serial debug output or equivalent development diagnostics

**Reason:**  
Bring-up should not depend on the BLE control layer being fully finished. This reduces implementation risk.

---

### 3.12 Cost target

**Decision:**  
Target core electronics cost of **about €100 or less**, excluding:
- tools
- shipping
- optional enclosure
- spare parts bought as safety margin

**Reason:**  
The project should remain realistic as a student build while still being strong enough for portfolio use.

---

## 4. Explicitly out of scope for revision 1

The following are not part of revision 1 unless this document is revised:

- RGB LEDs
- external animation memory
- Wi-Fi/cloud control
- battery operation
- audio-reactive features
- microphone input
- modular multi-PCB architecture as the baseline design
- USB as the main user control interface
- a full custom mobile app as a required deliverable

These are possible future extensions, not revision 1 requirements.

---

## 5. Implementation rules derived from the architecture

The following are now fixed assumptions for implementation:

1. Hardware design must assume **ESP32 + dedicated driver stage + 8-layer multiplexing**.
2. Firmware design must assume **continuous refresh scanning** independent of BLE timing.
3. Power design must assume **dynamic LED current peaks** and separate logic/display current behavior.
4. Validation must include **power integrity, layer scanning, mapping correctness, brightness stability, and bring-up diagnostics**.
5. Documentation must treat **BLE**, **single-board PCB**, **monochrome display**, and **internal-flash storage** as baseline facts, not open options.

---

## 6. What is still flexible

The following are still implementation-level choices and do not require a top-level architecture revision by themselves:

- exact ESP32 module variant
- exact LED driver IC part numbers
- exact transistor or MOSFET part numbers
- exact connector choice
- exact PCB layout strategy
- exact BLE command format
- exact animation set included in revision 1
- exact firmware file structure

These may be optimized during implementation as long as they remain inside the locked architecture.

---

## 7. Revision 1 Scope

### Purpose
Revision 1 is the minimum complete build of the project. Its purpose is to deliver a working, documented, and verifiable 8×8×8 LED cube without adding non-essential features that increase hardware, firmware, or integration risk.

### Revision 1 must include
1. A functional **8×8×8 monochrome LED cube** with **512 LEDs**.
2. **One custom ESP32-based control PCB** as the main hardware platform.
3. A **dedicated driver stage** between the ESP32 and the cube for LED line driving and layer switching.
4. A **multiplexed display architecture** with **one active layer at a time**.
5. **5 V external power input** with local **3.3 V regulation** for the ESP32 and logic.
6. Firmware that provides:
   - hardware initialization
   - continuous refresh / scan control
   - display mapping
   - a basic built-in animation set
   - animation storage in **ESP32 internal flash only**
   - **BLE-based smartphone control** for basic user commands
   - diagnostic / test-pattern modes for bring-up
7. A **programming/debug interface** for firmware upload and development diagnostics.
8. Basic validation evidence showing:
   - successful power-up
   - layer and line test operation
   - correct LED mapping
   - stable refresh behavior
   - successful BLE control of the cube

### Explicitly out of scope for revision 1
The following are not required for revision 1 and must be treated as later work:
- RGB LED implementation
- external EEPROM, SD card, or external flash for animation storage
- Wi-Fi or cloud control
- battery-powered operation
- audio-reactive features
- microphone input
- multi-PCB modular architecture as the baseline design
- USB as the main end-user control interface
- a full custom mobile app as a required deliverable

### Scope interpretation rules
1. Revision 1 is a **working first complete system**, not a maximum-feature version.
2. A **generic BLE client app** is acceptable for revision 1; a custom app is optional future work.
3. The built-in animation set only needs to be **small but demonstrable**.
4. Bring-up and diagnostics are part of revision 1 because they reduce implementation risk and support verification.
5. Any feature not listed in “must include” is not automatically part of revision 1.

### Documentation consistency rule
- **README** must describe only revision-1 baseline features as planned deliverables.
- **Requirements documents** must treat revision-1 items as mandatory requirements and place all other ideas under future work / later revisions.
- **Architecture documents** must keep the baseline fixed as: ESP32, monochrome 8×8×8 cube, single custom PCB, dedicated driver stage, multiplexed scanning, BLE primary control, internal flash only.
