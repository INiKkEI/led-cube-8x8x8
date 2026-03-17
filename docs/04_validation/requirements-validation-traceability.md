# LED Cube Requirements-to-Validation Traceability

## 1. Purpose
This document connects the major Rev 1 requirements to the planned validation strategy.

Its purpose is to make later testing justified and traceable by showing:
- which requirement is being checked,
- which validation method will be used,
- which planned test cases provide evidence,
- and what result must be recorded.

This document should be used together with `docs/04_validation/test-plan.md`.

---

## 2. Usage Rules
- Every major requirement shall have at least one planned validation path.
- A requirement may use more than one validation method where needed.
- Test execution results shall record the related requirement ID as well as the test ID.
- If the requirement set is later renumbered, this matrix and the test plan references shall be updated together.

---

## 3. Validation Methods
The following validation methods are used in this matrix:

- **Inspection** — visual review of assembly, orientation, wiring, or documentation
- **Continuity / electrical check** — resistance, continuity, or static electrical checks before power-on
- **Measurement** — direct electrical measurement under power
- **Functional test** — deliberate command or stimulus with observed system response
- **Integration test** — combined subsystem behavior checked as one system
- **Runtime / stress test** — extended operation or higher-load operation to confirm stability
- **Document review** — review that required validation evidence has been recorded

---

## 4. Requirements-to-Validation Matrix

| Req ID | Major requirement | Validation method(s) | Planned validation path | Planned test cases | Evidence to record |
|---|---|---|---|---|---|
| REQ-01 | The project shall use one custom ESP32-based control PCB as the main hardware platform. | Inspection, continuity / electrical check, functional test | Inspect assembled PCB, check critical nets, then confirm powered operation through bring-up. | TP-01, TP-02, TP-03, TP-05 | PCB photos, continuity notes, first power-on result |
| REQ-02 | The system shall accept 5 V external input power and provide local 3.3 V regulation for the ESP32 and logic. | Measurement, runtime / stress test | Measure input and regulated rails, then observe stability under animation load. | TP-05, TP-06, TP-07, TP-08, TP-09 | Measured voltages, current notes, scope/photo evidence if used |
| REQ-03 | The ESP32 subsystem shall boot reliably, run firmware, and support firmware upload for development. | Functional test, runtime / stress test | Repeated boot, programming, and heartbeat tests. | TP-10, TP-11, TP-12, TP-30 | Boot log, flash success result, firmware revision used |
| REQ-04 | The design shall include a dedicated driver stage between the ESP32 and the cube for LED line driving and layer switching. | Inspection, functional test, integration test | Verify driver-related interconnects, then confirm controlled line and layer behavior on hardware. | TP-03, TP-14, TP-15, TP-17, TP-19 | Driver-stage inspection notes, pattern photos, observed anomalies |
| REQ-05 | The display architecture shall be multiplexed with one active layer at a time. | Functional test, integration test | Use deterministic single-layer tests and full scan tests to confirm correct multiplexed behavior. | TP-14, TP-19 | Layer test photos/video, notes on unintended layer activity |
| REQ-06 | The assembled system shall function as an 8×8×8 monochrome LED cube. | Functional test, integration test | Run deterministic patterns across the full cube and confirm all regions participate correctly. | TP-16, TP-17, TP-19, TP-20 | Pattern evidence, mapping notes, failure observations if any |
| REQ-07 | Firmware shall provide continuous refresh / scan control and correct display mapping. | Functional test, integration test | Use walking-voxel, full-cube scan, and mapping validation tests. | TP-16, TP-19, TP-20 | Mapping table or notes, photos/video of commanded vs observed output |
| REQ-08 | Firmware shall provide a basic built-in animation set stored in ESP32 internal flash only. | Functional test, runtime / stress test | Run baseline stored animations, change modes, and observe stability over time. | TP-21, TP-22, TP-29 | Animation demo evidence, firmware notes, runtime observations |
| REQ-09 | The system shall support BLE-based smartphone control for basic user commands. | Functional test, integration test | Establish the control link, send valid commands, test invalid commands, and verify reconnect behavior. | TP-25, TP-26, TP-27, TP-28 | Connection screenshots/logs, command list, observed response notes |
| REQ-10 | Firmware shall provide diagnostic / test-pattern modes to support bring-up and troubleshooting. | Functional test | Enter diagnostic mode and use deterministic line, layer, and voxel tests. | TP-13, TP-14, TP-15, TP-16 | Diagnostic mode evidence, test pattern photos, notes |
| REQ-11 | The hardware and firmware together shall provide visually stable refresh with no major visible flicker in normal use. | Functional test, integration test, runtime / stress test | Observe normal operation with sparse and dense patterns and compare brightness consistency. | TP-23, TP-24, TP-32 | Observation notes, video evidence if useful |
| REQ-12 | The full system shall remain stable during representative normal operation. | Runtime / stress test, measurement | Run extended operation and thermal spot checks during representative display activity. | TP-09, TP-29, TP-31, TP-32 | Runtime duration, thermal notes, reset/failure observations |
| REQ-13 | The build shall include a programming / debug interface for firmware upload and development diagnostics. | Inspection, functional test | Inspect interface presence and use it successfully for flashing and basic diagnostics. | TP-03, TP-11, TP-12 | Flash result, interface notes, any debug output |
| REQ-14 | Validation results shall be recorded in a traceable way so the final build claim is justified. | Document review | Confirm that each executed test stores pass/fail result, measured values, notes, and evidence. | Test-plan execution records for TP-01 to TP-32 | `test-results.md`, bring-up log, photos, videos, scope captures |

---

## 5. Coverage Check
The matrix above is complete only if all of the following remain true:

- each major requirement has at least one validation method;
- each major requirement has at least one linked planned test case, except document-control requirements that are checked by review;
- no planned critical test exists without supporting at least one requirement;
- final test records include both requirement IDs and test IDs.

If a new major requirement is added later, this document must be updated before the requirement is considered covered.

---

## 6. Required Cross-Reference in the Test Plan
To keep the validation path explicit, add the following reference under the **References** section of `docs/04_validation/test-plan.md`:

- `docs/04_validation/requirements-validation-traceability.md`
- the project requirements document for Rev 1, once its final filename is fixed

Recommended sentence for Section 1 or Section 4 of the test plan:

> Planned test cases in this document are traced to the major Rev 1 requirements through `docs/04_validation/requirements-validation-traceability.md`.

---

## 7. Suggested Result Logging Format
When recording executed tests later, include both the test ID and the covered requirement IDs.

Example:

| Test ID | Covered requirements | Result | Evidence |
|---|---|---|---|
| TP-14 | REQ-04, REQ-05, REQ-10 | Pass | `photos/tp-14-layer-test.jpg` |
| TP-26 | REQ-09 | Pass | `videos/tp-26-command-response.mp4` |
| TP-29 | REQ-08, REQ-12 | Pass | `logs/runtime-01.md` |

---

## 8. Conclusion
This document provides the missing link between the requirement set and the validation work. It ensures that later testing is not just a list of checks, but a justified path showing how each major Rev 1 requirement will be proven.
