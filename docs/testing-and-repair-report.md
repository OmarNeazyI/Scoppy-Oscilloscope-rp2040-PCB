# Testing and Repair Report

## Project outcome

I assembled the PCB and debugged and tested the hardware until I had a working prototype. The completed board loads firmware, connects to the Scoppy Android application, acquires signals and generates signals.

<a href="../images/pcb-top-presentation.png"><img src="../images/pcb-top-presentation.png" alt="Assembled Scoppy PCB" width="520"></a>

[Original PCB photograph](../images/pcb-top-original.jpg) · [All project photographs](../images/README.md)

## Assembly process

I used the following equipment during assembly and repair:

- solder-paste stencil;
- solder-paste dispenser;
- heated bed/preheater;
- reflow oven;
- soldering iron; and
- hot-air rework station.

I applied solder paste using the stencil and solder-paste dispenser. I used the reflow oven for the RP2040 and several of the smaller-pad SMD components. The heated bed, hot-air station and soldering iron were used for the remaining assembly, hand soldering, desoldering and component rework.

## Initial bring-up

The board received USB power and the green indicator illuminated, but Windows did not detect the RP2040 USB device. I started with the power system before moving to the USB data path.

Voltage measurements were taken with the board powered. Resistance and continuity measurements were taken with the board disconnected from power.

## Power and control measurements

| Test point | Measured value | Result |
|---|---:|---|
| USB supply at both sides of FB1 | 5.15 V | USB power reached the board |
| U1 regulator output | 3.34 V | 3.3 V rail operating |
| C6 / RP2040 core supply | 1.09 V | Core supply operating |
| RP2040 RUN input | 3.26 V | Reset input high |
| R6 and R7 | Approximately 5 kΩ each | Consistent with the 5.1 kΩ design values |
| SW1 | Open when released, closed when pressed | BOOTSEL switch operating |

These results confirmed that the board was receiving power, the main regulator was working and the RP2040 was not being held in reset.

## USB data-path testing

The USB paths in the PCB are:

| Signal | Connection path |
|---|---|
| D+ | USB-C connector → protection/filter network → R3 → RP2040 USB D+ |
| D− | USB-C connector → protection/filter network → R4 → RP2040 USB D− |

| Test | Measured value | What it showed |
|---|---:|---|
| R3 | Approximately 27 Ω | Correct USB series resistance |
| R4 | Approximately 27 Ω | Correct USB series resistance |
| D+ to D− at the USB-side network | Approximately 0.7–0.8 Ω | Low-resistance short between the data lines |
| Multimeter probe baseline | Approximately 0.6 Ω | Confirmed that the D+/D− reading was close to a direct short |
| D+ to D− on the RP2040 side of R3/R4 | 52.7–54.01 Ω | The two 27 Ω resistors separated the RP2040 from the upstream short |
| End-to-end D+ and D− paths | Approximately 30 Ω each | Data paths passed through their series resistors |

The approximately 54 Ω reading was especially useful: it matched the two 27 Ω resistors in series through the short on the connector side. This focused the repair on the USB connector, protection components and L1 rather than the RP2040 side of the circuit.

## Additional component checks

| Component or section | Measurement |
|---|---:|
| Digital R1 | 989 Ω |
| Crystal-network R5 | 1002 Ω |
| Analog R1_1 | Approximately 460 kΩ |
| Analog R1_2 | Approximately 320 kΩ |
| RV1 | Approximately 516 kΩ in circuit |
| RV2 | Approximately 600 kΩ in circuit |

I also checked continuity through the crystal network, the XIN/XOUT connections and the relevant ground connections.

## Repair

I found a short underneath/around L1. I removed and replaced L1, then repeated the resistance checks and confirmed that the short was no longer present.

The USB-C connector also required rework because its data and power pads were very small. I desoldered and resoldered J1 several times until the connections were correct.

## Final validation

| Functional check | Result |
|---|---|
| Firmware loading | Successful |
| USB connection | Successful |
| Scoppy app connection | Successful |
| Signal acquisition | Successful |
| Signal generation | Successful |

<a href="../images/pcb-bottom-presentation.png"><img src="../images/pcb-bottom-presentation.png" alt="Scoppy PCB underside" width="520"></a>

[Original underside photograph](../images/pcb-bottom-original.jpg) · [Photo sources](../images/README.md)

## What I learned

- How to assemble a mixed SMD PCB using stencil, reflow and manual rework techniques.
- How to separate power, reset and USB data-path problems during bring-up.
- How to use series resistors as measurement boundaries during fault isolation.
- How probe resistance affects low-ohm measurements.
- How to inspect and rework small USB-C and filter-component pads.
- How to verify a repair through repeated electrical and functional testing.

