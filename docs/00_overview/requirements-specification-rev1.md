# Revision 1 Requirements Specification — 8×8×8 LED Cube

## 1. Purpose

This document defines the main system requirements for revision 1 of the 8×8×8 LED Cube project. It translates the current architecture baseline into clear, testable requirements that can later be verified during implementation, bring-up, and validation.

## 2. Scope of Revision 1

Revision 1 is the first complete working version of the system.

It covers:
- one **8×8×8 monochrome LED cube**
- one **custom main PCB** containing the control and drive electronics
- an **ESP32** as the main controller
- **BLE smartphone control** as the main user interface
- **internal flash storage only** for firmware and predefined animations
- operation from an external **5 V DC supply** with local **3.3 V regulation** for the ESP32
- enough firmware structure and diagnostics to support bring-up, demonstration, and later validation

Revision 1 does **not** include:
- RGB LED output
- external EEPROM, SD card, or other external animation storage
- Wi-Fi or cloud-based control as a required feature
- a full custom mobile app as a required deliverable
- battery power
- audio-reactive features
- microphone input
- USB as the main end-user control method
- modular multi-board architecture

## 3. System Summary

The system is a multiplexed 8×8×8 LED cube containing 512 monochrome LEDs. An ESP32-based custom PCB shall generate display data, drive the cube through dedicated driver circuitry, manage system state, store animations in internal flash, and accept user control over BLE.

## 4. Requirement Conventions

- Functional requirements are labeled **FR-x**.
- Non-functional requirements are labeled **NFR-x**.
- Each requirement is written so it can be checked later by inspection, test, or demonstration.
- The word **shall** indicates a mandatory requirement.

## 5. Functional Requirements

### 5.1 System Configuration and Operating States

| ID | Requirement | Verification |
|---|---|---|
| FR-01 | The system shall implement one **8×8×8 monochrome LED cube** containing **512 LEDs**. | Inspection |
| FR-02 | The system shall use an **ESP32** mounted on the custom main PCB as the main controller. | Inspection |
| FR-03 | The system shall operate from one external **5 V DC** power input. | Inspection / Test |
| FR-04 | The system shall generate a regulated **3.3 V rail** for the ESP32 and other 3.3 V logic, if used. | Inspection / Measurement |
| FR-05 | On power-up, the system shall enter an initialization state and then transition to a ready or active display state without requiring manual reset. | Test |
| FR-06 | The firmware shall support at least these operating states: **Initialization**, **Idle/Ready**, **Active Display**, and **Diagnostic/Test**. | Inspection / Test |
| FR-07 | The system shall provide a defined default behavior after boot. If no user command is received, it shall automatically start a default animation. | Test |

### 5.2 LED Cube Display Control

| ID | Requirement | Verification |
|---|---|---|
| FR-08 | The firmware shall represent the cube state in software as an addressable **8×8×8 voxel space**. | Inspection |
| FR-09 | The system shall be able to control every voxel position in the 8×8×8 cube individually through firmware. | Test |
| FR-10 | The display shall use a **multiplexed scanning architecture** and shall refresh all cube layers continuously during active display operation. | Inspection / Test |
| FR-11 | The firmware shall generate the electrical output pattern for each active layer from the current logical cube state. | Inspection / Test |
| FR-12 | The system shall support a global display clear state in which all LEDs are off. | Test |
| FR-13 | The system shall support a global display on/test state in which all addressable LEDs can be commanded on within the limits of the multiplexing scheme. | Test |
| FR-14 | The system shall support at least one single-voxel test function for checking cube mapping and wiring correctness. | Test |
| FR-15 | The system shall support global brightness control implemented in firmware through scan timing and/or other firmware-controlled brightness methods. | Test |

### 5.3 Animation Functions

| ID | Requirement | Verification |
|---|---|---|
| FR-16 | The system shall store predefined animations in **ESP32 internal flash memory** only. | Inspection |
| FR-17 | The system shall not require any external EEPROM, SD card, or other external memory device to run revision 1 animations. | Inspection |
| FR-18 | The firmware shall include at least **five** predefined animations available without recompilation after flashing the revision 1 firmware. | Demonstration |
| FR-19 | The firmware shall support switching between stored animations during runtime without requiring a power cycle. | Test |
| FR-20 | The firmware shall support a global animation speed setting that affects runtime playback speed. | Test |

### 5.4 User Control and Communication

| ID | Requirement | Verification |
|---|---|---|
| FR-21 | The primary end-user control interface shall be **BLE communication with a smartphone**. | Inspection / Demonstration |
| FR-22 | The system shall expose a BLE-based control path that allows a user to select the active animation. | Test |
| FR-23 | The system shall expose a BLE-based control path that allows a user to adjust global brightness. | Test |
| FR-24 | The system shall expose a BLE-based control path that allows a user to adjust animation speed. | Test |
| FR-25 | The system shall continue running the currently active animation if the BLE connection is lost. | Test |
| FR-26 | USB/UART may be used for programming, debug, and development testing, but it shall not be the required main end-user control interface for revision 1. | Inspection |

