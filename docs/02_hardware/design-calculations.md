# Electrical Design Calculations — 8×8×8 LED Cube

## 1. Purpose

This document captures the main first-pass electrical calculations for the revision-1 LED cube hardware.
It covers:

- LED current assumptions
- current-limiting resistor sizing
- multiplexing current implications
- layer-switch current requirements
- resistor and power dissipation estimates
- supply sizing checks

This is an **assumption-based design note** for the current repository stage. Final values must be rechecked after the actual driver parts and schematic are locked.

---

## 2. Fixed Assumptions Used Here

### 2.1 User-selected assumptions

- LED color: **blue**
- LED forward voltage assumption: **Vf = 3.2 V**
- Peak current per active LED during multiplexing: **15 mA**
- Current limiting method: **one resistor per line**
- Main input supply baseline: **5 V / 5 A**

### 2.2 Architecture assumptions already fixed in the repository

- Display type: **8×8×8 monochrome cube**
- Refresh method: **one active layer at a time**
- Driver topology: **high-side layer switching + low-side line sinking**
- Maximum active LEDs in one scan slot: **64**
- Total layers: **8**
- LED duty cycle at full-cube “all on”: **1/8 per LED**

### 2.3 First-pass switching-drop assumption

The exact low-side driver and high-side switch parts have **not** been selected yet, so resistor sizing cannot be tied to a real BOM value yet.
For first-pass calculations, reserve:

- high-side path drop allowance: **0.1 V**
- low-side path drop allowance: **0.2 V**
- total switching-path allowance: **0.3 V**

This is only a planning assumption. Recalculate once the actual devices are chosen.

---

## 3. Multiplexing Current Implications

### 3.1 Instantaneous active-layer current

Only one layer is active at a time, but that layer may contain up to **64 LEDs on simultaneously**.

Worst-case instantaneous display current:

$
I_{display,inst,max} = 64 \times 15\,mA = 960\,mA = 0.96\,A
$

**Result:** the active layer can demand up to **0.96 A** from the 5 V display path.

### 3.2 Average current per LED

Because the cube is scanned across 8 layers, each LED has only **1/8 duty cycle** in the all-on case.

$
I_{LED,avg} = \frac{15\,mA}{8} = 1.875\,mA
$

**Result:** each individual LED sees about **1.875 mA average current** when the whole cube is displaying a full-on image.

---

## 4. Current-Limiting Resistor Calculation

### 4.1 Resistor equation

With per-line current limiting, each active line resistor is sized from:

$
R = \frac{V_{supply} - V_f - V_{switch}}{I_{LED}}
$

Using the current assumptions:

- $\(V_{supply} = 5.0\,V\)$
- $\(V_f = 3.2\,V\)$
- $\(V_{switch} = 0.3\,V\)$
- $\(I_{LED} = 15\,mA\)$

$
R = \frac{5.0 - 3.2 - 0.3}{0.015} = \frac{1.5}{0.015} = 100\,\Omega
$

**Recommended first-pass resistor value: `100 Ω` per line**.

### 4.2 Sanity check for actual LED current with 100 Ω

If total driver-path drop is close to the assumed 0.3 V:

$
I = \frac{5.0 - 3.2 - 0.3}{100} = 15\,mA
$

If the real switching path is a little different, the current shifts accordingly:

- with **0.2 V** switch-path drop: about **16 mA**
- with **0.4 V** switch-path drop: about **14 mA**
- with **0.5 V** switch-path drop: about **13 mA**

So **100 Ω** is a reasonable, conservative first-pass choice until real driver drops are known.

---

## 5. Resistor Power Dissipation

### 5.1 Power in one resistor when its line is active

With a 100 Ω resistor at 15 mA:

$
P_{R,line} = I^2R = (0.015)^2 \times 100 = 0.0225\,W
$

**Result:** one active line resistor dissipates about **22.5 mW**.

### 5.2 Worst-case total resistor dissipation

In an all-on pattern, all 64 line channels can be active during each scan slot.
Total resistor power is then approximately:

$
P_{R,total} = 64 \times 0.0225\,W = 1.44\,W
$

**Result:** the resistor network can dissipate about **1.44 W total** in the worst sustained display case.

### 5.3 Practical sizing implication

Per resistor, the electrical load is small, so **0.125 W or 0.25 W** parts are electrically sufficient.
For assembly margin and thermal comfort, a practical first-pass choice is:

- **100 Ω**
- **±1% or ±5%**
- **at least 0.125 W rating**

The total heat is spread across many resistors, so no single resistor is heavily stressed.

---

## 6. Layer-Switch Sizing

### 6.1 Required current per layer switch

Each layer switch must carry the full active-layer current.

$
I_{layer,max} = 64 \times 15\,mA = 0.96\,A
$

**Result:** each high-side layer switch must safely carry **about 1 A instantaneous current**.

### 6.2 Design margin recommendation

The switch should not be selected exactly at the calculated limit.
A good first-pass requirement for each layer switch is:

- **continuous current capability of at least 1.5 A**
- preferably **2 A class or higher** for comfortable margin
- low conduction loss at the real gate-drive condition

This gives margin for:

- LED forward-voltage spread
- timing errors during bring-up
- brief transient overlap
- component tolerances

