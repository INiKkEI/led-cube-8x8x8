# Changelog

This file tracks major repository milestones for the LED Cube project.

It is intentionally milestone-level only.
For small edits and daily work sessions, use Git commit history.

## 2026-03-16 — Repository and documentation baseline
- Initialized the repository and base folder structure (`docs/`, `firmware/`, `hardware/`, `media/`).
- Added the initial README and established the project as a portfolio-grade public engineering project.
- Added the first documentation baseline, including architecture/project-planning and verification-related documents.
- Established the repository as the main record for future hardware, firmware, bring-up, and validation milestones.

## 2026-03-17 — Revision 1 documentation baseline alignment
- Completed a repository-wide consistency pass across scope, architecture, requirements, and validation documents.
- Established a clearer revision-1 project baseline with explicit in-scope and out-of-scope boundaries.
- Aligned the core documentation with the ESP32-based LED Cube architecture and corrected cross-document mismatches.
- Strengthened requirement quality by adding measurable acceptance criteria and planned validation paths.
- Prepared the repository for the next phase of detailed hardware, firmware, and implementation planning.

## 2026-03-20 — Electrical design calculations and schematic-preparation baseline
- Reworked `docs/02_hardware/design-calculations.md` into a first-pass electrical design note that is usable during schematic capture.
- Documented the main hardware calculations behind the cube, including LED current assumptions, resistor sizing, multiplexing current, resistor dissipation, and supply-side checks.
- Clarified where exact electrical parts still need to be selected so schematic capture can move from assumptions to real component choices.
- Reviewed the role and expected structure of the `hardware/` workspace in preparation for committing real Altium design source files.
