# 3-Stage MOSFET Common-Source Amplifier

An open-loop analog voltage amplifier designed in KiCad and verified with SPICE simulation, built for ELE404 (Electronic Circuits 1). The design uses three cascaded common-source NMOS stages, AC-coupled, to meet a target gain, bandwidth, and power budget from a single 3.3V supply.

## Design specs (given)

| Category | Spec |
|---|---|
| Supply | Single supply, VDD = 3.3V |
| Gain (unloaded) | ≥ 60 dB, ≤ 40 dB/stage |
| Bandwidth | ≥ 500 kHz (−3dB) |
| Output swing | ≥ 1.5 Vpp |
| DC power | ≤ 1 mW |
| Input resistance | ≥ 100 kΩ |

## Approach

Three common-source stages are cascaded with AC coupling capacitors between them, so each stage can be biased independently while blocking DC. Operating point was set at ID = 90µA, VOV = 0.2V (to maximize gm and gain per stage), and VDS = 1.65V (half the supply, to keep headroom for saturation). Gate biasing uses a resistor-divider network sized to meet the ≥100kΩ input resistance requirement.

## Results

| Parameter | Requirement | Simulated | Result |
|---|---|---|---|
| Gain (unloaded) | ≥ 60 dB | 63.9 dB | Pass |
| Gain per stage | ≤ 40 dB | 21.3 dB | Pass |
| Bandwidth | ≥ 500 kHz | 100 MHz | Pass |
| DC power | ≤ 1 mW | 0.94 mW | Pass |
| Input resistance | ≥ 100 kΩ | 130 kΩ | Pass |
| Gain reduction under load | ≤ 10% | 54% | Fail |
| Output swing under load | ≥ 1.5 Vpp | 1 Vpp | Fail |

The design meets every unloaded spec with margin. Under the specified 10kΩ load, gain and swing both fall short — the common-source topology has a high output resistance (~18kΩ), so the load forms a voltage divider with it and pulls gain down significantly. A common-drain (source-follower) output stage would fix this, since its output resistance (~1/gm, ≈1.6kΩ here) is far lower than the CS stage's.

## Files

- `Amplifier.kicad_sch` — full schematic (3-stage CS amplifier with biasing)
- `Amplifier.kicad_pro` — KiCad project file

## Tools

- **KiCad** — schematic capture and SPICE simulation
- Hand analysis for biasing, gm/ro, gain, and bandwidth