### 5.5 Diagnostics and Bring-Up Support

| ID | Requirement | Verification |
|---|---|---|
| FR-27 | The firmware shall include a **diagnostic/test mode** intended for bring-up and troubleshooting. | Demonstration |
| FR-28 | Diagnostic/test mode shall provide at least these test patterns: **full-cube test**, **layer-by-layer sweep**, and **single-voxel or line-based mapping test**. | Demonstration |
| FR-29 | The system shall provide a firmware upload path for the ESP32 during development and maintenance. | Inspection / Test |
| FR-30 | The design shall include enough electrical access for hardware verification during bring-up, such as connectors, headers, or test points where needed. | Inspection |

## 6. Non-Functional Requirements

### 6.1 Performance

| ID | Requirement | Verification |
|---|---|---|
| NFR-01 | During normal operation, the display refresh shall be high enough that the cube appears visually stable to an observer under normal indoor viewing conditions. | Demonstration |
| NFR-02 | The full-frame refresh rate shall be at least **100 Hz** in the normal active display mode. | Measurement |
| NFR-03 | After application of a valid BLE control command, the visible effect of that command shall begin within **1 second** under normal operation. | Test |
| NFR-04 | From power application, the system shall produce its first intentional visible output within **3 seconds**. | Test |

### 6.2 Electrical and Power Constraints

| ID | Requirement | Verification |
|---|---|---|
| NFR-05 | The complete revision 1 system shall be operable from one external **5 V / 5 A minimum** DC adapter. | Test |
| NFR-06 | Logic power and LED power shall be treated as separate electrical domains in the design, even if they originate from the same external 5 V source. | Inspection |
| NFR-07 | The design shall include local decoupling near controller and driver devices and bulk capacitance near dynamic LED load sections. | Inspection |
| NFR-08 | Under normal operation, no part of the design shall require a supply rail above **5 V DC**. | Inspection |

### 6.3 Architecture and Maintainability

| ID | Requirement | Verification |
|---|---|---|
| NFR-09 | Revision 1 shall be implemented on a **single custom main PCB**. | Inspection |
| NFR-10 | Firmware shall be structured into separate responsibilities for at least: **hardware access**, **display refresh**, **animation logic**, and **communication/control**. | Inspection |
| NFR-11 | The design shall be documented well enough that hardware, firmware, and validation documents can be traced back to these requirements. | Inspection |
| NFR-12 | The logical voxel mapping layer shall be separated from raw physical output ordering so that physical wiring order does not have to match animation-space order directly. | Inspection |

### 6.4 Reliability and Robustness

| ID | Requirement | Verification |
|---|---|---|
| NFR-13 | Loss and reconnection of the BLE link shall not require a full system power cycle to resume normal operation. | Test |
| NFR-14 | The system shall remain functionally stable during continuous active display operation for at least **30 minutes** without unintended reset or display freeze. | Endurance Test |
| NFR-15 | Diagnostic/test functions shall be available without modifying hardware. | Test |

### 6.5 Project and Build Constraints

| ID | Requirement | Verification |
|---|---|---|
| NFR-16 | The target core electronics cost for revision 1 shall be **about €100 or less**, excluding tools, shipping, optional enclosure, and spare parts. | BOM Review |
| NFR-17 | Revision 1 shall remain a **monochrome** design. Multi-color or RGB output is out of scope. | Inspection |
| NFR-18 | Revision 1 shall not depend on any external memory subsystem for normal animation playback. | Inspection |
| NFR-19 | Revision 1 shall not require a custom mobile application to demonstrate its main user control path; a generic BLE-capable control method is acceptable. | Demonstration |

## 7. Explicit Out-of-Scope Items

The following items are explicitly excluded from revision 1 and shall not be treated as required features during design verification:
- RGB or multi-color LED output
- battery-powered operation
- cloud connectivity
- Wi-Fi as a required control path
- audio-reactive operation
- microphone input
- external animation memory
- USB as the primary user control interface
- multi-board distributed architecture

## 8. Acceptance Criteria for Revision 1

Revision 1 shall be considered functionally complete when all of the following are true:
1. A custom PCB-based ESP32 system drives a monochrome 8×8×8 LED cube.
2. The cube can display stored animations from internal flash.
3. A smartphone can change at least animation, brightness, and speed through BLE.
4. Diagnostic patterns confirm basic cube mapping and layer operation.
5. The system runs from a 5 V external supply with local 3.3 V regulation.
6. No out-of-scope features are required to demonstrate a complete revision 1 build.

## 9. Recommended Repository Location

Recommended file path in the repository:

```text
docs/00_overview/requirements-specification.md
```

This keeps the requirements document close to the existing overview and architecture baseline documents.