### 6.3 High-side gate-drive assumption

If the high-side stage uses **P-channel MOSFETs on a 5 V rail**, the gate drive must be checked carefully.
A 3.3 V ESP32 GPIO should **not** be assumed to directly provide a valid 5 V-referenced high-side gate drive for a P-channel MOSFET in all states.

First-pass design implication:

- assume the high-side stage will need a **proper gate-drive / level-shift arrangement**
- do **not** assume “ESP32 GPIO directly to P-MOS gate” is automatically acceptable

This must be resolved explicitly in the schematic.

---

## 7. Low-Side Driver Channel Requirements

Each low-side sink channel only carries the current of one active LED position for the currently enabled layer.
So first-pass requirement per low-side channel is:

$
I_{line,max} = 15\,mA
$

However, the chosen line driver still needs margin.
A practical minimum per channel target is:

- **15 mA nominal sink current**
- preferably **20–30 mA channel capability** for margin

The complete low-side bank must support up to:

$
I_{sink,total,max} = 64 \times 15\,mA = 0.96\,A
$

So the low-side architecture must support roughly **1 A aggregate sink current** across the active line set.

---

## 8. 5 V Supply and Input Current Check

### 8.1 Display-only worst-case current

From the multiplexing calculation:

$
I_{display,max} = 0.96\,A
$

### 8.2 Add control-electronics budget

For first-pass budgeting, keep the same conservative planning allowance already used elsewhere in the repo:

- ESP32 and local support electronics: **0.25 A**
- extra driver/control overhead: **0.05 A**

Then:

$
I_{total,est} = 0.96 + 0.25 + 0.05 = 1.26\,A
$

### 8.3 Input power

$
P_{total,est} = 5.0\,V \times 1.26\,A = 6.3\,W
$

**Result:** with the 15 mA peak-LED assumption, the first-pass system estimate is about:

- **1.26 A total input current**
- **6.3 W total input power**

### 8.4 Supply sizing conclusion

The selected **5 V / 5 A** adapter remains comfortably oversized relative to the present estimate.
That is acceptable and useful because it provides margin for:

- switching transients
- brightness tuning later
- part-to-part variation
- early bring-up mistakes and rework

---

## 9. PCB and Connector Sizing Implications

First-pass electrical implications for layout and interconnects:

### 9.1 Main 5 V distribution

Because the display path can approach **1 A**, the main 5 V and return distribution should be treated as a **power path**, not a logic trace.
A reasonable first-pass target is to size the main display-current paths for at least:

- **2 A continuous board capability**

This gives useful margin above the current estimate.

### 9.2 Bulk capacitance

Because the display current is switched layer-by-layer, local bulk capacitance is needed near the display driver stage.
Exact capacitance will depend on the chosen driver devices and measured transients, but the schematic should include:

- local ceramic decoupling at logic and driver ICs
- bulk capacitance near the display power switching section

### 9.3 Connector requirement

Any main power connector and wiring should comfortably support more than the calculated 1.26 A estimate.
A practical first-pass requirement is:

- connector and wiring comfortably rated for **2 A or more**

---

## 10. Result Summary

| Item | First-pass value |
|---|---:|
| Supply voltage | 5.0 V |
| LED color | Blue |
| LED forward voltage assumption | 3.2 V |
| Peak current per active LED | 15 mA |
| Active layers at once | 1 |
| Max active LEDs per scan slot | 64 |
| LED duty cycle at full-cube all-on | 1/8 |
| Worst-case active-layer current | 0.96 A |
| Current-limiting method | 1 resistor per line |
| Calculated resistor value | 100 Ω |
| Number of line resistors | 64 |
| Power in one active resistor | 22.5 mW |
| Total resistor dissipation, worst case | 1.44 W |
| Recommended minimum layer-switch current class | 1.5 A minimum, 2 A preferred |
| Estimated total system input current | 1.26 A |
| Estimated total input power | 6.3 W |
| External supply baseline | 5 V / 5 A |

---

## 11. Open Items That Must Be Rechecked Later

This calculation note is **not yet schematic-final** because these implementation details are still unspecified:

1. exact low-side sink device
2. exact high-side switching device
3. exact switch-path voltage drops
4. exact ESP32 module/input-current behavior
5. exact connector family and PCB copper/layout details

Once the schematic and BOM exist, recheck at least:

- resistor value against actual driver drops
- layer-switch dissipation
- low-side driver current margin
- total 5 V input current
- decoupling and bulk-capacitance adequacy

---

## 12. Final Conclusion

Under the current first-pass assumptions, the most important electrical result is that the cube should be designed around **about 1 A display current per active layer**, not around the average current of a single LED.

With:

- **blue LEDs**
- **3.2 V forward voltage assumption**
- **15 mA peak LED current**
- **per-line current limiting**
- **5 V supply**

A consistent first-pass design point is:

- **100 Ω line resistors**
- **64 resistor channels**
- **high-side layer switches sized for about 1 A minimum working current, with margin**
- **overall input-current planning around 1.26 A**
- **5 V / 5 A external supply kept as the baseline**

This is suitable as a repository-stage design-calculations document until real driver parts and the schematic are locked.
