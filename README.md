# 8×8×8 LED Cube with Custom ESP32 Control PCB

Portfolio-focused embedded systems project for designing and documenting a custom **8×8×8 LED cube (512 LEDs)** with an **ESP32-based control board**, structured firmware, and transparent engineering documentation.

![Project hero image](media/hero/led-cube-hero.png)

---

## Project Overview

This project is an **8×8×8 monochrome LED cube** being developed as a public engineering portfolio project.

The goal is not only to build the cube itself, but also to document the development process clearly: project planning, system architecture, hardware decisions, firmware development, bring-up, and validation.

Current project direction:

- **Cube size:** 8×8×8
- **Display type:** monochrome LED cube
- **Controller:** ESP32
- **Hardware approach:** custom single-board control PCB
- **Control approach:** BLE to smartphone
- **Main goal:** strong public portfolio project with transparent documentation

---

## Why this project exists

This repository is intended to show more than a simple LED demo. It is being built to demonstrate:

- system-level design thinking
- custom PCB development
- embedded firmware structure
- hardware/software integration
- bring-up and debugging workflow
- verification and documentation discipline

---

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

---

## Repository Structure

```text
docs/
├── 00_overview/              Project summary and system architecture
├── 01_project-management/    Roadmap and planning documents
├── 02_hardware/              BOM and hardware-related documentation
└── 04_validation/            Test plan and future validation results

hardware/                     Hardware design workspace and outputs
firmware/                     ESP32 firmware workspace
media/                        Images, diagrams, photos, and demo media

CHANGELOG.md                  Repository-level change history
README.md                     Public project overview
=======
hardware/                     CAD files, schematics, PCB outputs
firmware/                     ESP32 source code and project files
media/                        Images, diagrams, photos, demo media
CHANGELOG.md                  Milestone-level repository changes
README.md                     Public project overview
