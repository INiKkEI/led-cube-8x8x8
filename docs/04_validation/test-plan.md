# LED Cube Test Plan

## 1. Purpose

This document defines how the 8×8×8 LED Cube project will be verified during bring-up, integration, and final validation. It describes what must be tested, how the tests will be performed, what equipment is needed, and what counts as a pass.

The goal of this plan is to make validation repeatable, traceable, and easy to present in the repository.

## 2. Scope

This test plan covers the first full hardware and firmware revision of the project:

- monochrome **8×8×8 LED cube**
- **custom ESP32-based control PCB**
- multiplexed LED driving architecture
- power input, regulation, and distribution
- firmware for scanning, animation playback, diagnostics, and control
- external user control interface implemented on the ESP32 platform

This plan covers bench testing, bring-up testing, subsystem testing, integration testing, and final functional validation.

It does **not** cover long-term product qualification, formal EMC certification, or production-line testing.

## 3. Test Objectives

The validation work shall confirm that:

1. the hardware can be powered safely without damage or unstable behavior;
2. the ESP32 subsystem boots, programs, and runs firmware correctly;
3. the LED driver stage can address the cube correctly without destructive current paths;
4. the cube displays the intended voxel patterns and animations correctly;
5. refresh behavior is visually stable and free of obvious flicker in normal operation;
6. the system remains electrically stable under dynamic LED load;
7. the BLE user-control path works as intended for revision 1;
8. diagnostic and test modes are sufficient to support troubleshooting and demonstration.

## 4. References

This plan should be used together with:

- `README.md`
- `docs/00_overview/architecture-decisions.md`
- `docs/00_overview/requirements-specification.md`
- `docs/00_overview/system-architecture.md`
- `docs/00_overview/hardware-firmware-boundary.md`
- hardware schematic, PCB, and manufacturing outputs in `hardware/`
- firmware source and notes in `firmware/`
- future test results, logs, and evidence in `docs/04_validation/`

## 5. Test Strategy

Validation will be done in four layers.

### 5.1 Inspection and pre-power checks

These tests are done before first power-on. They confirm:

- PCB assembly quality
- orientation of critical parts
- absence of obvious shorts
- connector and polarity correctness
- continuity of important power and signal paths

### 5.2 Bring-up tests

These are the first powered tests on the assembled hardware. They confirm:

- correct input voltage behavior
- correct regulator outputs
- ESP32 startup and programming access
- safe current draw during idle conditions

### 5.3 Subsystem tests

Each major subsystem is checked separately as far as practical:

- power subsystem
- ESP32 control subsystem
- LED driver subsystem
- cube interconnect / layer addressing
- firmware diagnostics and display engine
- communication / external control path

### 5.4 Integration and system tests

After subsystem checks pass, the full cube is tested as one system. These tests confirm:

- correct layer scanning
- correct voxel mapping
- animation playback
- brightness consistency
- acceptable thermal and electrical behavior during runtime

## 6. Test Environment and Equipment

The following equipment is expected for the planned validation work:

- bench DC power supply with current limiting
- digital multimeter
- oscilloscope
- USB cable / programming interface for ESP32
- test firmware with dedicated diagnostic modes
- smartphone or host device for the selected BLE client
- visual recording device for evidence collection
- optional thermal check method (IR thermometer or thermal camera if available)

## 7. Entry and Exit Criteria

### 7.1 Entry criteria

Testing can begin when:

- schematic and PCB revision are frozen for the tested build;
- assembly is complete enough for the selected test stage;
- firmware build used for testing is identified;
- required tools and measurement equipment are available.

### 7.2 Exit criteria

The test campaign is considered complete when:

- all planned critical tests have been executed;
- all blocking failures have been fixed or explicitly documented;
- final pass/fail status is recorded for each test item;
- evidence is stored in the validation folder;
- remaining limitations are clearly listed.

## 8. Pass / Fail Rules

A test **passes** when the observed behavior matches the expected result and no safety-related or function-blocking issue appears.

A test **fails** when:

- the expected function does not occur;
- the result is unstable or not repeatable;
- the behavior risks damaging hardware;
- the measured value is outside the defined acceptable range;
- the issue blocks later integration or demonstration.

