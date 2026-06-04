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
2. **Bad cable** — Thin/long micro-USB cables drop voltage; measure at PP1/PP2 vs PP7 (Pi 3) or TP1/TP2 (older)
3. **Pi 4 USB-C issue** — First-run Pi 4 boards reject e-marked cables (fixed in rev 1.2+)
4. **GPIO backfeed** — Powering via GPIO 5V pins bypasses protection; must be exactly 5V
5. **PoE HAT issues** — Some PoE switches don't negotiate correctly
6. **Pi 5 power button** — Single press = soft shutdown, hold = hard off; can appear "dead" if in halt state
7. **Pi 5 PMIC component failure** — Damaged capacitors or PMIC itself; see [pi5_pmic_breakdown.md](pi5_pmic_breakdown.md) for component-level reference
8. **Pi 3B+ MxL7704 PMIC failure** — 5V-to-3.3V GPIO short destroys PMIC, permanently shorts 3.3V rail to GND; see
   [pi3_power_management.md](pi3_power_management.md)
9. **Pi 3B PAM2306 regulator failure** — Overvoltage or downstream short kills 3.3V/1.8V rails; see
    [pi3_power_management.md](pi3_power_management.md)
10. **Pi 3 polyfuse (F1)** — Trips under overcurrent, can take minutes-to-hours to reset; board appears completely dead (no red LED). Located on PCB underside near micro-USB connector.
11. **Pi 3 micro-USB connector fatigue** — Mechanical wear causes intermittent power; wiggle cable while watching red LED to diagnose. Common on used/refurb boards.
12. **Pi 3 LAN9514 dragging 3.3V rail** — Failed USB/Ethernet chip can short the 3.3V rail, causing board to appear dead even though 5V is present; see [pi3_lan9514.md](pi3_lan9514.md)

## Diagnostic Steps

1. Is the red PWR LED lit? (No = no power reaching the board at all)
2. Is it steady or flickering? (Flicker = marginal supply)
3. Try a different cable first (cheapest swap)
4. Try a different supply rated for the model
5. If powering via GPIO: measure voltage across pins 2(5V) and 6(GND) — must be 4.8V–5.25V
6. Check `vcgencmd get_throttled` output if system boots (0x50005 = undervoltage occurred)
