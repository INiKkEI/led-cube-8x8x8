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
