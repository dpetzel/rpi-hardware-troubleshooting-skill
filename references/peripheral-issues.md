# Peripheral & Interface Issues

## USB

### Common Problems
- **Insufficient power for devices**: Pi can supply limited current per port; use powered hub for drives
- **Pi 4 USB 3.0 interference**: USB 3.0 devices can interfere with 2.4GHz Wi-Fi/Bluetooth (shielding helps)
- **USB boot not working**: Pi 4/5 need EEPROM configured for USB boot; Pi 3B+ supports it natively

### Diagnostic Steps
1. Does the device work on another computer?
2. Try a different port (USB 2 vs USB 3 on Pi 4/5)
3. Try with powered hub
4. Check `lsusb` — device enumerated?
5. Check `dmesg | tail` for connection/disconnect spam (power issue sign)

## GPIO

### Common Problems
- **Pin damage from overvoltage**: GPIO pins are 3.3V only — 5V will damage them permanently
- **Floating inputs**: Unconnected input pins read random values; use pull-up/pull-down
- **Kernel overlay not loaded**: Many interfaces (I2C, SPI, 1-Wire) need dtoverlay in config.txt

### Diagnostic Steps
1. Confirm pin numbering (BCM vs physical board pin)
2. Check if interface is enabled: `raspi-config` or `/boot/config.txt`
3. Test with simple LED + resistor before complex circuits
4. Use `gpio readall` or `pinctrl` (Pi 5) to check pin states

## Display (HDMI)

### Common Problems
- **No output**: HDMI must be connected at boot (hotplug unreliable on older models)
- **Resolution wrong**: Force with `hdmi_group` and `hdmi_mode` in config.txt
- **Pi 4/5 dual HDMI**: Primary output is HDMI0 (nearest USB-C port on Pi 4)
- **Blank screen with Pi 5**: Check if in standby (power LED breathing)

### Diagnostic Steps
1. Try a different HDMI cable
2. Try a different display/TV
3. Connect HDMI before powering on
4. Add `hdmi_force_hotplug=1` to config.txt (if you can access SD card)

## Camera (CSI)

### Common Problems
- **Ribbon cable orientation**: Blue side faces ethernet port (Pi 4) or away from board (varies)
- **Cable not seated**: Must click into connector; pull up latch, insert, push latch down
- **Legacy vs libcamera**: Pi OS Bullseye+ uses libcamera; `raspistill` no longer works

### Diagnostic Steps
1. Reseat ribbon cable (most common fix)
2. Check `dtoverlay=camera` or `start_x=1` in config.txt (legacy) vs auto-detect (Bookworm)
3. `libcamera-hello` for quick test
4. `vcgencmd get_camera` — shows detected/supported

## Networking

### Wi-Fi
- **Country not set**: Wi-Fi won't activate without regulatory domain (`raspi-config` > Localisation)
- **5GHz not connecting**: Not all models support 5GHz; Pi 3B is 2.4GHz only
- **Interference from USB 3**: Move USB 3 devices away or use USB 2 ports

### Ethernet
- **No link LED on port**: Try different cable, check switch port
- **Gigabit not negotiating**: Bad cable (needs all 8 wires for gigabit)
- **Pi 3B+ ethernet slow**: Known issue with some switches; force speed with ethtool

## Audio

- **No HDMI audio**: Set output with `raspi-config` or `amixer`
- **3.5mm jack noise**: PWM-driven, inherently noisy; use USB or I2S DAC for quality
- **Bluetooth audio choppy**: Bandwidth shared with Wi-Fi; reduce Wi-Fi load or use 5GHz
