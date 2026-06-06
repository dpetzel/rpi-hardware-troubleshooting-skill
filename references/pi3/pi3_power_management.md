# Pi 3B/3B+ Power Management

## Architecture Differences

The Pi 3B and 3B+ have fundamentally different power management designs. Always confirm which board revision you're working with before diagnosing power issues.

### Pi 3 Model B (non-plus)

- **Regulator**: PAM2306AYPKE (U3) — dual-channel synchronous buck converter
  - Channel A: 3.3V rail (PP8)
  - Channel B: 1.8V rail (PP9)
- **Input protection**: Polyfuse (F1) between micro-USB 5V input and board 5V rail
- **Design**: Discrete, simple — fewer failure modes but less protection
- **Max 3.3V output**: ~800mA (shared with SoC, LAN9514, peripherals)

### Pi 3 Model B+ (plus)

- **Regulator**: MxL7704 (MaxLinear) — 5-output Universal PMIC
  - 4 switching regulators + 1 LDO (100mA)
  - I²C interface to SoC for dynamic voltage scaling and status monitoring
  - Input range: 4.0V–5.5V (absolute max 6V)
- **Input protection**: Polyfuse between micro-USB and PMIC input
- **Design**: Integrated, intelligent — more features but single point of failure
- **Max 3.3V output**: ~1.5A (significantly more than Pi 3B)

## Common Failure Modes

### Pi 3B: PAM2306 Failure

**Symptoms:**
- Red PWR LED lit but no boot activity (green LED never blinks)
- PP8 reads 0V or significantly below 3.3V
- PP9 reads 0V or significantly below 1.8V

**Causes:**
- Overvoltage on micro-USB input (>5.5V kills it)
- 3.3V GPIO pin shorted to 5V or GND
- Downstream short (e.g., failed LAN9514 dragging the rail)

**Diagnosis:**
1. Measure PP8 (3.3V) and PP9 (1.8V) relative to PP3/PP5 (GND)
2. If both rails are dead but 5V is present at PP7: PAM2306 is likely failed
3. Check for shorts: measure resistance from PP8 to GND with power off — should NOT be near-zero
4. If 3.3V rail is shorted to GND, remove LAN9514 heat connection first (it can be the cause)

**Repair (advanced):**
- PAM2306AYPKE is a standard SOIC-8 package
- Requires hot air rework station (~350°C)
- After replacement, verify inductor integrity on output pins

### Pi 3B+: MxL7704 PMIC Failure

**Symptoms:**
- Red PWR LED lit, completely dead otherwise
- PP8 reads 0V (3.3V rail dead)
- Resistance from 3.3V to GND measures near-zero (internal short in PMIC)

**Causes (most common):**
- **5V shorted to 3.3V GPIO pin** — The MxL7704 attempts to clamp the overvoltage on its 3.3V output. This destroys the chip, permanently shorting the 3.3V rail to GND internally.
- Overvoltage input (>6V, e.g., wrong power adapter)
- Board placed on conductive surface while powered
- ESD damage through GPIO

**Diagnosis:**
1. Measure PP8 to GND — if 0Ω or near-zero, PMIC is destroyed
2. Check if board draws excessive current (>1A at idle with nothing connected = short)
3. The MxL7704 may feel warm/hot even with no boot activity

**Repair (advanced):**
- MXL7704-R3 available on AliExpress (~$2.50–3.50)
- 5mm × 5mm QFN package — requires hot air rework station
- After replacement, the new chip should output correct voltages with no programming needed (hardware-configured defaults work for Pi)

## Polyfuse (F1) Diagnostics

Both Pi 3B and 3B+ have a resettable polyfuse between the micro-USB power input and the board's 5V rail.

**Location**: Underside of PCB, near the micro-USB connector. On the 3B, oriented near the upper-left standoff hole when USB ports face right and HDMI faces up.

**Function**: Protects against overcurrent (trips at ~2.5A sustained)

**Symptoms of tripped/failed polyfuse:**
- Board appears completely dead (no red LED)
- 5V present at PP1/PP2 (before polyfuse) but not at PP7 (after polyfuse)

