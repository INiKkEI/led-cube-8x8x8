# Power Budget — 8×8×8 LED Cube

## 1. Purpose

This document estimates the expected power requirements of the 8×8×8 LED cube and its control electronics for revision 1.

The goal is to:
- identify the main current consumers
- estimate worst-case and typical power draw
- justify external supply sizing
- state the main thermal expectations before schematic and PCB completion

This is a planning estimate, not a measured result. The values here should be refined later after the schematic, LED part selection, and first hardware tests are complete.

---

## 2. Fixed design assumptions

This estimate is based on the current revision-1 architecture:

- monochrome **8×8×8** LED cube
- **multiplexed layer scanning**
- **one active layer at a time**
- **5 V** main external input
- local **3.3 V** regulation for ESP32 logic
- dedicated driver stage between the ESP32 and cube
- brightness controlled conservatively through timing, not aggressive LED overdrive

---

## 3. Main power consumers

The major current consumers in revision 1 are:

1. **LED display load**
   - the dominant power consumer
   - current depends on how many LEDs are on in the active layer

2. **Layer-switching and line-driver circuitry**
   - small control current
   - some additional switching and conduction losses

3. **ESP32 control electronics**
   - ESP32 module
   - BLE activity
   - firmware execution
   - small supporting logic

4. **3.3 V regulation losses**
   - depends on regulator type
   - can become noticeable if a linear regulator is used

---

## 4. Working assumptions for first estimate

### 4.1 LED current assumption

For revision 1, use this baseline planning value:

- **10 mA peak current per active LED**

This is intentionally moderate and matches the project direction of avoiding extreme operating conditions.

### 4.2 Multiplexing implication

Because the cube is scanned one layer at a time:

- maximum simultaneously active LEDs = **64**
- total LEDs in the cube = **512**
- each LED has **1/8 duty cycle** at full-frame “all on”

Important: although each individual LED is only on for 1/8 of the time, the **power supply still sees one active layer continuously during refresh**, so the display rail current is based on the active layer load, not on 512 LEDs all being driven at once.

### 4.3 Logic-domain estimate

Use the following conservative planning numbers for the control side:

- **ESP32 + BLE + firmware activity:** up to **250 mA**
- **small support / driver logic overhead:** about **50 mA**

These are budgeting values for early design. Actual current should be replaced later with measured board data.

---

## 5. Estimated current and power

## 5.1 Display worst-case current

Worst case for the active layer:

- 64 LEDs on at once
- 10 mA peak per LED

$$
I_{display,max} = 64 \times 10\,mA = 640\,mA = 0.64\,A
$$

## 5.2 Display power from 5 V rail

$$
P_{display,max} = 5\,V \times 0.64\,A = 3.2\,W
$$

So the display section alone is expected to draw about:

- **0.64 A**
- **3.2 W**

in the worst sustained “all voxels on” case at the chosen planning current.

## 5.3 Control and support electronics

Conservative early budget:

- ESP32 and 3.3 V domain: **0.25 A equivalent input budget**
- support / driver control overhead: **0.05 A**

## 5.4 Total estimated input current

$$
I_{total,est} = 0.64\,A + 0.25\,A + 0.05\,A = 0.94\,A
$$

Rounded engineering estimate:

- **expected worst-case continuous input current: about 1.0 A**
- **recommended design target for continuous current handling: 1.2 A**

## 5.5 Total estimated input power

$$
P_{total,est} = 5\,V \times 0.94\,A = 4.7\,W
$$

Rounded:

- **expected worst-case total input power: about 5 W**

---

## 6. Sensitivity check for LED current choice

Because LED brightness and part selection may change later, it is useful to see how the supply requirement changes with the chosen peak LED current.

| Peak LED current per active LED | Display current | Approx. total input current | Approx. total input power |
|---|---:|---:|---:|
| 8 mA  | 0.512 A | ~0.81 A | ~4.1 W |
| 10 mA | 0.640 A | ~0.94 A | ~4.7 W |
| 15 mA | 0.960 A | ~1.26 A | ~6.3 W |

### Interpretation

- **10 mA peak** is a reasonable baseline for the first design estimate.
- Even if the design later moves toward **15 mA peak**, the total continuous input current is still around **1.3 A**, not several amps.
- The external adapter can therefore be sized with large margin for stability and future tuning.

---

## 7. Supply sizing conclusion

### 7.1 External supply

For the current design direction, the estimated continuous load is approximately:

- **1.0 A typical worst-case planning value**
- **up to about 1.3 A** if LED current is increased toward 15 mA peak

However, revision 1 should still keep the external supply baseline at:

- **5 V / 5 A minimum**

### 7.2 Why this supply size still makes sense

A 5 V / 5 A adapter is larger than the current estimate, but it is still justified because it provides:

- large margin for startup and scanning transients
- reduced risk of voltage sag during layer switching
- lower adapter heating
- flexibility while the exact LED current and final driver topology are still being finalized
- headroom for future design corrections without replacing the supply

### 7.3 PCB and connector implication

Even though the adapter is rated far above the estimated average draw, the board itself should still be designed around the expected load:

- **design 5 V display-current paths comfortably for at least 2 A continuous**
- keep logic and display return paths separated as much as practical
- place bulk capacitance near the display driver section
- place local decoupling near ESP32, driver ICs, and switching stages

---

## 8. Thermal expectations

At the current 10 mA planning point:

- total system input power is about **5 W**
- most of that power is associated with the **display path**
- heat will mainly appear in:
  - current-limiting resistors or current-setting elements
  - line/layer driver devices
  - the 3.3 V regulator
  - to a much smaller extent, the ESP32 itself

### Expected thermal behavior

- **No active cooling should be necessary** for revision 1 if the PCB layout is reasonable.
- The board may become **noticeably warm** around driver devices and regulator areas during worst-case display patterns.
- If a **linear 3.3 V regulator** is used, it should be checked carefully for dissipation and temperature rise.
- Raising peak LED current later will directly increase both supply demand and thermal stress.

---

## 9. Design result summary

| Item | Estimated value |
|---|---:|
| Main input voltage | 5 V |
| Logic rail | 3.3 V |
| Max active LEDs at once | 64 |
| Baseline peak current per active LED | 10 mA |
| Estimated display current | 0.64 A |
| Estimated total input current | ~0.94 A |
| Rounded design target | 1.2 A continuous |
| Estimated total input power | ~4.7 W |
| Recommended external adapter baseline | 5 V / 5 A minimum |

---

## 10. Final conclusion

The dominant load in the system is the multiplexed LED display, not the ESP32. Under a reasonable first-pass assumption of **10 mA peak current per active LED**, the complete cube and control electronics are expected to draw roughly **1 A from the 5 V input**, or about **5 W total**.

This means the currently selected **5 V / 5 A** supply direction is electrically conservative and appropriate. It provides comfortable margin for multiplexing transients, final component selection, and early bring-up while keeping the overall design aligned with the project architecture.
