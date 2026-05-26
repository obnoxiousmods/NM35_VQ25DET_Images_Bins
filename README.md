# NM35 VQ25DET Images & Bins

Repository for tuning information and premade ECU binary (.bin) files for the **VQ25DET** engine found in the **Nissan Stagea NM35 (WC34)**.

## Overview

This repo contains ECU ROM dumps, XML definition files, and premade tuned .bin files for the VQ25DET. Use these to flash your ECU with pre-configured performance tunes.

## Premade Tunes (3 Stages)

| Stage | Description | Status |
|-------|-------------|--------|
| **Stage 1** | KMPH speed limit raised from 180 → 300 km/h | ✅ Available |
| **Stage 2** | Stage 1 + Rev limit raised from 6600 → 6800 RPM, restore from 6300 → 6400 RPM | ✅ Available |
| **Stage 3** | Stage 1 + Stage 2 + Extra boost, increased throttle response, enhanced street driving tune | ✅ Available |

### Stage 1

- **File:** `nm35_vq25det_STAGE1.bin`
- Raises the factory 180 km/h speed limiter to 300 km/h
- All other parameters remain stock

### Stage 2

- **File:** `nm35_vq25det_STAGE2.bin`
- Stage 1 modifications +
- Rev limit increased from 6600 → 6800 RPM
- Fuel cut restore lowered from 6300 → 6400 RPM

### Stage 3

- **File:** `nm35_vq25det_STAGE3.bin`
- All Stage 1 & 2 modifications +
- Slightly increased boost pressure
- Improved throttle response
- Enhanced street driving characteristics
- Optimized fuel and ignition maps for daily drivability

## Folder Structure

```
├── Airflow/           # MAF and airflow-related maps
├── Fuel/              # Fuel compensation, injection multiplier, target AFR maps
├── Ignition Timing/   # Cold, high octane, and regular ignition trim maps
├── Limiters/          # Rev limit / fuel cut maps
├── Torque Management/ # Engine torque and torque conversion maps
├── Variable Cam Timing/ # Intake cam timing maps
├── refs/              # XML definition files and reference ROMs
└── *.bin              # Premade tuned ECU binaries
```

## Disclaimer

**Use at your own risk.** Tuning your ECU can cause engine damage if not done properly. Always verify tunes are compatible with your specific ECU part number before flashing. The authors assume no responsibility for any damage caused by using these files.

## Credits

- Tuning community of the VQ25DET / NM35 Stagea
- Nispro, Nistune, and RomRaider communities for tools and definitions
