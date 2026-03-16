# Bring-Up Checklist

## Document Control
- **Project:** 8×8×8 LED Cube with Custom ESP32 Control PCB
- **Document:** Bring-Up Checklist
- **Repository section:** `docs/04_validation/bring-up-checklist.md`
- **Status:** Draft

## 1. Purpose
This checklist is used during first hardware power-up, initial firmware loading, and first functional verification of the LED cube system. Its goal is to reduce the risk of damaging the custom PCB, ESP32, LED drivers, or the LED cube during early testing.

## 2. Scope
This document covers:
- pre-power inspection of the assembled hardware
- safe first power-on of the main PCB
- ESP32 connectivity and firmware upload checks
- first LED driver and layer switching checks
- basic cube functionality checks
- logging of issues found during bring-up

It does not replace the full validation procedure from the test plan.

## 3. Required Equipment
- bench power supply set to **5 V** with adjustable current limit
- USB cable for ESP32 programming and serial logging
- digital multimeter
- computer with firmware toolchain installed
- latest bring-up firmware / test firmware
- assembled main PCB
- assembled 8×8×8 LED cube
- soldering tools for rework if needed

## 4. Safety Rules
- Do not connect the full cube immediately if the main PCB has not passed basic power checks.
- Start with a low current limit on the bench supply.
- Power off immediately if any IC, regulator, transistor, or LED becomes unusually hot.
- Do not reflash firmware while power integrity problems are suspected.
- Record every fault before rework so the troubleshooting history is preserved.

## 5. Pre-Power Visual Inspection
Mark each item when complete.

| ID | Check | Result | Notes |
|---|---|---|---|
| BUC-01 | PCB matches latest schematic and assembly revision | ☐ Pass / ☐ Fail | |
| BUC-02 | No visible solder bridges between adjacent pins or pads | ☐ Pass / ☐ Fail | |
| BUC-03 | No cold joints, cracked joints, or unsoldered pins | ☐ Pass / ☐ Fail | |
| BUC-04 | All polarized parts are oriented correctly | ☐ Pass / ☐ Fail | |
| BUC-05 | ESP32 module orientation is correct | ☐ Pass / ☐ Fail | |
| BUC-06 | Driver IC orientation is correct | ☐ Pass / ☐ Fail | |
| BUC-07 | Transistors / MOSFETs are placed in the correct orientation | ☐ Pass / ☐ Fail | |
| BUC-08 | Decoupling capacitors are fitted at required locations | ☐ Pass / ☐ Fail | |
| BUC-09 | Power connector polarity is clearly verified | ☐ Pass / ☐ Fail | |
| BUC-10 | Cube connector / header alignment is correct | ☐ Pass / ☐ Fail | |
| BUC-11 | No damaged traces, lifted pads, or broken vias visible | ☐ Pass / ☐ Fail | |
| BUC-12 | Board is clean from loose solder balls and flux residue in critical areas | ☐ Pass / ☐ Fail | |

## 6. Unpowered Electrical Checks
Perform these checks before any power is applied.

| ID | Check | Target / Method | Result | Notes |
|---|---|---|---|---|
| BUC-13 | Resistance from 5 V to GND | Not a short circuit | ☐ Pass / ☐ Fail | |
| BUC-14 | Continuity of main ground net | Low resistance across ground points | ☐ Pass / ☐ Fail | |
| BUC-15 | Continuity from power input to board 5 V rail | Continuous | ☐ Pass / ☐ Fail | |
| BUC-16 | No short between 3.3 V rail and GND | Not a short circuit | ☐ Pass / ☐ Fail | |
| BUC-17 | Critical ESP32 power pins connected correctly | Verified against schematic | ☐ Pass / ☐ Fail | |
| BUC-18 | Driver power pins connected correctly | Verified against schematic | ☐ Pass / ☐ Fail | |
| BUC-19 | Layer control lines are not shorted together | Continuity check | ☐ Pass / ☐ Fail | |
| BUC-20 | Column / row data lines are not shorted together | Continuity check | ☐ Pass / ☐ Fail | |

## 7. First Power-Up of Main PCB
Recommended sequence:
1. Disconnect the cube if possible and power the control board alone first.
2. Set bench supply to **5.0 V**.
3. Set current limit to a conservative starting value.
4. Apply power and watch current draw immediately.
5. Measure local voltage rails before proceeding.

