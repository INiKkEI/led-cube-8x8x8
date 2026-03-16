# System Architecture — 8×8×8 LED Cube

## 1. Purpose

This document defines the high-level system architecture of the 8×8×8 LED Cube project. It explains how the main subsystems interact, what responsibilities each subsystem has, and which design decisions shape the overall solution.

The document is intended to support:

- project documentation in the repository
- hardware and firmware implementation work
- later verification and validation activities
- portfolio presentation of the project as a complete embedded system

---

## 2. System Overview

The project is a custom-built 8×8×8 LED cube containing 512 LEDs. The cube is controlled by an ESP32-based custom main PCB that manages LED scanning, animation generation, power distribution, and external control.

The system is designed as a complete embedded product rather than only a visual demo. This means the architecture must support not only light output, but also maintainability, documentation, validation, and clear hardware/firmware separation.

At system level, the LED cube can be viewed as four cooperating layers:

1. **User interaction layer** — external control and configuration input
2. **Control and processing layer** — ESP32 firmware, timing, animation logic, and state handling
3. **LED drive layer** — electrical interface between controller logic and the 8×8×8 LED structure
4. **Power layer** — stable supply and current delivery for logic and LEDs

---

## 3. Architectural Drivers

The architecture is driven by the following project goals and constraints:

### 3.1 Functional drivers

- Control a full **8×8×8 LED cube (512 LEDs)**
- Generate 3D visual effects and animations
- Refresh the display fast enough to avoid visible flicker
- Support structured firmware rather than a single monolithic loop
- Allow future external control features through the ESP32 platform

### 3.2 Non-functional drivers

- Use a **custom single-board hardware design**
- Keep the design portfolio-friendly and well documented
- Separate hardware-specific code from animation/application logic
- Make the architecture testable during bring-up and validation
- Keep the design maintainable for future revisions

### 3.3 Engineering constraints

- The cube cannot drive all LEDs continuously at full static current; it must use a multiplexed scanning architecture
- LED current switching and logic control must be electrically separated from high-level animation logic
- The power subsystem must handle dynamic load changes caused by scanning and animation patterns
- The PCB should simplify wiring and integration compared to a breadboard or multi-board prototype

---

## 4. Top-Level Architecture

```text
+-----------------------+
| External User / Tool  |
| - demo control        |
| - config / commands   |
+-----------+-----------+
            |
            v
+-----------------------+
| ESP32 Control Layer   |
| - system state        |
| - animation engine    |
| - frame generation    |
| - timing control      |
| - communication       |
+-----------+-----------+
            |
            v
+-----------------------+
| LED Driver Layer      |
| - row/column control  |
| - layer selection     |
| - current switching   |
+-----------+-----------+
            |
            v
+-----------------------+
| 8x8x8 LED Cube        |
| - 512 LEDs            |
| - multiplexed display |
+-----------------------+

+-----------------------+
| Power Subsystem       |
| - DC input            |
| - regulation/filter   |
| - logic + LED rails   |
+-----------------------+
```

The ESP32 is the architectural center of the system. It does not directly represent the visual content one LED at a time from user logic. Instead, it manages a display model, converts it into scan data, and repeatedly updates the hardware driver stage.

---

## 5. Hardware Architecture

## 5.1 Main hardware blocks

The hardware architecture is divided into the following major blocks:

### A. DC power input and conditioning
Responsible for accepting the system power input and distributing it safely to logic and LED-driving circuitry.

Main responsibilities:

- receive the external supply
- provide stable voltage rails
- reduce supply noise and transient disturbance
- support peak current demands during LED scanning

### B. ESP32 control subsystem
This subsystem contains the main microcontroller and its support circuitry.

Main responsibilities:

- run firmware and animation logic
- generate timing and scan-control signals
- manage communication and configuration interfaces
- coordinate the full cube refresh cycle

Typical support functions include:

- power regulation/decoupling for the controller
- boot and programming access
- reset and debug access

### C. LED driver subsystem
This subsystem forms the electrical bridge between the ESP32 and the LED cube.

Main responsibilities:

- translate controller signals into drive signals suitable for the cube
- switch active layers/planes in the multiplexing sequence
- control LED line states for the currently active slice
- protect the controller from direct high-current LED switching loads

At architecture level, this block is treated as a dedicated driver stage, regardless of the final exact implementation details at component level.

### D. LED cube load
The 8×8×8 cube is the visual output device.

Main responsibilities:

- convert time-multiplexed electrical drive into a 3D visual image
- display one active layer at a time while relying on persistence of vision
- reflect the frame data produced by the firmware

---

## 5.2 Hardware interaction model

The hardware operates in a scan-based manner:

1. The ESP32 prepares output data for one cube layer.
2. The LED driver subsystem applies the correct line states.
3. One layer is enabled for a short time window.
4. The layer is disabled.
5. The process repeats for the next layer.
6. A full frame is produced after all layers are refreshed.

This approach reduces the number of simultaneously driven LEDs and makes a 512-LED cube practical with a compact embedded controller.

---

## 5.3 Power architecture view

The power architecture should be considered as two logical domains:

- **logic domain** — ESP32 and low-power support circuitry
- **display power domain** — LED drive path and switching currents

Even if both domains come from the same external source, the architecture should treat them separately in layout and filtering because their electrical behavior is different.

Key design considerations:

- local decoupling near controller and driver devices
- bulk capacitance near dynamic LED load sections
- short high-current return paths
- controlled grounding strategy to reduce visible display artifacts and controller instability

---

## 6. Firmware Architecture

The firmware should be structured in layers rather than written as one large application file. A layered firmware architecture improves testing, portability, and readability.

## 6.1 Firmware layers

### A. Hardware abstraction layer (HAL)
Lowest firmware layer responsible for direct peripheral access.

Main responsibilities:

- configure GPIO and timing resources
- control scan outputs
- provide low-level driver interfaces
- isolate hardware-specific details from upper layers

### B. Display engine
Responsible for converting logical cube state into scan-ready output.

Main responsibilities:

- maintain cube frame representation
- map voxel data to physical output order
- sequence layers for multiplexed refresh
- manage brightness timing if brightness control is implemented

### C. Animation engine
Responsible for visual behavior at application level.

Main responsibilities:

- generate animation patterns and transitions
- update the cube model over time
- manage effect parameters, speed, and mode changes

### D. Communication / control interface
Responsible for receiving external commands or future expansion inputs.

Main responsibilities:

- parse control messages or settings
- expose mode selection or configuration hooks
- separate external control from display timing logic

### E. Diagnostics and system services
Responsible for internal monitoring and maintainability features.

Main responsibilities:

- startup self-checks
- test patterns for bring-up
- error handling or safe fallback states
- development diagnostics for hardware verification

---

## 6.2 Suggested runtime behavior

A practical runtime model is:

- **fast periodic task / interrupt context** for scan refresh
- **main control loop** for animation updates and communication handling
- **diagnostic/test mode** for bring-up and validation

This separation is important because display refresh timing is time-critical, while animation selection and user interaction are not.

---

## 7. Data Flow and Control Flow

## 7.1 Display data path

The logical display path is:

```text
Animation parameters / commands
            ->
Application state
            ->
3D frame buffer / voxel model
            ->
Layer extraction
            ->
Driver output pattern
            ->
Active LED layer
            ->
Visible cube image
```

## 7.2 Control flow summary

- External input selects or modifies operating mode
- Firmware updates the internal cube representation
- Refresh logic continuously scans the cube
- Driver hardware applies electrical switching to the active layer
- The viewer perceives a stable 3D animation

---

## 8. External Interfaces

The system architecture includes the following external interfaces at minimum:

### 8.1 Power interface
- external DC power input to the custom PCB

