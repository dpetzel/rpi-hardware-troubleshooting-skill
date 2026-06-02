# Power Issues Reference

## Power Requirements by Model

| Model | Recommended PSU | Connector | Notes |
|-------|----------------|-----------|-------|
| Pi 5 | 5V/5A (27W PD) | USB-C | 5V/3A works with peripheral limits; power button present |
| Pi 4B | 5V/3A (15W) | USB-C | Early revisions had USB-C e-marked cable issue |
| Pi 3B+ | 5V/2.5A | Micro-USB | PoE HAT available |
| Pi 3B | 5V/2.5A | Micro-USB | |
| Pi Zero 2W | 5V/2.5A | Micro-USB | Lower draw but same rail |
| Pi Zero/W | 5V/1.2A | Micro-USB | |
| Pi 400 | 5V/3A | USB-C | Built into keyboard form factor |

## Symptoms of Insufficient Power

- Lightning bolt icon on display (undervoltage detected)
- Random reboots under load
- USB devices disconnecting
- SD card corruption (writes fail during voltage dip)
- Wi-Fi/Bluetooth dropping
- Kernel throttling messages in dmesg

## Common Power Causes

1. **Inadequate supply** — Phone chargers often can't sustain rated current
2. **Bad cable** — Thin/long micro-USB cables drop voltage; measure at TP1/TP2 if possible
3. **Pi 4 USB-C issue** — First-run Pi 4 boards reject e-marked cables (fixed in rev 1.2+)
4. **GPIO backfeed** — Powering via GPIO 5V pins bypasses protection; must be exactly 5V
5. **PoE HAT issues** — Some PoE switches don't negotiate correctly
6. **Pi 5 power button** — Single press = soft shutdown, hold = hard off; can appear "dead" if in halt state
7. **Pi 5 PMIC component failure** — Damaged capacitors or PMIC itself; see
   [pi5_pmic_breakdown.md](pi5_pmic_breakdown.md) for component-level reference
8. **Pi 5 USB-C power MOSFET (G73) failure** — MOSFET between USB-C and 5V rail fails open/short; see
   [pi5_g73_breakout.md](pi5_g73_breakout.md)

## Diagnostic Steps

1. Is the red PWR LED lit? (No = no power reaching the board at all)
2. Is it steady or flickering? (Flicker = marginal supply)
3. Try a different cable first (cheapest swap)
4. Try a different supply rated for the model
5. If powering via GPIO: measure voltage across pins 2(5V) and 6(GND) — must be 4.8V–5.25V
6. Check `vcgencmd get_throttled` output if system boots (0x50005 = undervoltage occurred)
