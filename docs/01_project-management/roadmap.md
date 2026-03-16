# Project Roadmap

## Purpose
This roadmap shows the planned execution path for the 8×8×8 LED Cube project from completed concept work to final validated portfolio release. It is intended to keep development focused, visible, and easy to track.

## Roadmap Overview

| Phase | Status | Main Goal | Key Outputs |
|---|---|---|---|
| 1. Project Definition | Done | Define scope, goals, and documentation structure | Project summary, requirements, repository structure |
| 2. System Architecture | Done | Define system-level design and major technical decisions | Architecture document, block diagrams, interface definitions |
| 3. Hardware Design | Done | Design the LED cube electronics and PCB | Schematic, PCB layout, manufacturing files |
| 4. Hardware Assembly | In Progress / Done after physical build confirmation | Assemble PCB and cube hardware safely and correctly | Soldered PCB, cube structure, bring-up notes |
| 5. Firmware Foundation | In Progress | Establish low-level firmware needed to operate the cube | GPIO mapping, scan engine, timing control, test code |
| 6. Functional Bring-Up | Planned / In Progress | Prove the cube can display stable patterns and basic animations | Layer test, LED test, first animation demo |
| 7. Control and Effects | Planned | Add higher-level animation and control features | Animation engine, presets, control interface |
| 8. Validation and Debugging | Planned | Verify performance, stability, power behavior, and correctness | Test results, issue logs, fixes, validation notes |
| 9. Portfolio Packaging | Planned | Present the project professionally for GitHub and recruiters | Final README updates, media, final report, release package |

## Milestones

### M1 — Documentation Baseline
**Target:** completed  
**Description:** Project scope, requirements, and structure are defined.  
**Deliverables:**
- Project summary
- Requirements documentation
- Initial repository structure
- Planning documents

### M2 — Architecture Freeze
**Target:** completed  
**Description:** High-level hardware and firmware architecture is defined and stable enough to start implementation.  
**Deliverables:**
- System architecture document
- Block diagrams
- Main design decisions
- Interface definitions

### M3 — Hardware Design Complete
**Target:** completed  
**Description:** Main control PCB and electrical design are ready for manufacturing.  
**Deliverables:**
- Schematic
- PCB layout
- Design exports
- Manufacturing files

### M4 — First Power-On
**Target:** next major checkpoint  
**Description:** Hardware is powered safely and the first electrical checks are completed without damage.  
**Deliverables:**
- Bring-up checklist
- Voltage rail checks
- Short-circuit inspection results
- Initial hardware issue log

### M5 — LED Scan Engine Running
**Target:** after first power-on  
**Description:** Firmware can drive rows/layers reliably and produce stable light output.  
**Deliverables:**
- GPIO initialization
- Multiplexing logic
- Timing control
- Basic test patterns

### M6 — First Full-Cube Demo
**Target:** after scan engine validation  
**Description:** The cube displays a complete 3D pattern or animation across all LEDs.  
**Deliverables:**
- Full cube test video
- Animation demo
- Known limitations list
- Firmware revision milestone

### M7 — Feature Expansion
**Target:** after stable basic operation  
**Description:** Higher-level control features are added to make the project stronger as a portfolio piece.  
**Deliverables:**
- Multiple animations
- Better code structure
- Optional external control features
- Improved configuration and maintainability

### M8 — Validation Complete
**Target:** before final release  
**Description:** The cube is tested against project requirements and major risks are addressed.  
**Deliverables:**
- Validation report
- Power and thermal observations
- Functional verification results
- Final bug list and fixes

### M9 — Final Portfolio Release
**Target:** final project milestone  
**Description:** The project is packaged as a polished public engineering portfolio entry.  
**Deliverables:**
- Clean repository
- Updated README
- Photos and demo media
- Final documentation set
- Versioned release

## Current Focus
The immediate priority is to move from completed design work into reliable physical bring-up and firmware-driven verification.

### Near-Term Tasks
- Verify assembled hardware before extended power testing
- Perform first power-on checks
- Confirm power rails and signal integrity
- Bring up basic LED scanning firmware
- Test individual LEDs, rows, columns, and layers
- Record bring-up issues and fixes
- Capture early demo media

## Medium-Term Tasks
- Improve animation quality and timing
- Optimize firmware structure
- Add more demonstration patterns
- Validate stability under sustained operation
- Document measured behavior and limitations

## Long-Term Tasks
- Finalize validation documents
- Improve portfolio presentation
- Record final photos and videos
- Prepare final public release

## Exit Criteria by Phase

### Hardware Assembly Exit Criteria
- PCB assembled
- No visible shorts or major assembly faults
- Cube wiring completed
- Ready for controlled power-on

### Firmware Foundation Exit Criteria
- Pins mapped correctly
- Basic scan logic works
- Repeatable test patterns run
- No critical timing instability

### Functional Bring-Up Exit Criteria
- Entire cube can be addressed
- Basic 3D effects work
- Brightness is visually acceptable
- No persistent hardware faults blocking demos

### Validation Exit Criteria
- Core requirements checked
- Main risks reviewed
- Results documented
- Final known issues clearly stated

## Notes
This roadmap is a living document. It should be updated whenever a phase is completed, delayed, re-scoped, or split into smaller milestones.