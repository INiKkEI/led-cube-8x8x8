# Hardware–Firmware Boundary

## 1. Purpose

This document defines the responsibility boundary between **hardware** and **firmware** for the 8×8×8 LED Cube project.

Its purpose is to:

- prevent overlap between circuit design and embedded software work
- make implementation ownership clear
- provide a stable reference for architecture, bring-up, and validation documents
- keep later hardware and firmware documents consistent with the same subsystem split

---

## 2. Scope

This boundary applies to the embedded LED cube system built around the custom ESP32 control PCB.

Included in scope:

- power input and on-board power distribution
- ESP32 control hardware
- LED driver and switching circuitry
- cube electrical interconnect
- firmware running on the ESP32

Outside this boundary:

- smartphone application or control UI
- external 5 V power adapter
- PC tools used only for firmware upload, serial debug, or development testing

---

## 3. Baseline assumptions

This document follows the current project baseline:

- the main controller is an **ESP32**
- the project uses a **custom single-board control PCB**
- user control is primarily through **BLE**
- BLE is provided by the **ESP32 platform**
- animations are stored in **ESP32 internal flash**
- there is **no external memory** in revision 1
- the system uses **5 V main input**
- the external adapter target is **5 V / 5 A minimum**

---

## 4. Boundary principle

The boundary is based on one simple rule:

- **Hardware owns electrical implementation**
- **Firmware owns runtime behavior**

In practice:

- if something is about **power, voltages, current paths, drivers, signal conditioning, protection, PCB connectivity, or cube wiring**, it belongs to **hardware**
- if something is about **timing, scanning, animation logic, state handling, command processing, or diagnostics behavior**, it belongs to **firmware**

---

## 5. Hardware responsibilities

Hardware is responsible for making the system electrically correct, physically realizable, and safe for the ESP32 to control.

### 5.1 Power architecture

Hardware shall:

- accept the external **5 V DC** input
- distribute power to LED-driving circuitry and support electronics
- provide bulk capacitance and local decoupling where needed
- define grounding and current return paths
- size traces, connectors, regulators, and switching components for expected load
- include any selected input protection or reverse-polarity protection

### 5.2 ESP32 hardware integration

Hardware shall:

- integrate the ESP32 onto the control PCB
- provide all required support circuitry for stable ESP32 operation
- provide boot, reset, programming, and debug access as needed
- ensure all signals connected to the ESP32 stay within valid electrical limits
- define default pin states at power-up and reset where relevant

### 5.3 LED driver and switching circuitry

Hardware shall:

- implement the electrical driver stage between the ESP32 and the LED cube
- provide line-driving and layer-switching circuitry
- provide current-limiting strategy required by the chosen design
- keep high-current LED switching away from direct ESP32 loading
- define the real signal polarity seen by firmware
- ensure components operate within voltage, current, and thermal limits

### 5.4 Cube interconnect and physical mapping

Hardware shall:

- define the physical wiring of the 8×8×8 cube
- define how the cube connects to the PCB
- define the final net-to-pin mapping
- provide connectors, headers, and test access where practical
- manage layout issues such as routing density, noise, and high-current return paths

### 5.5 Safe electrical default behavior

Hardware shall, as far as practical:

- keep outputs in a non-destructive state during power-up and reset
- avoid floating control lines that could unintentionally light LEDs
- avoid obvious overcurrent conditions by design
- provide an electrically stable platform before firmware begins normal scanning

---

## 6. Firmware responsibilities

Firmware is responsible for system behavior after the hardware is powered and electrically ready.

### 6.1 Startup and initialization

Firmware shall:

- initialize GPIO, timers, communication interfaces, and any required peripherals
- place the cube into a known safe logical state during startup
- perform the startup sequence required before normal refresh begins
- initialize BLE-related software functionality on the ESP32 side

### 6.2 Display scanning and timing

Firmware shall:

- generate the multiplexing sequence
- control layer-by-layer refresh timing
- generate any PWM or time-based brightness control
- manage blanking or update order required for stable display behavior
- keep refresh fast enough to avoid visible flicker
- respect hardware timing limits documented by the hardware design

### 6.3 Animation and frame control

Firmware shall:

- store animations and static patterns in ESP32 internal flash
- maintain frame data within available RAM
- map logical voxel coordinates to physical hardware outputs
- manage animation playback, transitions, and mode changes
- separate visual logic from low-level scan behavior

### 6.4 BLE communication and command handling

Firmware shall:

- implement the embedded side of the BLE control path
- receive and parse commands coming from the external controller side
- validate commands before applying them
- update display behavior or configuration based on valid input
- handle invalid data, disconnects, and timeout conditions

