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

### Pi 3 Model B+
- Boot entirely from SD card (no onboard EEPROM)
- **USB boot supported natively** — no OTP bit required; will attempt USB after SD fails
- Boot order: SD card → USB mass storage (built-in)
- No SD card inserted and no bootable USB = no boot attempt, no activity LED blink
- Rainbow screen = GPU started but can't find kernel

### Pi 3 Model B (non-plus)
- Boot entirely from SD card by default (no onboard EEPROM)
- **USB boot requires OTP bit** — one-time permanent fuse blow:
  1. Boot from SD card with Raspberry Pi OS
  2. Add `program_usb_boot_mode=1` to `/boot/config.txt`
  3. Reboot (this programs the OTP bit irreversibly)
  4. Verify: `vcgencmd otp_dump | grep 17:` should show `3020000a`
  5. SD card can now be removed; board will try USB boot
- **OTP is irreversible** — cannot be undone. Board will always attempt USB boot after SD.
- No SD card (and OTP not set) = no boot attempt whatsoever, no LED blinks
- Rainbow screen = GPU started but can't find kernel

### Pi 3 RUN Pad Reset
- PP21 (RUN) can be used to reset the SoC without power cycling
- Useful for hung boards where power LED is on but system is unresponsive
- Brief short of PP21 to GND = hardware reset

### Pi 3 and earlier (general)
- All models boot from GPU first — the ARM CPU is secondary
- Boot code comes from SD card (`bootcode.bin` on the boot partition)
- No onboard EEPROM means no boot diagnostics without a valid SD card present

### Pi Zero / Zero 2W
- Same as Pi 3 boot flow
- No onboard activity LED on original Zero (Zero 2W has one)

## Failure Symptoms by Stage

| Symptom | Likely Stage | Check |
|---------|-------------|-------|
| No LEDs at all | Pre-boot / power | Power supply, cable |
| Red LED only, no green blink (Pi 3) | SoC not executing | Crystal dead, or no SD card (no EEPROM to provide blink codes) |
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
