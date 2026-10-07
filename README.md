# Schmitt Trigger Based Level Detection and Noise Rejection Control Circuit

CSE350 course project.

**Ahnaf Tahmid Tausif** (22201402) &middot; **Md. Taosif Farhan Samee** (22299145)

---

## About

A two-level automatic lamp controller built around three LM741 operational
amplifiers.

The lamp switches **ON** when the sensed voltage rises past **5.25 V**, and
switches **OFF** only when it falls back below **3.75 V**. The 1.50 V gap
between these two thresholds is the hysteresis window. Any disturbance smaller
than that gap cannot cause a switch, which is what removes the flicker that an
ordinary single-threshold comparator produces.

An ordinary comparator stage is built alongside the Schmitt trigger and fed from
the same buffered signal, so the two can be compared directly under identical
input conditions.

---

## Circuit stages

| Stage | Part | Function |
|---|---|---|
| Sensor | LDR + 2.2k + 10k trimmer + 22k | Produces a voltage that rises as light falls |
| Buffer | LM741 (U1) | Isolates the sensor from the feedback network |
| Comparator | LM741 (U2) | Single-threshold reference stage, for comparison |
| Schmitt trigger | LM741 (U3) | Two thresholds, set by 100k positive feedback |
| Driver | BC547 + 1N4148 | Switches the white LED lamp |

Signal path:

```
LDR / POT  ->  U1 buffer  ->  VSENSE  -+->  U2 comparator  (demonstration)
                                       |
                                       +->  U3 Schmitt  ->  BC547  ->  lamp
```

---

## Design values

| Quantity | Symbol | Value | Set by |
|---|---|---|---|
| Reference voltage | V_ref | 4.09 V | 12k / 10k divider |
| Feedback factor | k | 0.1 | R1 / R2 = 10k / 100k |
| Saturation voltage | V_sat | 7.5 V (measured) | LM741 |
| Window centre | V_C | 4.50 V | V_ref (1 + k) |
| Upper threshold (lamp ON) | V_TH | 5.25 V | V_ref(1+k) + k V_sat |
| Lower threshold (lamp OFF) | V_TL | 3.75 V | V_ref(1+k) - k V_sat |
| Hysteresis width | dV | 1.50 V | 2 k V_sat |

The two design parameters are independent: **R2** sets the *width* of the
window, and **V_ref** sets its *position*. Either can be changed without
disturbing the other.

---

## Components

| Component | Value / part | Qty |
|---|---|---|
| Operational amplifier | LM741 | 3 |
| Transistor | BC547 | 1 |
| Diode | 1N4148 | 1 |
| Trimmer preset | 10k | 1 |
| Potentiometer (linear) | 10k | 1 |
| Light dependent resistor | LDR | 1 |
| Resistors | 220R, 1k x3, 2.2k, 4.7k, 10k x3, 12k, 22k, 100k | - |
| Ceramic capacitors | 100 nF | 3 |
| LEDs | white x1, red x2, green x2 | 5 |
| Supply | 9 V battery x2 (dual rail) | 2 |

---

## Files

| File | Description |
|---|---|
| `report.tex` | IEEE-format report source (LaTeX, compiles on Overleaf) |
| `project_demo_notes.pdf` | Demonstration and viva notes |
| `schematic.png` | Circuit schematic |
| `simulation.png` | Tinkercad simulation |
| `hardware.jpg` | Breadboard implementation |

### Building the report

Upload `report.tex` and the three image files to a blank
[Overleaf](https://www.overleaf.com) project and compile. The `IEEEtran` class
is included with Overleaf, so no additional packages are required.

---

## Notes

The two input sources serve different purposes. The **LDR** provides a real
signal carrying genuine noise, which is required to demonstrate comparator
instability. The **potentiometer** provides a clean, controllable voltage,
which is required to measure the thresholds accurately. Both feed the same
buffer, so the measured thresholds are identical for either source.
