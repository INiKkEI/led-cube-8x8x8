# Design Calculations — 8×8×8 LED Cube

## 1. Purpose

This document converts the locked display assumptions into concrete schematic-level numbers so the driver stage and its support parts can be drawn without leaving major electrical gaps.

The goal of this file is to answer:

- how many drive channels are required
- what resistor value should be used on each LED line
- what current and voltage ratings the line and layer drivers must meet
- what support parts are required around the switching stage
- what minimum decoupling and bulk capacitance should be placed in the schematic

This is a first implementation calculation for schematic capture. Final values should still be checked against the exact LED part, final PCB layout, and bring-up measurements.

---

## 2. Fixed design assumptions

The calculations below use the current revision-1 baseline:

- Cube size: **8×8×8 monochrome LED cube**
- Total LEDs: **512**
- Drive method: **one active layer at a time**
- Supply rail for display stage: **5 V**
- LED color: **blue**
- LED forward voltage assumption: **Vf = 3.2 V**
- Target peak LED current while active: **15 mA**
- Current limiting method: **one resistor per line**
- Line driver direction: **low-side sinking**
- Layer driver direction: **high-side sourcing**
- External adapter baseline: **5 V / 5 A**
- No separate 3.3 V regulator stage is added in this document

The topology assumed by this file is therefore:

- **64 low-side line channels** for column control
- **8 high-side layer switches** for layer enable

---

## 3. What the schematic must contain

For the display-driving portion, the schematic needs at minimum:

- **64 LED line resistors**
- **64 low-side sink outputs**
- **8 high-side layer switches**
- **8 layer-switch control stages** between the ESP32 GPIO and the high-side MOSFET gates
- **local decoupling** at every driver IC
- **bulk capacitance** on the 5 V rail near the display driver section

A practical implementation that matches these calculations is:

- **8 × TPIC6B595** for the 64 low-side sink lines
- **8 × P-channel MOSFETs** for the layer high-side switches
- **8 × small NPN transistors** to pull each P-MOSFET gate low from a 3.3 V GPIO

This gives a complete electrical path for all 64 columns and all 8 layers without asking the ESP32 to carry LED current directly.

---

## 4. Multiplexing consequences

Because the cube is scanned layer by layer:

- only **1 of 8 layers** is active at any moment
- up to **64 LEDs** can be on at once
- each LED has **1/8 duty cycle** in the worst sustained full-cube pattern

### 4.1 Maximum simultaneously active LEDs

$N_{active,max} = 8 \times 8 = 64$

### 4.2 Maximum layer current

$
I_{layer,max} = 64 \times 15\,mA = 960\,mA = 0.96\,A
$

So the selected **high-side layer switch for each layer must comfortably handle about 1 A peak**.

### 4.3 Average LED current per voxel at full-frame all-on

$
I_{LED,avg} = 15\,mA \times \frac{1}{8} = 1.875\,mA
$

This confirms the design is using moderate average LED stress even though the instantaneous active current is higher.

---

## 5. Current-limiting resistor calculation

Current limiting is one resistor per line, so the resistor value must be chosen from the instantaneous active-layer current path.

### 5.1 Current path model

For one lit LED in the active layer, the current path is:

$
5V \rightarrow P\text{-}MOSFET \rightarrow LED \rightarrow line\ resistor \rightarrow low\text{-}side\ sink
$

Use these first-pass voltage allowances:

- Supply: **5.0 V**
- LED forward voltage: **3.2 V**
- High-side switch drop allowance: **0.1 V**
- Low-side sink drop allowance: **0.1 V**
- Target LED current: **15 mA**

### 5.2 Ideal resistor value

$
R = \frac{5.0 - 3.2 - 0.1 - 0.1}{0.015} = 106.7\,\Omega
$

### 5.3 Selected standard value

Choose:

$
\boxed{R_{line} = 120\,\Omega}
$

This is the safer standard value because it keeps the design slightly conservative and gives margin for LED forward-voltage spread.

### 5.4 Expected LED current with 120 Ω

$
I_{LED} = \frac{5.0 - 3.2 - 0.1 - 0.1}{120} = 13.3\,mA
$

In practice, current will vary with the exact LED Vf and real driver voltage drops, but **120 Ω places the design in the intended ~13–15 mA peak range**.

### 5.5 Resistor dissipation

Peak resistor dissipation when a line is on:

$
P_{R,peak} = I^2R = (0.0133)^2 \times 120 \approx 21\,mW
$

Using the full 15 mA design target:

$
P_{R,peak,worst} = (0.015)^2 \times 120 = 27\,mW
$

Average dissipation at 1/8 duty:

$
P_{R,avg} \approx \frac{27\,mW}{8} \approx 3.4\,mW
$

### 5.6 Resistor selection result

For each of the 64 line resistors, use:

- **120 Ω**
- **1% preferred**
- **at least 0.125 W rating**
- **0603 or 0805 package is electrically sufficient**

**Schematic count:** **64 resistors**.

---

## 6. Low-side line-driver sizing

Each active column line must sink current from exactly one LED in the active layer.

### 6.1 Per-channel current requirement

$
I_{line,max} = 15\,mA
$

So each sink channel only needs to carry about **15 mA peak**.

### 6.2 Grouping into 8-bit driver packages

If one 8-bit sink register is assigned to 8 lines, then worst case per package is:

$
I_{package,max} = 8 \times 15\,mA = 120\,mA
$

### 6.3 Practical part choice

A suitable implementation part is **TPIC6B595**.

Why it fits this design:

- one IC provides **8 outputs**, so **8 ICs** cover **64 lines**
- outputs are **open-drain low-side sinks**
- outputs are rated for **150 mA continuous sink current per channel**
- it also provides the **shift-register + latch function**, so no separate line expansion IC is needed

That is much more than the required **15 mA per line**, so the device is electrically comfortable in this application.

### 6.4 Low-side driver result

Use:

- **8 × TPIC6B595**
- each output connected to one line through one **120 Ω resistor**
- shared serial data, clock, and latch lines daisy-chained across the eight devices
- OE can be used as a global blanking/PWM control line

**Schematic count:** **8 driver ICs**.

---

## 7. High-side layer-switch sizing

Each layer switch must source the full current of one active layer.

### 7.1 Required current rating

From Section 4:

$
I_{layer,max} = 0.96\,A
$

A sensible design rule is to choose a switch with at least **2× practical margin**, so target **2 A class or better** even though the calculated peak is about 1 A.

### 7.2 P-channel MOSFET requirement

Each layer switch should therefore satisfy:

- **P-channel MOSFET**
- **VDS rating ≥ 20 V**
- **continuous current rating comfortably above 1 A**
- **low RDS(on) at about -4.5 V gate drive**
- package practical for eight repeated channels

### 7.3 Example fit

A practical example is **AO3401A**.

Its ratings are comfortably above the calculated need for a 5 V, ~1 A layer switch.

### 7.4 Estimated conduction loss in one layer MOSFET

Using a conservative on-resistance estimate of **60 mΩ** at strong gate drive:

$
P_{MOSFET,on} = I^2R = (0.96)^2 \times 0.06 \approx 55\,mW
$

This is low enough that no heatsink or special thermal hardware is expected to be needed in revision 1, assuming reasonable PCB copper.

### 7.5 High-side switch result

Use:

- **8 × P-channel MOSFETs** of roughly the AO3401A class

**Schematic count:** **8 MOSFETs**.

---

## 8. Why a gate-drive stage is required

A 5 V high-side P-MOSFET cannot be driven correctly from a 3.3 V GPIO by itself.

If the MOSFET source sits at 5 V:

- GPIO high at 3.3 V gives

$
V_{GS} = 3.3 - 5.0 = -1.7\,V
$

That is not a reliable OFF state for a high-side P-MOSFET.

So each layer switch needs a small transistor stage that can:

- pull the MOSFET gate **up to 5 V** for OFF
- pull the MOSFET gate **near 0 V** for ON

A simple and practical solution is one **NPN transistor per layer**.

---

## 9. Layer gate-drive component calculation

### 9.1 Recommended gate-drive structure per layer

For each layer:

- **1 × NPN transistor**
- **1 × base resistor** from ESP32 GPIO to NPN base
- **1 × gate pull-up resistor** from P-MOSFET gate to 5 V
- **1 × small series gate resistor** between NPN collector node and MOSFET gate

### 9.2 NPN transistor selection

A small signal transistor such as **2N3904 / MMBT3904 class** is sufficient.

The transistor does not carry the LED layer current. It only drives the MOSFET gate.

### 9.3 Base resistor calculation

Choose a base resistor around:

$
R_B = 4.7\,k\Omega
$

Then base current from a 3.3 V GPIO is approximately:

$
I_B = \frac{3.3 - 0.7}{4.7k} \approx 0.55\,mA
$

That is more than enough for a small transistor that only needs to discharge a MOSFET gate and overcome the gate pull-up current.

### 9.4 Gate pull-up resistor

Choose:

$
R_{PU} = 10\,k\Omega
$

This keeps the MOSFET OFF by default at reset or during ESP32 boot.

Steady pull-up current when the NPN is ON:

$
I_{PU} = \frac{5V}{10k\Omega} = 0.5\,mA
$

This is safely small.

### 9.5 Series gate resistor

Choose:

$
R_G = 100\,\Omega
$

This is not for DC biasing. It is simply a good practice part to:

- reduce ringing
- soften peak gate-charge current
- make the gate path less abrupt

### 9.6 Layer control component count

Per layer:

