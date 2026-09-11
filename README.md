# MOSFET Amplifier Design

Multi-stage MOSFET voltage amplifier designed and SPICE-simulated on a 180nm CMOS process — three cascaded common-source stages with a source-follower buffer, meeting all gain, bandwidth, power, and swing targets.

## Specifications (all met)

| Quantity | Target | Theoretical | Simulated |
|---|---|---|---|
| Gain | ≥ 60 dB | 63.6 dB | 65.5 dB |
| Bandwidth | ≥ 500 kHz | — | ✓ |
| Input resistance | ≥ 100 kΩ | 750 kΩ | 794 kΩ |
| Output swing | ≥ 1.5 Vpp | 1.72 V | 1.55 V |
| Load sensitivity | ≤ 10% | 5.15% | — |
| DC power | ≤ 1 mW | 0.987 mW | 0.997 mW |
| Max branch current | ≤ 200 µA | 158 µA | 158.7 µA |
| Supply | 3.3 V single | 3.3 V | 3.3 V |

## Architecture

A four-stage MOSFET voltage amplifier powered from a single 3.3 V supply:

- **Three common-source gain stages (Q4, Q3, Q1)** — W/L = 22/1 µm, RD = 30 kΩ drain resistor, each biased with a 3 MΩ / 1 MΩ resistor-divider network setting VGS ≈ 0.825 V. These provide the bulk of the voltage gain (~11.85 V/V, 21.48 dB per stage).
- **Source-follower output buffer (Q2)** — W/L = 100/1 µm, biased via a 220 kΩ / 1 MΩ divider with a 12 kΩ source resistor. Reduces output resistance and improves the circuit's ability to drive the load, at the cost of a small gain loss (~0.9 V/V).

AC coupling capacitors isolate the DC bias of each stage from the next while passing the amplified signal through. The input is introduced through a 50 Ω source resistance, and the output drives a 10 kΩ / 2 pF load.

## Design process

1. **Architecture selection** — cascaded common-source stages for gain, source follower for load isolation and drive strength
2. **Hand calculations** — device sizing and biasing solved from MOSFET square-law equations (ID ≈ 41.83 µA per CS stage; buffer bias solved via the source-follower quadratic, ID4 = 158 µA)
3. **Schematic capture** in KiCad
4. **SPICE simulation** using process BSIM4 NMOS models — DC operating point, AC (loaded and unloaded), and transient analyses, with iterative refinement until all specs were satisfied

## Hand calculation vs. simulation

Hand calculations predicted 63.6 dB total gain against a simulated 65.5 dB loaded gain — a close match, with the largest single discrepancy in drain current and transconductance (~10%), attributable to non-ideal transistor behavior and simplifications inherent to the square-law hand model. All other parameters matched within a few percent.

## Files

- `mosfet_amplifier.kicad_sch` — KiCad schematic source
- `mosfet_amplifier_schematic.pdf` — exported schematic (Figure 1)
- `ELE404_Project_Report.pdf` — full written report, including OP/AC/transient simulation results, hand calculations, and conclusions

## Course context

Designed for ELE 404 (Electronics I) at Toronto Metropolitan University. Shared component values were used across group members per course requirements; all written analysis and report text are my own.
