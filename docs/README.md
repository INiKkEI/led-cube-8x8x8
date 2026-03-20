# Documentation Overview

This folder contains the engineering documentation for the LED Cube project.

## Sections

- `00_overview/` — project baseline, requirements, system architecture, and hardware/firmware boundary
- `01_project-management/` — factual build history and project risk tracking
- `02_hardware/` — hardware design notes, calculations, topology choices, BOM, and assembly planning
- `03_firmware/` — firmware architecture, state handling, BLE protocol notes, and animation-engine planning
- `04_validation/` — bring-up and formal validation planning
- `05_portfolio/` — final presentation material for recruiters and portfolio use

## Source-of-Truth Rules

To reduce duplication and drift:

- locked top-level decisions belong in `00_overview/architecture-decisions.md`
- mandatory revision-1 requirements belong in `00_overview/requirements-specification-rev1.md`
- validation method and test coverage belong in `04_validation/test-plan.md`
- detailed implementation notes belong in the matching hardware or firmware section
- portfolio-facing summaries belong in `05_portfolio/`

Each topic should have one main source of truth rather than being repeated across multiple files.
