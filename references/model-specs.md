# Raspberry Pi Model Specifications (Hardware-Relevant)

## Pi 5 (2023)
- SoC: BCM2712 (Cortex-A76 quad-core, 2.4GHz)
- RAM: 4GB / 8GB LPDDR4X
- Power: USB-C, 5V/5A recommended (PD negotiation), power button
- Storage: microSD, PCIe 2.0 x1 (via HAT for NVMe)
- USB: 2x USB 3.0, 2x USB 2.0
- Display: 2x micro-HDMI (4Kp60)
- Boot: Onboard EEPROM, supports SD/USB/NVMe/network
- Notable: Real-time clock (needs battery), dedicated UART connector, PCIe

## Pi 4 Model B (2019)
- SoC: BCM2711 (Cortex-A72 quad-core, 1.5GHz/1.8GHz)
- RAM: 1GB / 2GB / 4GB / 8GB LPDDR4
- Power: USB-C, 5V/3A (early boards reject e-marked cables)
- Storage: microSD
- USB: 2x USB 3.0, 2x USB 2.0
- Display: 2x micro-HDMI (4Kp60 + 4Kp30, or dual 4Kp30)
- Boot: Onboard EEPROM (updatable), supports SD/USB/network
- Notable: Runs hot under load (heatsink/fan recommended), USB-C power bug on rev 1.1

## Pi 3 Model B+ (2018)
- SoC: BCM2837B0 (Cortex-A53 quad-core, 1.4GHz)
- RAM: 1GB LPDDR2
- Power: Micro-USB, 5V/2.5A
- USB: 4x USB 2.0
- Display: Full-size HDMI (1080p60)
- Network: Gigabit ethernet (via USB 2.0, capped ~300Mbps), dual-band Wi-Fi
- Boot: SD card primary, USB boot supported natively
- Notable: Metal-lid SoC package for better thermals

## Pi 3 Model B (2016)
- SoC: BCM2837 (Cortex-A53 quad-core, 1.2GHz)
- RAM: 1GB LPDDR2
- Power: Micro-USB, 5V/2.5A
- USB: 4x USB 2.0
- Display: Full-size HDMI
- Network: 10/100 ethernet, 2.4GHz Wi-Fi only
- Boot: SD card (USB boot possible with OTP bit set)

## Pi Zero 2 W (2021)
- SoC: RP3A0 (Cortex-A53 quad-core, 1GHz)
- RAM: 512MB LPDDR2
- Power: Micro-USB, 5V/2.5A
- USB: 1x micro-USB OTG
- Display: Mini-HDMI
- Network: 2.4GHz Wi-Fi, Bluetooth 4.2
- Notable: Same footprint as original Zero, has activity LED

## Pi Zero / Zero W (2015/2017)
- SoC: BCM2835 (ARM1176, single-core, 1GHz)
- RAM: 512MB
- Power: Micro-USB, 5V/1.2A
- USB: 1x micro-USB OTG
- Display: Mini-HDMI
- Network: None (Zero) / 2.4GHz Wi-Fi + BT 4.1 (Zero W)
- Notable: No activity LED on original Zero, no ethernet

## Pi 400 (2020)
- SoC: BCM2711 (same as Pi 4, clocked at 1.8GHz)
- RAM: 4GB LPDDR4
- Power: USB-C, 5V/3A
- Form factor: Built into keyboard
- Notable: Better thermals than Pi 4 (keyboard acts as heatsink), no CSI/DSI

## Common Hardware Failure Modes by Model

| Model | Known Issue | Symptom |
|-------|------------|---------|
| Pi 5 | Power button confusion | Appears dead (actually in halt) |
| Pi 4 (rev 1.1) | USB-C e-marked cable rejection | No power with some cables |
| Pi 4 | Thermal throttling | Slowdowns under sustained load |
| Pi 4 | EEPROM corruption | Won't boot from any media |
| Pi 3B+ | USB/ethernet hub chip failure | All USB + ethernet dead simultaneously |
| Pi 3 | SD card slot wear | Intermittent boot failures |
| Pi Zero | Micro-USB port fragility | Loose connection, intermittent power |