### 6.5 Diagnostics and operating behavior

Firmware shall:

- define operating modes, test modes, and startup behavior
- provide test patterns useful for bring-up and debugging
- define fallback behavior after reset or communication loss
- provide serial or other development diagnostics where needed
- stay within ESP32 timing, memory, and runtime limits

---

## 7. Explicit exclusions

### 7.1 Not firmware

The following do **not** belong to firmware:

- power regulation design
- current limiting by hardware design
- transistor, MOSFET, or driver selection
- trace width sizing
- connector definition
- voltage compatibility
- input protection circuitry
- physical cube wiring

Firmware may only use those interfaces as documented by hardware.

### 7.2 Not hardware

The following do **not** belong to hardware:

- animation logic
- scan scheduling
- brightness algorithms
- mode switching logic
- frame generation
- BLE command parsing
- timeout handling
- system-state behavior
- diagnostic pattern sequencing

Hardware provides the capability; firmware defines how that capability is used.

---

## 8. Interface assumptions

These assumptions define the handoff between hardware and firmware.

### 8.1 Power interface assumptions

- The board receives **5 V main input**
- Firmware assumes valid rails are present and stable before normal operation
- Firmware does not manage voltage conversion itself

### 8.2 ESP32 interface assumptions

- Hardware must document the actual ESP32 pin usage
- Hardware must document default pin states, active polarities, and any reserved pins
- Firmware must use only the documented pin mapping
- Firmware must not assume undocumented inversions or undocumented pull states

### 8.3 Driver interface assumptions

- Hardware exposes a defined digital control interface from the ESP32 to the LED driver stage
- Hardware must document:
  - control signal names
  - signal polarity
  - mapped ESP32 pins
  - safe default states
  - any timing restrictions
- Firmware must operate only within that documented interface

### 8.4 Timing assumptions

- Hardware must document electrical constraints such as:
  - minimum pulse widths
  - required setup/hold relationships if relevant
  - required dead time or blanking if relevant
- Firmware is responsible for respecting those constraints during refresh

### 8.5 Brightness and current assumptions

- Hardware defines the safe electrical current limits
- Firmware controls only effective visual brightness through timing and duty behavior
- Firmware must not intentionally command unsafe simultaneous drive states

### 8.6 BLE assumptions

- BLE is part of the ESP32-based control platform
- Hardware is responsible only for making the ESP32 platform electrically functional
- Firmware is responsible for BLE software behavior, command handling, and system response
- The smartphone app is external and is not part of embedded firmware scope

### 8.7 Memory assumptions

- No external memory is present in revision 1
- Firmware must fit code, tables, and animation content into ESP32 internal memory resources
- Hardware does not provide memory expansion in this revision

### 8.8 Feedback assumptions

- Unless dedicated sensing hardware is added later, firmware should assume there is no direct hardware feedback for LED current, temperature, or fault status
- If future revisions add sensing or fault lines, hardware must document them before firmware depends on them

---

## 9. Responsibility split by subsystem

| Subsystem | Hardware responsibility | Firmware responsibility |
|---|---|---|
| Power input and regulation | 5 V input handling, regulation, filtering, decoupling, distribution | safe startup behavior after valid power is present |
| ESP32 control platform | ESP32 integration, support circuitry, boot/reset/programming access | peripheral initialization, runtime control, system logic |
| BLE control path | no separate BLE hardware beyond ESP32 platform support unless later added | BLE software stack usage, command reception, parsing, and handling |
| LED driver stage | driver topology, switching devices, current path design, signal conditioning | scan timing, output sequencing, brightness control |
| LED cube matrix | physical wiring, connector mapping, electrical interconnect | logical voxel-to-output mapping |
| Diagnostics support | test points and physical debug access | test patterns, debug output, diagnostic behavior |

---

## 10. Rules for later documents

Later documents shall use this boundary consistently:

- **system architecture** documents should use this split when assigning subsystem roles
- **hardware documents** should describe circuits, rails, interfaces, mappings, and protection
- **firmware documents** should describe timing, state logic, data structures, control flow, and BLE handling
- **bring-up documents** should check power rails, pin states, and driver hardware before testing scan firmware
- **validation documents** should clearly separate electrical verification from behavior verification

---

## 11. Summary

For this project:

- **Hardware** owns the electrical, physical, and protection-related implementation
- **Firmware** owns the runtime behavior, display timing, animation logic, BLE handling, and diagnostics behavior

This document is the default reference for implementation ownership unless a later architecture revision explicitly changes the boundary.
