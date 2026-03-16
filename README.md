# 8×8×8 LED Cube with Custom ESP32 Control PCB

Portfolio-focused embedded systems project for designing and documenting a custom **8×8×8 LED cube (512 LEDs)** with an **ESP32-based control board**, structured firmware, and transparent engineering documentation.

![Project hero image](media/hero/led-cube-hero.png)

---

## Project Overview

This repository is intended to become a complete engineering case study, not just a visual LED demo. The goal is to document the full path from concept and architecture decisions to hardware design, firmware bring-up, validation, and final presentation.

Current project direction:
- **Cube size:** 8×8×8
- **Display type:** monochrome LED cube
- **Controller:** ESP32
- **Hardware approach:** custom single-board control PCB
- **Control approach:** BLE to smartphone
- **Main goal:** strong public portfolio project with transparent documentation

---

## Why this project exists

This project is being built to demonstrate:
- system-level design thinking
- custom PCB development
- embedded firmware structure
- hardware/software integration
- bring-up and debugging workflow
- verification and documentation discipline

---

## Planned Features

- 8×8×8 LED cube with 512 LEDs
- Custom ESP32-based controller PCB
- Multiplexed LED scanning architecture
- Animation engine for 3D effects
- BLE-based user control
- Structured power distribution and current handling
- Bring-up, validation, and measurement records
- Demo media and final portfolio packaging

---

## Repository Structure

```text
docs/
├── 00_overview/              Project summary, architecture, decisions
├── 01_project-management/    Roadmap, risks, build log
├── 02_hardware/              Hardware documentation
├── 03_firmware/              Firmware documentation
└── 04_validation/            Test planning and future results

hardware/                     CAD files, schematics, PCB outputs
firmware/                     ESP32 source code and project files
media/                        Images, diagrams, photos, demo media
CHANGELOG.md                  Milestone-level repository changes
README.md                     Public project overview
```

## Current Focus

The immediate focus is to turn the documentation baseline into committed implementation work:

- add the requirements/specification document
- start the real hardware design files
- start the real firmware source tree
- replace placeholder progress claims with measured evidence as work is completed

## Documentation Policy

This repository is intended to track the project directly in Git:

- build-log.md records work sessions and problems found
- roadmap.md tracks phase-level progress
- risks.md tracks active project risks
- CHANGELOG.md records milestone-level changes