If a test reveals a problem but the project can still proceed, the issue should be logged as an open limitation or non-blocking defect.

## 9. Defect Severity

For simple project tracking, defects may be classified as:

- **Critical** — risk of hardware damage, unsafe operation, or complete system failure
- **Major** — key feature does not work or results are unreliable
- **Minor** — feature works but with a limited issue that does not block progress
- **Cosmetic** — presentation or documentation issue only

## 10. Planned Test Cases

### 10.1 Hardware inspection and pre-power tests

| ID | Test name | Method | Expected result | Pass criteria |
|---|---|---|---|---|
| TP-01 | Visual PCB inspection | Inspect assembly, solder joints, polarity, orientation | No obvious assembly faults | No visible shorts, missing parts, reversed polarized parts, or damaged pads |
| TP-02 | Power rail short check | Measure resistance between power and ground before power-on | No hard short present | Resistance is not near 0 Ω and does not indicate direct short |
| TP-03 | Continuity of critical nets | Continuity check on main supply, regulator outputs, ground, programming lines, key driver nets | Intended connections are present | Selected nets match schematic intent |
| TP-04 | Cube wiring / interconnect inspection | Check layer and column interconnects against design | No swapped or broken cube connections | No obvious wiring mismatch found |

### 10.2 Power subsystem tests

| ID | Test name | Method | Expected result | Pass criteria |
|---|---|---|---|---|
| TP-05 | Current-limited first power-on | Apply supply with bench current limit | Board powers without abnormal current surge | No smoke, overheating, or immediate overcurrent trip |
| TP-06 | Input voltage verification | Measure system input node under power | Correct input voltage reaches board | Measured input is within intended supply range |
| TP-07 | Idle current measurement | Measure supply current after boot in idle/test-safe state | Current draw is plausible and repeatable | Idle current remains stable and within expected engineering estimate |
| TP-08 | Dynamic load stability | Observe supply rail during animation / scanning | No unacceptable droop or reset behavior | No controller resets or obvious display corruption under normal patterns |

### 10.3 ESP32 and firmware bring-up tests

| ID | Test name | Method | Expected result | Pass criteria |
|---|---|---|---|---|
| TP-09 | ESP32 boot test | Power board and observe boot behavior | Controller starts successfully | Firmware starts reliably after repeated power cycles |
| TP-10 | Programming interface test | Flash firmware from development environment | Firmware upload works | Successful flash and reboot confirmed |
| TP-11 | Basic firmware heartbeat | Run minimal firmware with serial/log or test indication | Firmware main loop is alive | Stable repeated indication is observed |
| TP-12 | Diagnostic mode entry | Trigger dedicated diagnostic/test mode | Diagnostic mode can be entered intentionally | Test mode activates on command and remains stable |

### 10.4 LED driver and addressing tests

| ID | Test name | Method | Expected result | Pass criteria |
|---|---|---|---|---|
| TP-13 | Single-layer activation test | Enable one layer at a time with simple pattern | Only intended layer activates | No unintended layers illuminate |
| TP-14 | Single-column / line test | Drive selected LED lines in controlled pattern | Only intended lines respond | No cross-coupling beyond acceptable ghosting level |
| TP-15 | Walking voxel test | Step one voxel through the full cube address space | Address mapping is correct | Every commanded voxel appears at intended physical location |
| TP-16 | Full-layer pattern test | Show full-plane and checkerboard patterns | Driver stage handles dense patterns | Pattern appears correct and repeatable |
| TP-17 | Ghosting observation | Display sparse and dense patterns and inspect unwanted light | Ghosting remains acceptable | No severe unintended illumination that breaks usability |

### 10.5 Integrated cube functional tests

