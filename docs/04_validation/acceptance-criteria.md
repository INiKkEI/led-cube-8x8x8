# LED Cube Acceptance Criteria

## 1. Purpose
This document defines measurable pass/fail acceptance criteria for the Rev 1 build of the 8×8×8 LED Cube project.

The criteria are intended to be:
- directly verifiable during validation,
- realistic for the planned hardware and firmware build,
- traceable into the test cases already listed in `docs/04_validation/test-plan.md`.

---

## 2. Acceptance Criteria

### AC-01 — Safe power-up
**Requirement intent:** The system shall power up safely from the intended external supply.

**Acceptance criteria**
- The assembled board powers from a **5 V DC supply** with bench current limiting enabled.
- No smoke, overheating, visible damage, or immediate overcurrent trip occurs during first power-on.
- The measured board input voltage during the test is within **4.75 V to 5.25 V**.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-05, TP-06

---

### AC-02 — ESP32 boot reliability
**Requirement intent:** The ESP32 control subsystem shall boot reliably.

**Acceptance criteria**
- The firmware boots successfully in **5 consecutive power cycles**.
- After each boot, a defined firmware-alive indication is present, such as serial output, heartbeat pattern, or test indication.
- No manual rework, jumper change, or repeated reset-button intervention is required during those 5 cycles.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-10, TP-12, TP-30

---

### AC-03 — Programming interface operation
**Requirement intent:** Firmware shall be uploadable through the intended development interface.

**Acceptance criteria**
- Firmware upload completes successfully in **3 consecutive attempts** using the intended programming interface.
- After each upload, the device automatically or manually reboots into the application firmware.
- The uploaded firmware image is confirmed to run after flashing.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-11

---

### AC-4 — Layer switching correctness
**Requirement intent:** The display shall use correct multiplexed layer control.

**Acceptance criteria**
- In single-layer diagnostic mode, only the commanded layer is illuminated.
- This result is verified for **all 8 layers**.
- No unintended extra layer remains visibly active beyond minor non-blocking transient effects.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-13, TP-14, TP-19

---

### AC-05 — LED line driving correctness
**Requirement intent:** The driver stage shall activate intended LED lines only.

**Acceptance criteria**
- In a controlled line test, only the intended lines respond.
- The result is verified across a representative sample that covers **every driver output group** used by the cube.
- No severe cross-coupling or unintended illumination is observed that would prevent correct use of the display.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-15, TP-18

---

### AC-06 — Full voxel address mapping
**Requirement intent:** Logical voxel coordinates shall match physical LED positions.

**Acceptance criteria**
- A walking-voxel diagnostic test is executed across the full **8×8×8** address space.
- Every commanded voxel appears at the intended physical location.
- The mapping check completes with **0 blocking mapping errors**.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-16, TP-20

---

### AC-07 — Functional full-cube display
**Requirement intent:** The assembled cube shall operate as a complete 8×8×8 monochrome display.

**Acceptance criteria**
- All **8 layers** participate in scanning during full-cube operation.
- No permanent missing region, missing layer, or permanently dark section is present.
- Full-plane and checkerboard-style patterns render correctly and repeatably.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-17, TP-19

---

### AC-08 — Built-in animation capability
**Requirement intent:** Rev 1 firmware shall provide built-in animations stored locally on the controller.

**Acceptance criteria**
- At least **3 distinct built-in animations** are available without external storage.
- Each animation starts on command and runs without crash, freeze, or major visual corruption.
- The system runs the animation set continuously for **30 minutes** without reset or lockup.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-21, TP-22, TP-29

---

### AC-09 — BLE control link
**Requirement intent:** The cube shall support BLE-based smartphone control for basic user commands.

**Acceptance criteria**
- A smartphone establishes a BLE connection successfully in **3 consecutive attempts**.
- At minimum, one **mode-change** command and one **speed-change** command are received and applied correctly.
- For valid commands, visible cube behavior changes within **2 seconds** of transmission.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-25, TP-26

---

### AC-10 — Invalid command robustness
**Requirement intent:** The control interface shall fail safely on bad user input.

**Acceptance criteria**
- An invalid, malformed, or unsupported command can be sent through the control interface.
- The system does not crash, freeze, reboot unexpectedly, or require power cycling after that command.
- Normal valid control commands still work afterward.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-27, TP-28

---

### AC-11 — Diagnostic mode usefulness
**Requirement intent:** Firmware shall support bring-up and troubleshooting through explicit diagnostic modes.

**Acceptance criteria**
- A diagnostic mode can be entered intentionally.
- Diagnostic capability includes at least:
  - single-layer test,
  - single-line or equivalent addressing test,
  - walking-voxel test.
- Diagnostic mode remains stable for **5 minutes** without reset or freeze.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-13, TP-14, TP-15, TP-16

---

### AC-12 — Visually stable refresh
**Requirement intent:** Display refresh shall be stable enough for normal viewing and demonstration.

**Acceptance criteria**
- During observation from approximately **0.5 m to 1.0 m**, no obvious flicker is visible to the naked eye under normal viewing conditions.
- Sparse patterns and dense patterns both remain visually stable.
- Brightness non-uniformity does not create a major visual defect that makes the cube appear faulty.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-23, TP-24

---

### AC-13 — Runtime and thermal stability
**Requirement intent:** The cube shall remain stable during representative use.

**Acceptance criteria**
- The cube runs a representative animation or mode continuously for **30 minutes**.
- During that test there is **no reset, freeze, spontaneous reboot, supply collapse, or severe display corruption**.
- No regulator, driver, or other checked component reaches an obviously unsafe temperature during normal operation.

**Pass condition:** All three statements are true.

**Trace to test cases:** TP-29, TP-31, TP-32

---

## 3. Final Acceptance Rule
Rev 1 is accepted when:
- all acceptance criteria AC-01 through AC-14 pass,
- any remaining minor issues are documented and do not block safe demonstration,
- validation evidence is stored with the other project validation records.

If any acceptance criterion fails, Rev 1 is not yet fully accepted and the failure must be recorded in the validation results.

---

## 4. Traceability Summary

| Acceptance Criterion | Related Test Cases |
|---|---|
| AC-01 | TP-05, TP-06 |
| AC-02 | TP-09, TP-11, TP-29 |
| AC-03 | TP-10 |
| AC-04 | TP-12, TP-13, TP-18 |
| AC-05 | TP-14, TP-17 |
| AC-06 | TP-15, TP-19 |
| AC-07 | TP-16, TP-18 |
| AC-08 | TP-20, TP-21, TP-28 |
| AC-09 | TP-24, TP-25 |
| AC-10 | TP-26, TP-27 |
| AC-11 | TP-12, TP-13, TP-14, TP-15 |
| AC-12 | TP-22, TP-23 |
| AC-13 | TP-28, TP-30, TP-31 |
