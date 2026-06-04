# Pi 3B/3B+ Test Points

Community-sourced test point map for Raspberry Pi 3 Model B and 3 Model B+. Test points are labeled "PP" on the PCB (mostly on the underside).

## Test Point Map

| Pad | Function | Expected Voltage | Notes |
|-----|----------|-----------------|-------|
| PP1 | VCC +5V (from micro-USB) | 4.8–5.25V | Before polyfuse |
| PP2 | VCC +5V (from micro-USB) | 4.8–5.25V | Before polyfuse |
| PP3 | GND | 0V | Reference ground |
| PP4 | GND | 0V | Reference ground |
| PP5 | GND | 0V | Reference ground |
| PP6 | GND | 0V | Reference ground |
| PP7 | 5V after polyfuse | 4.8–5.25V | Should match PP1; drop = polyfuse issue |
| PP8 | 3.3V rail | 3.3V ± 5% | From PAM2306 (3B) or MxL7704 (3B+) |
| PP9 | 1.8V rail | 1.8V ± 5% | SoC core voltage |
| PP10 | Power LED | ~1.8V when lit | Goes from 3.3V to ~2V on brownout |
| PP13 | ACT (activity) LED | Toggles | SD card access indicator |
| PP20 | HDMI 5V (H5V) | 5V | 5V at HDMI connector |
| PP21 | RUN (reset) | 3.3V (high) | Pull low momentarily to reset SoC |
| PP22 | Ethernet Activity LED | Toggles | Ethernet traffic indicator |
| PP23 | Ethernet Link LED | High when linked | Link established indicator |
| PP24 | Composite video | AC signal | Analog video output |
| PP25 | Left audio | AC signal | 3.5mm jack left channel |
| PP26 | Right audio | AC signal | 3.5mm jack right channel |
| PP27 | USB VCC | ~5V | Power to USB ports (after USB polyfuse on 3B) |
| PP35 | VCC +5V | 4.8–5.25V | Additional 5V tap |
| PP36 | USB D- | — | USB data line |
| PP39 | — | — | Undocumented |
| PP40 | — | — | Undocumented |
| PP41 | USB D+ | — | USB data line |
| PP42–PP47 | USB D-/D+ pairs | — | Individual USB port data lines |
| PP48–PP51 | GND | 0V | Additional grounds |
| PP52 | Ethernet TX+ | — | Ethernet transmit |
| PP53 | Ethernet 3V3 | 3.3V | Ethernet PHY power |
| PP54 | Ethernet RX+ | — | Ethernet receive |
| PP55 | Ethernet TX- | — | Ethernet transmit |
| PP56 | Ethernet RX- | — | Ethernet receive |
| PP57 | Ethernet 3V3 | 3.3V | Ethernet PHY power |

## Key Diagnostic Measurements

All measurements relative to GND (PP3, PP4, PP5, or PP6).

### Power Chain Verification (do these in order)

1. **PP1/PP2** — Is power arriving from micro-USB? If 0V: bad cable, bad supply, or broken micro-USB connector.
2. **PP7** — Is power getting past the polyfuse? If 0V but PP1 has 5V: polyfuse tripped/failed.
3. **PP8** — Is 3.3V rail alive? If 0V: regulator failure (PAM2306 or MxL7704) or downstream short.
4. **PP9** — Is 1.8V rail alive? If 0V: regulator failure.

### Short Circuit Detection (power OFF, resistance mode)

| Measurement | Expected | Fault if... |
|-------------|----------|-------------|
| PP8 to GND | >100Ω | Near 0Ω = 3.3V rail shorted (dead PMIC or LAN9514) |
| PP9 to GND | >50Ω | Near 0Ω = 1.8V rail shorted (dead regulator or SoC) |
| PP1 to PP7 | < 1Ω | >10Ω = polyfuse damaged/open |

### USB Power Verification

- **PP27 to GND**: Should be ~5V when board is running. If 0V but board boots: USB polyfuse blown (3B only; 3B+ doesn't have individual USB fuses).
- On the Pi 3B, there are individual polyfuses per USB port pair. The 3B+ removed these.

## RUN Pad (PP21) — Reset Function

PP21 is the RUN/reset signal. It's held high (3.3V) during normal operation by an internal pull-up.

**Uses:**
- **Reset button**: Solder a momentary switch between PP21 and GND to create a hardware reset
- **Diagnostic**: If PP21 reads 0V during operation, something is holding the SoC in reset
- **Recovery**: Brief short to GND resets the SoC (like pressing a reset button)

**Caution**: Holding RUN low keeps the SoC in permanent reset — only pulse it briefly.

## Notes

- Test points are on the **underside** (bottom) of the PCB on most Pi 3 revisions
- Use fine-tip multimeter probes or pogo pins — pads are small
- For reliable measurements under load, power the Pi with a known-good supply and a typical boot SD card inserted
- The Pi 3B has slightly different pad locations than the 3B+ — visually confirm labels on PCB silkscreen

## Sources

- Raspberry Pi Stack Exchange: "What are the functions of the test pads on the Pi 3B/3B+"
- Raspberry Pi reduced schematics (Rev 1.2 for 3B, Rev 1.0 for 3B+)
- Community forum posts consolidating measurements
