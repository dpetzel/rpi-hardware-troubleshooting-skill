# Raspberry Pi 5 Recovery via rpiboot/usbboot

When a Pi 5 won't boot normally (corrupted EEPROM, bad firmware, inaccessible storage),
you can use `rpiboot` to access the board over USB from another computer.

**Official source:** https://github.com/raspberrypi/usbboot

## When to Use

- Board won't boot at all (no LED activity after EEPROM corruption)
- Need to reflash the bootloader EEPROM
- Need to access storage (SD/eMMC/NVMe) as a USB mass-storage device
- Boot error code 45 (fatal firmware error)
- SPI EEPROM errors (3 long + 1 short LED blinks)

## Entering rpiboot Mode on Pi 5

1. Disconnect USB-C power completely (shutdown alone is not sufficient)
2. Hold the power button down
3. While holding the power button, connect a USB-C cable from the Pi 5 to the host computer
4. Release the power button

The Pi 5 boot ROM will now enumerate as a USB device on the host.

## Host Setup

```bash
# Install dependencies (Debian/Ubuntu)
sudo apt install git libusb-1.0-0-dev pkg-config build-essential

# Clone and build
git clone --recurse-submodules --shallow-submodules --depth=1 https://github.com/raspberrypi/usbboot
cd usbboot
make
```

## Common Recovery Operations

### Access storage as USB mass-storage device

```bash
sudo ./rpiboot -d mass-storage-gadget
```

This boots a Linux initramfs on the Pi that exposes SD/eMMC/NVMe as USB drives on the host.
You can then use Raspberry Pi Imager to write a fresh OS image.

### Reflash the bootloader EEPROM

```bash
sudo ./rpiboot -d recovery5
```

This writes the default bootloader to the Pi 5 SPI EEPROM.

## Diagnostic Value

If `rpiboot` can successfully communicate with the Pi 5:
- The BCM2712 SoC is alive
- The USB-C port and boot ROM are functional
- The problem is likely in EEPROM, storage, or downstream components

If `rpiboot` gets no USB enumeration at all:
- The SoC may be dead
- Power delivery to the SoC may have failed
- Check 5V and 3.3V rails with a multimeter

## Notes

- The mass-storage-gadget also provides a console login via USB CDC-UART
- macOS and Windows are supported (see usbboot README for build instructions)
- Pi 4B and Pi 400 require a GPIO-based nRPIBOOT configuration (different procedure)