| ID | Test name | Method | Expected result | Pass criteria |
|---|---|---|---|---|
| TP-18 | Full cube scan test | Run full refresh across all layers | Entire cube refreshes correctly | All layers participate in display without missing sections |
| TP-19 | Voxel mapping validation | Compare commanded coordinates with observed location | Logical-to-physical mapping is correct | Mapping is consistent across full cube |
| TP-20 | Animation playback test | Run baseline animation set | Animations render correctly | Stored animations run without crash, freeze, or major visual corruption |
| TP-21 | Mode switching test | Change between animation modes / states | System changes modes correctly | Requested mode changes are applied consistently |
| TP-22 | Flicker evaluation | Observe display directly and through camera if useful | Refresh is visually stable | No obvious flicker during normal viewing conditions |
| TP-23 | Brightness consistency check | Compare brightness across cube regions and layers | No major brightness imbalance | Variations stay acceptable for portfolio demonstration |

### 10.6 Communication and control tests

| ID | Test name | Method | Expected result | Pass criteria |
|---|---|---|---|---|
| TP-24 | BLE link test | Connect smartphone or BLE client to the ESP32 control interface | Link initializes correctly | Device connects reliably |
| TP-25 | Command reception test | Send valid commands such as mode change or speed setting | Firmware receives and applies commands | Requested change appears correctly on cube |
| TP-26 | Invalid command handling | Send malformed or unsupported command input | System remains stable | No crash, lockup, or unsafe behavior occurs |
| TP-27 | Reconnect behavior | Disconnect and reconnect controller/client | System recovers correctly | Reconnection works without requiring full power cycle unless intentionally designed |

### 10.7 Reliability and runtime tests

| ID | Test name | Method | Expected result | Pass criteria |
|---|---|---|---|---|
| TP-28 | Extended runtime test | Run representative animation continuously for an extended period | Stable operation over time | No reset, freeze, overheating, or visible degradation during test window |
| TP-29 | Repeated power-cycle test | Power cycle system multiple times | Startup remains consistent | System starts correctly on repeated cycles |
| TP-30 | Thermal spot check | Check temperature of regulators, drivers, and critical areas | Temperatures remain reasonable | No part reaches obviously unsafe temperature in normal operation |
| TP-31 | Max-normal-load visual test | Run dense / bright test pattern within intended normal usage | System remains stable | No supply collapse, reset, or severe artifacting |

## 11. Requirement-to-Validation Traceability

| Requirement ID | Summary | Validation path |
|---|---|---|
| FR-01 | 8×8×8 monochrome cube | TP-04, TP-15, TP-18, visual inspection |
| FR-02 | One custom ESP32-based main PCB | TP-01, hardware inspection |
| FR-03 | Dedicated driver stage | TP-01, TP-13, TP-14, TP-16 |
| FR-04 | One active layer at a time | TP-13, TP-18 |
| FR-05 | 5 V input | TP-05, TP-06 |
| FR-06 | Internal flash only for revision-1 storage | firmware inspection, TP-20 |
| FR-07 | Initialization, scan, mapping, animation, diagnostics | TP-09, TP-11, TP-12, TP-15, TP-20 |
| FR-08 | BLE smartphone control | TP-24, TP-25, TP-27 |
| FR-09 | Programming/debug interface | TP-10, TP-11 |
| FR-10 | Voxel-addressable mapping | TP-15, TP-19 |
| FR-11 | Bring-up independent of final BLE path | TP-05 through TP-12 |
| FR-12 | Validation evidence produced | test records stored in `docs/04_validation/` |
| NFR-01 | 5 V / 5 A planning baseline | power-budget analysis, architecture/requirements inspection |
| NFR-02 | Electrical stability during normal use | TP-08, TP-28, TP-31 |
| NFR-03 | Visual stability / no obvious flicker | TP-22 |
| NFR-04 | Safe operating margins over extreme brightness | design calculations, TP-30, engineering review |
| NFR-05 | Core electronics cost about €100 or less | BOM analysis |
| NFR-06 | Scan path separated from higher-level logic | firmware architecture review |
| NFR-07 | Every major requirement has a validation path | this traceability matrix |

## 12. Evidence Storage Rule

When tests are executed, the following should be recorded where practical:

- test ID
- date
- hardware revision
- firmware revision or commit
- measured values
- pass/fail result
- notes on failures or limitations
- photos, screenshots, or oscilloscope captures when useful

This evidence should be stored in `docs/04_validation/` or a clearly linked subfolder.
