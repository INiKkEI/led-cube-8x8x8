# Risk Register

This document tracks the main technical and project risks for the 8×8×8 LED cube project.

## Rating Method

- **Likelihood (L):** 1 = very unlikely, 5 = very likely
- **Impact (I):** 1 = very low impact, 5 = very high impact
- **Priority:** `L × I`

## Risk Register

| ID | Risk | Cause | L | I | Priority | Mitigation | Contingency | Status |
|---|---|---|---:|---:|---:|---|---|---|
| R-01 | LED current exceeds safe limits | Multiplexing, resistor choice, or drive timing may be incorrect | 3 | 5 | 15 | Calculate worst-case current early, simulate drive paths, verify resistor values before PCB finalization | Reduce brightness, increase resistor values, limit PWM duty cycle | Open |
| R-02 | Power supply instability or voltage drop | High instantaneous current during layer switching can cause dips and resets | 4 | 5 | 20 | Add bulk and decoupling capacitors, size power traces correctly, validate supply margin during design | Lower brightness, slow scan timing, use stronger regulated supply | Open |
| R-03 | PCB design error requires rework | Dense routing, wrong footprints, or incorrect net connections on custom board | 3 | 5 | 15 | ERC/DRC checks, footprint verification, schematic review, connector pinout review before ordering | Add bodge wires, cut/re-route traces, spin revised PCB if necessary | Open |
| R-04 | ESP32 pin count or timing becomes limiting | Cube control, layer selection, and future features may exceed practical I/O or timing margins | 2 | 4 | 8 | Freeze architecture early, reserve pins, keep signal map documented, prototype critical interfaces first | Add external drivers/shift registers or simplify feature scope | Open |
| R-05 | Firmware scan routine causes flicker | Refresh rate too low or timing not deterministic enough | 3 | 4 | 12 | Define refresh target early, keep scan loop lightweight, test on partial hardware first | Reduce animation complexity, optimize timing, move non-critical tasks out of scan path | Open |
| R-06 | Brightness is non-uniform across cube | LED tolerances, wiring differences, transistor variation, or timing mismatch | 3 | 3 | 9 | Use consistent components, keep wiring/layout symmetrical, test layer-by-layer during bring-up | Add software brightness compensation or replace worst outlier LEDs | Open |
| R-07 | Mechanical assembly misalignment | 8×8×8 structure is difficult to solder straight by hand | 4 | 3 | 12 | Use a jig/template, build and inspect one layer at a time, verify spacing before full assembly | Rework individual layers, remake mechanical frame section | Open |
| R-08 | Soldering defects create hard-to-find faults | Large number of joints increases chance of cold joints, shorts, or open circuits | 4 | 4 | 16 | Continuity-test subassemblies, inspect under magnification, bring up hardware in stages | Reflow suspect joints, isolate faulty layer/column and repair locally | Open |
| R-09 | Thermal stress on drivers or regulators | Repetitive switching and current peaks may overheat active components | 2 | 4 | 8 | Estimate dissipation, check datasheets, validate temperature during bench testing | Lower brightness/current, improve copper area, replace component with higher-rated part | Open |
| R-10 | Wireless/app control takes longer than expected | BLE communication and user interface work can expand scope | 3 | 3 | 9 | Keep MVP focused on core cube operation first, define control interface boundaries early | Deliver local/demo control first and postpone advanced app features | Open |
| R-11 | Documentation falls behind implementation | Hardware and firmware progress can outpace written records | 4 | 3 | 12 | Update docs after each milestone, keep screenshots/exports/version history, use templates | Reserve one cleanup pass before release to sync all documents | Open |
| R-12 | Final result looks unfinished for portfolio use | Working hardware alone may not be enough without validation evidence and presentation material | 3 | 4 | 12 | Capture photos, diagrams, test results, and decisions throughout the project | Add final validation summary, hero media, and lessons learned before publishing | Open |

## Highest-Priority Risks

The risks that currently deserve the most attention are:

1. **R-02 Power supply instability**
2. **R-08 Soldering defects**
3. **R-01 LED current limits**
4. **R-03 PCB design errors**

These should be reviewed at each major milestone:
- before PCB order
- before assembly
- during first power-up
- during full-cube testing

## Review Rule

This register should be updated whenever:
- a major hardware decision changes
- a PCB revision is made
- bring-up reveals a new failure mode
- a risk is closed, reduced, or replaced