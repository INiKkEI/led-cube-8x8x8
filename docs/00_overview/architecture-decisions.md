# Revision 1 Scope

## Purpose
Revision 1 is the minimum complete build of the project. Its purpose is to deliver a working, documented, and verifiable 8×8×8 LED cube without adding non-essential features that increase hardware, firmware, or integration risk.

## Revision 1 must include
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

## Explicitly out of scope for revision 1
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

## Scope interpretation rules
1. Revision 1 is a **working first complete system**, not a maximum-feature version.
2. A **generic BLE client app** is acceptable for revision 1; a custom app is optional future work.
3. The built-in animation set only needs to be **small but demonstrable**.
4. Bring-up and diagnostics are part of revision 1 because they reduce implementation risk and support verification.
5. Any feature not listed in “must include” is not automatically part of revision 1.

## Documentation consistency rule
- **README** must describe only revision-1 baseline features as planned deliverables.
- **Requirements documents** must treat revision-1 items as mandatory requirements and place all other ideas under future work / later revisions.
- **Architecture documents** must keep the baseline fixed as: ESP32, monochrome 8×8×8 cube, single custom PCB, dedicated driver stage, multiplexed scanning, BLE primary control, internal flash only.
