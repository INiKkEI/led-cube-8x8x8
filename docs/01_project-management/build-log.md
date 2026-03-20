# Build Log

Working engineering log for the **8×8×8 LED Cube** project.

This file records only work that has actually been done. It is not a roadmap, milestone list, or task tracker.

---

## Session History

### 2026-03-16 — Build log cleanup and conversion to factual history

- **Session goal:** Convert `build-log.md` from a template/task-tracker style document into a factual engineering session log.
- **Work completed:** Build log file was added, then placeholder `[TBD]` milestone-style entries were removed. The example issue-tracking section was also removed so the file only supports real session logging.
- **Problems found:** The earlier version mixed future/projected milestones with actual history, which made the file read like a tracker instead of an engineering log.
- **Measurements / observations:** No hardware or firmware measurements recorded in this session. This was a documentation cleanup session.
- **Files or commits updated:** `docs/01_project-management/build-log.md` (`46267f0`, `c01a1e8`, `e51636d`).
- **Next step:** Add a new entry only after a real hardware, firmware, bring-up, or validation session is completed.

### 2026-03-17 — Revision 1 documentation baseline and repo consistency pass

- **Session goal:** Bring the LED Cube repository documentation to a consistent revision-1 baseline, remove architecture mismatches, and prepare the project docs for detailed design and implementation work.
- **Work completed:** Reviewed the repository for duplicated, outdated, and mismatched information; checked README and overview documents against the current project direction; defined revision-1 scope and explicit out-of-scope items; reworked and locked the main architecture decisions; corrected the hardware/firmware boundary so it matches the ESP32-based project direction; revised the system architecture overview and improved the architecture diagram approach; created or refined the main requirements/specification document; added measurable acceptance criteria for major requirements; linked major requirements to planned validation methods and test planning; reviewed the repository again for remaining inconsistencies; identified follow-up GitHub issues and milestone candidates for the next phase.
- **Problems found:** Several documents were not fully aligned with each other; some content still referenced the wrong control platform and needed correction to the ESP32-based baseline; some requirements were too broad or not directly testable; validation links were missing or unclear for parts of the requirements set; some repo information risked duplication across overview and management documents.
- **Measurements / observations:** No hardware measurements were taken in this session because the work was documentation-focused. Main observation is that the project documentation is now much closer to a usable engineering baseline: scope is clearer, architecture is more consistent, requirements are more testable, and validation planning is better connected to the requirement set.
- **Files or commits updated:** README.md; architecture decisions document; revision-1 scope content; hardware/firmware boundary document; system architecture document and diagram content; requirements/specification document; acceptance criteria content; validation/test-planning content; build-log/changelog guidance; issue and milestone planning notes. Replace this line later with the exact file paths and commit hashes you actually saved today.
- **Next step:** Run one final repo-wide consistency pass on the saved files, commit the documentation in logical chunks, create or update the related GitHub issues and milestone, and then move into detailed hardware and firmware planning for revision 1.

### 2026-03-20 — Electrical calculations rewrite and Altium schematic-prep pass

- **Session goal:** Rewrite the hardware calculations so they directly support schematic capture, and define the next concrete steps for starting the Altium project.
- **Work completed:** Reworked the electrical design calculations into a schematic-usable first-pass design note. Documented the blue LED assumptions, 15 mA peak drive target, per-line current limiting approach, switching-drop allowance, resistor sizing, resistor dissipation, active-layer current, and supply-side checks. Reviewed when exact components need to be selected in the design flow. Checked the role of the `hardware/` folder and prepared the project direction for adding real Altium source files. Defined the next setup steps for creating the Altium project.
- **Problems found:** The `hardware/` workspace still does not contain real schematic source files, so the repo is not yet carrying the actual Altium design artifacts. Exact low-side driver and high-side switch parts are still not locked, so some electrical values remain first-pass assumptions that must be recalculated after real part selection.
- **Measurements / observations:** First-pass resistor value is `100 Ω` per line for the current assumption set. Worst-case instantaneous display current for one active layer is `0.96 A`. Estimated dissipation per active resistor is about `22.5 mW`, so `0.125 W` or `0.25 W` resistors are electrically sufficient at this stage.
- **Files or commits updated:** `docs/02_hardware/design-calculations.md` (`e3ab6d6`, `139745d`, `836557b`, `7dd9ecd`, `7cf51eb`); supporting repo updates also touched `README.md` (`b4370c6`, `44c4c12`) and `docs/04_validation/test-plan.md` (`d28f4d3`).
- **Next step:** Create the actual Altium project inside `hardware/`, commit the real schematic source files, and replace first-pass switching-drop assumptions with values from the selected low-side driver and high-side switch components.

---

## Session Entry Template

Copy this block for the next real session and place it above older entries.

```md
### YYYY-MM-DD — Short session title

- **Session goal:** 
- **Work completed:** 
- **Problems found:** 
- **Measurements / observations:** 
- **Files or commits updated:** 
- **Next step:** 