| ID | Check | Acceptance Criteria | Result | Notes |
|---|---|---|---|---|
| BUC-21 | Initial current draw is reasonable | No immediate current-limit hit or abnormal surge | ☐ Pass / ☐ Fail | |
| BUC-22 | 5 V rail measured on board | Within expected tolerance | ☐ Pass / ☐ Fail | |
| BUC-23 | 3.3 V rail measured on board | Within expected tolerance | ☐ Pass / ☐ Fail | |
| BUC-24 | No component heating within first 60 s | Touch-safe / no abnormal heating | ☐ Pass / ☐ Fail | |
| BUC-25 | ESP32 powers up | Boot indication or serial output present | ☐ Pass / ☐ Fail | |
| BUC-26 | USB connection to ESP32 is recognized by PC | Port enumerates correctly | ☐ Pass / ☐ Fail | |

## 8. Firmware Bring-Up
Use a simple test firmware first, not the full animation firmware.

Recommended first test firmware features:
- serial boot message
- heartbeat indicator
- manual layer stepping
- simple pattern output to LED drivers
- low brightness / low duty cycle operation

| ID | Check | Acceptance Criteria | Result | Notes |
|---|---|---|---|---|
| BUC-27 | Test firmware builds successfully | No build errors | ☐ Pass / ☐ Fail | |
| BUC-28 | Test firmware uploads successfully | Upload completes without errors | ☐ Pass / ☐ Fail | |
| BUC-29 | Serial monitor shows boot log | Readable startup output | ☐ Pass / ☐ Fail | |
| BUC-30 | GPIO test pattern runs | Expected logic activity present | ☐ Pass / ☐ Fail | |
| BUC-31 | No watchdog reset / brownout during idle | Stable operation for at least several minutes | ☐ Pass / ☐ Fail | |

## 9. LED Driver and Layer Switching Checks
Perform this stage only after basic board power is confirmed stable.

| ID | Check | Acceptance Criteria | Result | Notes |
|---|---|---|---|---|
| BUC-32 | One known LED can be turned on intentionally | Correct LED responds | ☐ Pass / ☐ Fail | |
| BUC-33 | One known LED can be turned off intentionally | LED turns fully off | ☐ Pass / ☐ Fail | |
| BUC-34 | Single-layer activation works | Only intended layer is active | ☐ Pass / ☐ Fail | |
| BUC-35 | Layer stepping works in correct order | No missing or swapped layers | ☐ Pass / ☐ Fail | |
| BUC-36 | Column / voxel addressing is correct | Selected coordinates match firmware command | ☐ Pass / ☐ Fail | |
| BUC-37 | No ghosting in static single-voxel test | Non-selected LEDs remain off | ☐ Pass / ☐ Fail | |
| BUC-38 | No obvious brightness collapse during multiplexing | Display remains stable | ☐ Pass / ☐ Fail | |

## 10. First Full Cube Functional Checks
After basic addressing is verified, continue with limited full-system checks.

| ID | Check | Acceptance Criteria | Result | Notes |
|---|---|---|---|---|
| BUC-39 | All 8 layers can be addressed | Each layer responds | ☐ Pass / ☐ Fail | |
| BUC-40 | Representative LEDs across the cube light correctly | No major dead zones | ☐ Pass / ☐ Fail | |
| BUC-41 | Simple full-layer pattern renders correctly | Shape matches expected pattern | ☐ Pass / ☐ Fail | |
| BUC-42 | Basic animation runs without reset | Stable execution | ☐ Pass / ☐ Fail | |
| BUC-43 | System remains stable for short run test | No overheating, reset, or visual corruption | ☐ Pass / ☐ Fail | |

## 11. Optional Interface Checks
Use this section only if the corresponding feature is already implemented.

| ID | Check | Acceptance Criteria | Result | Notes |
|---|---|---|---|---|
| BUC-44 | BLE advertising starts | Device visible to phone | ☐ Pass / ☐ Fail / ☐ N/A | |
| BUC-45 | BLE connection can be established | Connection stable | ☐ Pass / ☐ Fail / ☐ N/A | |
| BUC-46 | Command from phone changes pattern | Expected response from cube | ☐ Pass / ☐ Fail / ☐ N/A | |

## 12. Fault Log
Record every issue found during bring-up.

| Fault ID | Stage | Symptom | Suspected Cause | Action Taken | Status |
|---|---|---|---|---|---|
| F-01 |  |  |  |  | Open / Closed |
| F-02 |  |  |  |  | Open / Closed |
| F-03 |  |  |  |  | Open / Closed |

## 13. Bring-Up Decision
Use this section at the end of the session.

| Decision Item | Status | Notes |
|---|---|---|
| Safe to continue with extended firmware tests | ☐ Yes / ☐ No | |
| Safe to continue with full brightness testing | ☐ Yes / ☐ No | |
| Rework required before next test session | ☐ Yes / ☐ No | |
| Test plan execution may start | ☐ Yes / ☐ No | |

## 14. Session Record
- **Date:**
- **Board revision:**
- **Firmware revision / commit:**
- **Tester:**
- **Overall result:** ☐ Pass / ☐ Conditional pass / ☐ Fail
- **Summary notes:**