### 8.2 Programming and debug interface
- firmware upload and development access for the ESP32

### 8.3 User control interface
- external command/configuration path implemented through the ESP32 platform
- can be used for demo control, animation selection, or future application features

### 8.4 Mechanical/electrical cube interface
- interconnection between the PCB and the 8×8×8 LED structure

---

## 9. System States

At architecture level, the system can be modeled with the following operating states:

1. **Power-off** — no active logic or display output
2. **Initialization** — power-up, peripheral setup, optional self-test
3. **Idle / ready** — system initialized, waiting for or holding current mode
4. **Active display** — normal animation and refresh operation
5. **Diagnostic / test** — hardware verification patterns and troubleshooting support
6. **Fault / safe state** — optional fallback if a critical internal problem is detected

This state-based view is useful for firmware design and test planning.

---

## 10. Key Architectural Decisions

### Decision 1 — ESP32 as central controller
The ESP32 is selected as the main control device because it provides enough processing capability and flexible peripheral support for a custom LED cube controller while leaving room for future control features.

### Decision 2 — Custom single-board PCB
The project uses a custom PCB as the main system platform instead of loose module wiring. This improves electrical integration, reproducibility, and portfolio quality.

### Decision 3 — Multiplexed display architecture
A cube of 512 LEDs is implemented using scanning rather than full direct drive. This is the practical architecture for reducing pin count, routing complexity, and instantaneous driver requirements.

### Decision 4 — Layered firmware structure
Firmware is split into hardware, display, animation, and interface responsibilities to keep the codebase maintainable and testable.

### Decision 5 — Documentation-first engineering approach
The architecture is intentionally described in formal project documentation so that implementation can be traced to design intent and later validation evidence.

---

## 11. Architectural Risks and Sensitivities

The following items are the most architecture-sensitive parts of the project:

### 11.1 Refresh timing risk
If the scan rate is too low, visible flicker will occur. The firmware and driver design must therefore support a stable full-frame refresh rate.

### 11.2 Power integrity risk
Rapid switching of LED loads can inject noise into the system. Poor grounding, weak decoupling, or insufficient bulk capacitance may cause visual artifacts or controller instability.

### 11.3 Thermal and current-loading risk
Even with multiplexing, peak current paths can become significant. Driver devices, traces, and connectors must be dimensioned appropriately.

### 11.4 Mapping complexity risk
The logical voxel order and physical wiring order may not match naturally. A clear mapping layer in firmware is required to avoid difficult debugging later.

### 11.5 Maintainability risk
If scan logic, effect logic, and communication logic become tightly coupled, future changes will be harder. The layered firmware architecture is intended to reduce this risk.

---

## 12. Verification Implications

The architecture directly suggests the following verification activities:

- confirm stable power rails under dynamic LED load
- verify controller startup and programming access
- verify correct layer-by-layer scanning behavior
- verify full-cube addressing and voxel mapping
- verify flicker-free display under representative animations
- verify communication/control path if implemented in the current revision
- verify diagnostic modes and test patterns for bring-up support

These checks should later be linked to the validation documents in `docs/04_validation/`.

---

## 13. Traceability to Repository Structure

This architecture document belongs in:

```text
docs/00_overview/system-architecture.md
```

It should be read together with:

- `README.md` for project-level overview
- hardware design files in `hardware/`
- firmware notes and source material in `firmware/`
- validation evidence in `docs/04_validation/`

---

## 14. Conclusion

The 8×8×8 LED Cube is architected as a layered embedded system centered around a custom ESP32 control PCB. The core architectural idea is to separate user-facing behavior, firmware control, LED drive electronics, and power handling into clear subsystems. This makes the project easier to build, test, explain, and extend.

The selected architecture is appropriate for a portfolio-grade embedded project because it balances practical implementation constraints with clean engineering structure. It also creates a clear path from design decisions to hardware realization, firmware development, and final validation.
