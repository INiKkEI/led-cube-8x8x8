# 8×8×8 LED Cube with Custom ESP32 Control PCB

Portfolio-focused embedded-systems project for designing, building, and documenting a custom **8×8×8 monochrome LED cube** driven by an **ESP32-based control PCB**.

![Project hero image](media/hero/led-cube-hero.png)

## Project Overview

This repository documents a complete revision-1 build of an **8×8×8 LED cube (512 LEDs)**. The goal is not only to produce a working display, but also to show a clear engineering process: requirements, architecture, hardware design, firmware structure, bring-up, validation, and final presentation.

## Revision 1 Baseline

Revision 1 is based on these locked decisions:

- **Display:** monochrome 8×8×8 LED cube
- **Controller:** ESP32 on one custom main PCB
- **Drive method:** dedicated driver stage with multiplexed layer scanning
- **User control:** BLE from a smartphone
- **Storage:** ESP32 internal flash only
- **Power:** 5 V external input with local 3.3 V regulation
- **Power target:** baseline adapter target **5 V / 3 A**
- **Main goal:** portfolio-grade build with transparent documentation

The full baseline is defined in:

- `docs/00_overview/architecture-decisions.md`
- `docs/00_overview/requirements-specification.md`
- `docs/00_overview/system-architecture.md`

## Why this project exists

This repository is intended to demonstrate more than a simple LED effect demo. It is structured to show:

- system-level design
- custom PCB development
- embedded firmware organization
- hardware/firmware integration
- bring-up and debugging discipline
- traceable verification
- professional project documentation

## Repository Structure

```text
docs/
├── README.md
├── 00_overview/
│   ├── architecture-decisions.md
│   ├── hardware-firmware-boundary.md
│   ├── project-summary.md
│   ├── requirements-specification.md
│   └── system-architecture.md
├── 01_project-management/
│   ├── build-log.md
│   └── risks.md
├── 02_hardware/
│   ├── assembly-strategy.md
│   ├── bom.md
│   ├── design-calculations.md
│   ├── driver-topology.md
│   └── power-budget.md
├── 03_firmware/
│   ├── animation-engine.md
│   ├── ble-protocol.md
│   ├── firmware-architecture.md
│   └── state-machine.md
├── 04_validation/
│   ├── bring-up-checklist.md
│   └── test-plan.md
└── 05_portfolio/
    ├── final-story.md
    ├── lessons-learned.md
    └── recruiter-summary.md

hardware/     Hardware design workspace and outputs
firmware/     Firmware project workspace
media/        Images, diagrams, photos, and demo media

CHANGELOG.md  Repository-level milestone history
README.md     Public landing page
```

## Documentation Map

Use the repository in this order:

1. `docs/00_overview/architecture-decisions.md` for the locked technical baseline
2. `docs/00_overview/requirements-specification.md` for what revision 1 must satisfy
3. `docs/00_overview/system-architecture.md` for subsystem structure and interfaces
4. `docs/04_validation/test-plan.md` for how the requirements will be verified
5. topic-specific hardware, firmware, and portfolio folders for detailed follow-on work

## Status

The repository currently focuses on locking the revision-1 definition and preparing the design, implementation, and validation structure. Hardware implementation, firmware implementation, and final evidence collection will be added as the project progresses.
