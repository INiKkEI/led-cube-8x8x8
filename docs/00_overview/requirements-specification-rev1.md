# LED Cube Requirements Specification

## 1. Purpose

This document defines the revision-1 requirements for the **8×8×8 LED Cube** project in clear, testable language. It is the main source of truth for what the first complete version of the project must do and how success will be judged.

## 2. Scope

These requirements apply to revision 1 of the project only. They cover the functional behavior, electrical baseline, implementation constraints, and validation expectations for the first complete build.

## 3. References

- `docs/00_overview/architecture-decisions.md`
- `docs/00_overview/system-architecture.md`
- `docs/00_overview/hardware-firmware-boundary.md`
- `docs/04_validation/test-plan.md`

## 4. Requirement Conventions

Requirement IDs use this format:

- `FR-xx` — functional requirement
- `NFR-xx` — non-functional requirement

Validation methods use these codes:

- **INS** — inspection / document review
- **TEST** — measured or bench-tested verification
- **DEMO** — functional demonstration
- **ANL** — engineering analysis or calculation

## 5. Functional Requirements

| ID | Requirement | Acceptance Criteria | Planned Validation |
|---|---|---|---|
| FR-01 | The system shall implement a **monochrome 8×8×8 LED cube**. | The built assembly contains 8 layers of 8×8 LEDs for a total of **512 LEDs**, and the hardware is intended for one LED color only. | INS |
| FR-02 | The system shall use **one custom ESP32-based main control PCB** as the primary controller platform. | A custom PCB is present and hosts the ESP32-based control electronics used to operate the cube. | INS |
| FR-03 | The system shall use a **dedicated driver stage** between the ESP32 and the cube. | The schematic and assembled design show dedicated circuitry for LED line driving and layer switching, and the cube is not powered directly from ESP32 GPIO current paths. | INS, TEST |
| FR-04 | The display shall use a **multiplexed scan architecture with one active layer at a time**. | During diagnostic operation, layer stepping shows only the intended active layer at each scan step. | TEST |
| FR-05 | The system shall accept **5 V external input power**. | The board accepts the intended 5 V input, and the 5 V rail measures within an acceptable operating range for the ESP32 during bring-up. | TEST |
| FR-06 | The system shall store revision-1 animations and configuration data in **ESP32 internal flash only**. | No external EEPROM, SD card, or external flash device is required for the baseline animation set used in revision 1. | INS, DEMO |
| FR-07 | The firmware shall provide **hardware initialization, refresh scanning, display mapping, animation playback, and diagnostic/test-pattern modes**. | Test firmware and baseline application firmware can initialize the hardware, refresh the cube, show mapped patterns, run at least one built-in animation, and enter a diagnostic mode. | DEMO, TEST |
| FR-08 | The system shall support **BLE-based smartphone control** for basic user commands. | A smartphone or generic BLE client can connect and successfully trigger at least one intended user action such as changing mode, animation, or speed. | DEMO, TEST |
| FR-09 | The system shall provide a **programming/debug interface** for firmware upload and development diagnostics. | Firmware can be uploaded to the ESP32 through the provided development interface, and development diagnostics are available through the same path or an equivalent method. | TEST |
| FR-10 | The implementation shall include a **voxel-addressable display mapping**. | A walking-voxel or equivalent mapping test can command individual positions and the observed LED location matches the intended coordinates throughout the cube. | TEST |
| FR-11 | The implementation shall support **safe bring-up diagnostics independent of final BLE control flow**. | The board can be powered, programmed, and checked using dedicated bring-up or diagnostic firmware without requiring the BLE user path to be fully finished. | TEST, DEMO |
| FR-12 | The project shall produce **basic validation evidence** for revision 1. | The repository contains recorded pass/fail evidence for power-up, layer/line tests, mapping verification, refresh stability, and BLE control checks. | INS |

## 6. Non-Functional Requirements

| ID | Requirement | Acceptance Criteria | Planned Validation |
|---|---|---|---|
| NFR-01 | The design shall target a baseline external supply of **5 V / 5 A** for revision 1 planning. | Power planning, architecture documentation, and later hardware calculations use **5 V / 5 A** as the baseline adapter target unless formally revised. | INS, ANL |
| NFR-02 | The system shall remain electrically stable during normal scan operation. | Normal patterns and baseline animations run without controller resets, brownouts, or obvious corruption caused by supply instability. | TEST |
| NFR-03 | The display shall be visually stable enough for normal demonstration use. | In normal indoor viewing conditions, baseline patterns and animations show no obvious flicker that would undermine demonstration quality. | DEMO |
| NFR-04 | The design shall prioritize reliability and safe operating margins over extreme brightness. | The chosen current, timing, and driver strategy remain within component limits documented by the design work and do not rely on intentionally extreme operating conditions. | ANL, TEST |
| NFR-05 | The core electronics cost target shall be **about €100 or less**, excluding tools, shipping, optional enclosure parts, and spare parts. | The BOM estimate for core revision-1 electronics remains at or below the stated target, or any justified deviation is explicitly documented. | ANL |
| NFR-06 | The firmware shall be organized so that the time-critical scan path remains separated from higher-level animation and BLE handling. | The firmware architecture documentation and implementation structure show a dedicated scan/refresh path separated from non-time-critical logic. | INS |
| NFR-07 | Each major revision-1 requirement shall have a planned validation path. | Every major requirement in this document is referenced by at least one validation method in the test plan or supporting verification evidence. | INS |

## 7. Explicitly Out of Scope for Revision 1

The following are not required for revision 1:

- RGB LED implementation
- external EEPROM, SD card, or external flash as a required storage subsystem
- Wi-Fi or cloud control
- battery-powered operation
- audio-reactive input
- multi-PCB architecture as the baseline hardware solution
- a custom mobile application as a required deliverable

## 8. Traceability Rule

When new hardware, firmware, or validation documents are added, they should reference the requirement IDs from this document where relevant. This keeps future design choices and test results tied to the same baseline.