**Testing:**
- Power off, measure resistance across polyfuse: should be < 1Ω
- If high resistance (>10Ω): polyfuse is tripped or damaged
- Tripped polyfuse should self-reset after cooling (minutes to hours)
- Permanently damaged polyfuse: replace or bridge with wire (loses protection)

**Bypass test**: Temporarily bridge PP1 to PP7 with a wire to confirm polyfuse is the issue (remove bridge after testing — this defeats overcurrent protection).

## Crystal (X2) Failure — 19.2MHz System Oscillator

The 19.2MHz crystal (X2) provides the master clock to the BCM2837B SoC. Without it, the SoC cannot execute any code — including the GPU boot ROM. This presents as a board with healthy power rails but zero boot activity.

**Schematic details (Pi 3B+):**
- Crystal: X2, 19.2MHz, 4-pad SMD, 3.2mm × 2.5mm package
- Load capacitors: C165 and C178, both 18pF (0402/1005 package)
- Connects to SoC pins: XTALP (V8) and XTALN (U8)
- Powered from PLL_VDD (1.8V rail)

**Symptoms:**
- Red PWR LED solid, green ACT LED never blinks
- No HDMI output
- SoC is cold (not warm to touch)
- All voltage rails measure correctly (PP7=5V, PP8=3.3V, PP9=1.8V)
- Current draw ~120–340mA but no activity

**Diagnosis without oscilloscope:**
1. DC voltage on crystal signal pads (power on):
   - XTALP (drive output from SoC): ~0.5–0.8V DC = SoC amplifier is alive and trying
   - XTALN (return to SoC): 0V DC = crystal is NOT resonating
   - Both at 0V: SoC amplifier dead or no 1.8V PLL power
2. AC voltage (multimeter AC mode, power on):
   - < 0.01V AC on signal pads = not oscillating (multimeter can't measure 19.2MHz accurately, but complete absence confirms no activity)
3. Continuity (power off):
   - Two ground pads should have continuity to each other and to board GND
   - Two signal pads should be OPEN (infinite resistance) — if near-zero, crystal is shorted internally
4. RUN pad reset (PP15/PP21): try briefly shorting to GND — rules out stuck SoC

**Key insight:** The SoC driving 0.7V on XTALP while XTALN reads 0V is strong evidence of a dead crystal with a healthy SoC. If the SoC were dead, the drive pin would also read 0V.

**Replacement part (spec-matched, not yet verified in practice):**
- TXC 7V-19.200MDDJ-T (Digikey: 887-7V-19.200MDDJ-TCT-ND)
- 19.2MHz, 18pF load, 60Ω ESR, ±20ppm, 4-SMD 3.2×2.5mm
- ~$0.54/ea

**Alternative:** ECS-192-18-33-JGN-TR (Digikey: XC2308CT-ND), 80Ω ESR, ~$0.47/ea

**Also check:** The two 18pF load capacitors (C165, C178) adjacent to the crystal. If either is cracked, missing, or tombstoned, the oscillator won't start even with a good crystal. Inspect with magnification.

## Micro-USB Connector Issues

The micro-USB connector on Pi 3 is mechanically weaker than USB-C and a common failure point on used boards:

- **Wiggle test**: With power applied, gently wiggle the micro-USB cable. If red LED flickers, the connector or solder joints are failing.
- **Visual inspection**: Look for cracked solder joints on the 5 signal pins and 2 mounting tabs
- **Cold solder joint**: Reflow with soldering iron if joints look dull/cracked
- **Broken connector**: Full replacement requires desoldering — consider powering via GPIO pins 2+4 (5V) and pin 6 (GND) as a workaround, noting this bypasses the polyfuse

## Quick Voltage Reference

| Test Point | Expected | Meaning |
|-----------|----------|---------|
| PP1/PP2 → GND | 4.8–5.25V | Power reaching the board from USB |
| PP7 → GND | 4.8–5.25V | Power after polyfuse (should match PP1) |
| PP8 → GND | 3.3V ± 5% | Main logic rail (PAM2306 Ch.A or MxL7704) |
| PP9 → GND | 1.8V ± 5% | SoC core rail (PAM2306 Ch.B or MxL7704) |

If PP7 < PP1 by more than 0.2V under load: polyfuse is suspect or has high resistance.