- 1 NPN transistor
- 1 base resistor 4.7 kΩ
- 1 gate pull-up resistor 10 kΩ
- 1 gate resistor 100 Ω

For 8 layers total:

- **8 × NPN transistors**
- **8 × 4.7 kΩ resistors**
- **8 × 10 kΩ resistors**
- **8 × 100 Ω resistors**

---

## 10. Display power estimate from the final chosen current

Using the more concrete 120 Ω resistor selection, a realistic first estimate is about **13.3 mA peak** per active LED.

### 10.1 Display current

$
I_{display,max} = 64 \times 13.3\,mA \approx 0.85\,A
$

### 10.2 Display power from 5 V rail

$
P_{display,max} = 5V \times 0.85A \approx 4.25W
$

### 10.3 Upper-bound design case

If the actual LED current after part tolerances is closer to the original 15 mA design target:

$
I_{display,max,upper} = 64 \times 15\,mA = 0.96\,A
$

$
P_{display,max,upper} = 5V \times 0.96A = 4.8W
$

So the display section should be designed for roughly **0.85–0.96 A peak layer current**.

---

## 11. Bulk capacitance and decoupling

The display load changes quickly as layers switch, so the schematic should not rely only on the external adapter and cable.

### 11.1 Local bulk capacitor near the driver stage

Use the capacitor sizing relation:

$
C = \frac{I \cdot \Delta t}{\Delta V}
$

Take a first-pass transient target:

- current step to support locally: **0.96 A**
- short transient window: **25 µs**
- allowed local droop: **0.2 V**

Then:

$
C = \frac{0.96 \times 25\,\mu s}{0.2} = 120\,\mu F
$

So **100 µF is the minimum useful local bulk value**, and **220 µF gives better margin**.

### 11.2 Input bulk capacitor at board entry

Because wiring, connectors, and scan transients still exist, add a larger capacitor at the 5 V input as well.

Recommended first-pass values:

- **1 × 470 µF electrolytic or polymer** near 5 V board entry
- **1 × 100 µF to 220 µF** near the display driver section

### 11.3 Ceramic decoupling

Place:

- **100 nF per TPIC6B595** directly at the IC supply pins
- **100 nF near each local logic/power entry cluster**
- the normal **ESP32 local decoupling required by the chosen ESP32 implementation**

### 11.4 Decoupling count for the display section

At minimum, this means:

- **8 × 100 nF** for the eight TPIC6B595 devices
- **1 × 470 µF** at board input
- **1 × 100–220 µF** near driver/layer switching area

---

## 12. Trace and connector implications

Even though the average total current is much lower than the adapter rating, the active-layer current is close to 1 A.

That means the schematic and PCB should treat these nets as current-carrying power nets:

- **5 V input path**
- **main return path**
- **layer source distribution**
- **driver-section supply path**

Practical implications:

- use a connector comfortably rated above the expected current
- keep the 5 V entry path wide
- keep layer-current returns out of sensitive logic return paths as much as possible
- place bulk capacitance physically close to the layer/line driver area

---

## 13. Final schematic-oriented result

### 13.1 Main electrical decisions now fixed by calculation

For the current schematic direction, use:

- **64 × 120 Ω line resistors**
- **8 × TPIC6B595** low-side sink drivers
- **8 × P-channel MOSFET layer switches** in the AO3401A class
- **8 × NPN transistors** in the 2N3904/MMBT3904 class for MOSFET gate drive
- **8 × 4.7 kΩ base resistors**
- **8 × 10 kΩ gate pull-up resistors**
- **8 × 100 Ω series gate resistors**
- **8 × 100 nF driver decoupling capacitors**
- **1 × 470 µF input bulk capacitor**
- **1 × 100–220 µF local driver bulk capacitor**

### 13.2 Electrical capability summary

- Maximum simultaneously active LEDs: **64**
- Peak current per line: **~13–15 mA**
- Peak current per 8-line driver IC: **~106–120 mA**
- Peak current per active layer switch: **~0.85–0.96 A**
- Peak display power from 5 V rail: **~4.25–4.8 W**

### 13.3 What this file resolves for the schematic

This document now fixes the main missing schematic-support numbers:

- resistor value per line
- number of line-driver ICs
- number and type of layer switches
- need for gate-drive transistors and their support resistors
- minimum local decoupling and bulk capacitance

That is enough to draw a complete first-pass driver schematic without leaving any major display-path component undefined.

---

## 14. References for component classes used in this calculation

These example component classes match the electrical requirements derived above:

- **TPIC6B595**: 8-bit power shift register with open-drain low-side outputs
- **AO3401A**: P-channel MOSFET suitable for 5 V high-side switching with comfortable current margin
- **2N3904 / MMBT3904**: small-signal NPN transistor for MOSFET gate drive

Exact manufacturer and package may still be chosen in the BOM phase as long as the final parts meet or exceed the requirements stated in this document.
