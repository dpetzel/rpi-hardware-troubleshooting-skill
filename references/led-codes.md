# LED Status Codes

## Standard LEDs

- **Red (PWR)**: Power present on the 5V rail. Should be steady.
- **Green (ACT)**: SD card / boot activity. Blinks during disk access.

## Green LED Blink Patterns (Boot Failure Codes)

These patterns indicate the bootrom detected an error. Count the blinks in each burst:

| Blinks | Meaning |
|--------|---------|
| 1 | start*.elf not found |
| 2 | start*.elf found but can't launch (corrupt?) |
| 3 | start*.elf launched but kernel not found |
| 4 | kernel launched but failed early (check UART) |
| 7 | Kernel image not valid |
| 8 | SDRAM not recognized (hardware failure) |

**Pattern format**: N blinks, pause, N blinks, pause (repeating)

## Pi 4/5 Specific

- **Solid green, no blink**: EEPROM issue — board stuck in bootrom
- **Rapid irregular green flashing**: Normal SD card activity (booting)
- **No green at all + red steady**: SD card not detected or empty

## Pi 5 Power LED

- **Solid**: Normal operation
- **Slow breathing/pulse**: In halt/standby state (press power button to wake)
- **Off**: No power or hard power off

## Diagnostic Tips

- Film the LED with your phone in slow-mo if blinks are too fast to count
- If green LED never blinks at all: the bootrom isn't finding anything to load
- On headless setups, the green LED pattern is your primary diagnostic tool
