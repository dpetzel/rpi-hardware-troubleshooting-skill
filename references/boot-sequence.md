# Boot Sequence & Diagnostics

## Boot Order (All Models)

1. GPU/bootrom powers on, reads bootcode from SoC ROM (Pi 4/5) or SD card (older)
2. Loads `start.elf` (or `start4.elf` for Pi 4) from boot partition
3. Reads `config.txt` and `cmdline.txt`
4. Loads kernel (`kernel8.img` for 64-bit, `kernel7l.img` for Pi 4 32-bit)
5. Kernel initializes, mounts root filesystem
6. Init system starts (systemd on modern Raspberry Pi OS)

## Model-Specific Boot Behavior

### Pi 5
- Has dedicated boot EEPROM (like Pi 4) — can boot from SD, USB, NVMe, network
- Power button: press to wake from halt, hold 10s for hard power off
- HDMI diagnostics display on boot failure (colored screen or error text)

### Pi 4
- Boot EEPROM (updatable) — check with `rpi-eeprom-update`
- Can boot from SD, USB mass storage, or network (after EEPROM update)
- Stuck on green screen = start4.elf not found or corrupt

### Pi 3 and earlier
- Boot entirely from SD card (no onboard EEPROM)
- No SD card = no boot attempt, no activity LED blink
- Rainbow screen = GPU started but can't find kernel

### Pi Zero / Zero 2W
- Same as Pi 3 boot flow
- No onboard activity LED on original Zero (Zero 2W has one)

## Failure Symptoms by Stage

| Symptom | Likely Stage | Check |
|---------|-------------|-------|
| No LEDs at all | Pre-boot / power | Power supply, cable |
| Red LED only, no green blink | Bootrom can't read SD | SD card, boot partition |
| Green LED blinks in pattern | Bootrom error code | See led-codes.md |
| Rainbow/colored screen | GPU up, kernel not found | kernel*.img on boot partition |
| Four raspberries, then black | Kernel panic early | cmdline.txt root= path, filesystem |
| Boots to login then freezes | Late userspace | Likely thermal or SD corruption |

## EEPROM Issues (Pi 4/5)

- Corrupt EEPROM = board won't boot from any media
- Recovery: use rpi-eeprom-recovery image on SD card
- Symptoms: solid green LED (Pi 4), no display output, no USB activity
- Update via: `sudo rpi-eeprom-update -a` then reboot
