# NM35 VQ25DET Images & Bins

Repository for tuning information and premade ECU binary (.bin) files for the **VQ25DET** engine found in the **Nissan Stagea NM35**.

> ⚠️ **IMPORTANT:** This repository is **ONLY for the NM35 Stagea (2001-2007, M35 platform).** It is **NOT for the WC34 Stagea (1996-2001, C34 platform).** The WC34 uses a completely different ECU (RB-series based), different wiring, and different ROM definitions. Flashing an NM35 .bin onto a WC34 ECU **will brick it.** Do not use these files on a WC34.

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

---

## Setup & Installation Guide

### Prerequisites

- **Windows** (7/8/10/11) or **Linux** (RomRaider is Java-based and cross-platform)
- **Java Runtime Environment (JRE)** — Java 8 or newer ([Adoptium](https://adoptium.net/) recommended)
- **RomRaider** — Free open-source ECU tuning & logging software ([romraider.com](https://www.romraider.com/))
- **A .bin file** from this repo matching your ECU
- **The XML definition file** for your ROM (`refs/nissandefs.xml` + your ECU's specific `.xml`)

### Step-by-Step: Installing RomRaider

1. **Download RomRaider** from [romraider.com/downloads](https://www.romraider.com/Download)
2. **Extract** the ZIP to a permanent folder (e.g., `C:\RomRaider\`)
3. **Run `romraider.jar`** — double-click or run `java -jar romraider.jar`
4. On first launch, you'll be prompted to set up your **ROM definitions**

### Step-by-Step: Loading XML Definitions

RomRaider uses `.xml` definition files to know how to read and edit each ECU ROM. This repo contains definitions in the `refs/` folder.

**Method 1: RomRaider Editor (recommended)**
1. Open RomRaider
2. Go to **Edit → ECU Definitions → ECU Editor**
3. Click **Add...** and browse to the `refs/` folder
4. Select **`nissandefs.xml`** (the master definition file)
5. Also add your specific ECU XML (e.g., `5Y016.xml`) if needed
6. Click **Apply** and restart RomRaider

**Method 2: Manual (if Method 1 doesn't work)**
1. Close RomRaider
2. Copy ALL `.xml` files from `refs/` into RomRaider's `definitions/` folder
3. Restart RomRaider

### Step-by-Step: Opening a .bin File

1. In RomRaider, go to **File → Open Image...**
2. Browse to the `.bin` file (e.g., `nm35_vq25det_STAGE1.bin`)
3. RomRaider will auto-detect the ROM ID and load all defined maps
4. You should see a tree of categories: **Ignition Timing, Fuel, Airflow, Limiters, Variable Cam Timing, etc.**
5. Double-click any map to view and edit it in a table or 3D graph view

### Verifying ROM Compatibility

⚠️ **CRITICAL:** Always verify your ECU part number matches before flashing:
1. In RomRaider, open your stock `.bin`
2. Check **ROM Info** (usually bottom panel) — verify HWID, ECU ID, and internal ID
3. Compare against the XML definition's expected `<romid>` tags
4. Never flash a .bin that doesn't match your ECU hardware — you can brick it

### Flashing the ECU

Flashing requires additional hardware:
- **Nisprog** (open-source Nissan flasher) — supports SH7055/SH7058/SH7059 processors
- **Nistune** (commercial tuning suite) — also supports VQ ECUs
- **K-Line / consult cable** (FTDI-based, e.g., VAG-COM KKL cable)

> Consult the Nisprog and Nistune documentation for flash procedures specific to the VQ25DET ECU.

---

## XML Definition File Reference — All Available Maps

This section explains every map category and table defined in `refs/nissandefs.xml` and the individual ROM XMLs, with stock values from the NM35 VQ25DET ECU.

### Scaling Conventions Used Across All Maps

| Scaling | Formula | Used For |
|---------|---------|----------|
| `byte_%` | `x × 0.78125` | Load axis (cylinder filling efficiency %) |
| `RPMx50` | `x × 50` | Engine speed axis (RPM) |
| `RPMx12` | `x × 12.5` | Rev limiter RPM values |
| `AFR_Gas` | `1881.6 / x` | Gasoline AFR (14.7 = stoich) |
| `Lambda` | `128 / x` | Lambda (1.0 = stoich) |
| `VehicleSpeedkmh` | `x × 0.1` | Speed in km/h |
| `VehicleSpeedMPH` | `x × 0.06215` | Speed in MPH |

---

### 🔥 Ignition Timing Maps

#### High Octane Ignition Trim (16×16)
- **Category:** Ignition Timing
- **X-Axis:** Cylinder filling efficiency (12.5% → 199.2%)
- **Y-Axis:** Engine speed (400 → 6400 RPM)
- **Units:** Degrees advance (positive = more advance)
- **Stock range:** −11° to +137° advance

| RPM ↓ / Load → | 12.5% | 50% | 87.5% | 125% | 162.5% | 199.2% |
|-----------------|-------|-----|-------|------|--------|--------|
| 400 | 6° | 4° | −5° | −9° | −6° | −6° |
| 800 | 0° | 1° | 133° | 128° | 118° | 116° |
| 2000 | −3° | 0° | 129° | 128° | 127° | 114° |
| 3200 | 2° | 0° | 131° | 129° | 131° | 127° |
| 4000 | 2° | 2° | 134° | 130° | 134° | 129° |
| 4800 | 0° | −3° | −8° | −9° | −7° | −11° |
| 5600 | −2° | −6° | −2° | −1° | 2° | −5° |
| 6400 | −2° | −3° | 2° | 0° | 4° | −5° |

> **Safe range:** Low-load cruising can be advanced to improve efficiency (up to 140°). High-load/RPM should be conservative — knock is most destructive here. Stay under 135° at WOT above 4000 RPM unless on high-octane fuel (98 RON+).

#### Regular Ignition Trim (16×16)
- **Category:** Ignition Timing
- **X-Axis:** Cylinder filling efficiency (12.5% → 199.2%)
- **Y-Axis:** Engine speed (400 → 6400 RPM)
- **Stock range:** −18° to +129°

> ⚠️ **Rule:** Regular table must ALWAYS have lower values than High Octane table at every cell. The ECU interpolates between them based on knock sensor feedback. Stock Regular table runs 3-10° less advance than High Octane at peak torque.

#### Cold Ignition Trim (16×16)
- **Category:** Ignition Timing
- **Stock range:** −9° to +137°
- **Purpose:** Applied when coolant temp is below threshold (~70°C). Extra advance in mid-RPM to warm up faster.
- **Safe note:** This table can be cloned from the High Octane table; the stock one already adds 1-5° extra at mid-RPM for cold enrichment compensation.

#### Knock Slice Table (16×8)
- **Category:** Knock Detection
- **X-Axis:** RPM (400 → 6400)
- **Y-Axis:** Cylinder (1-8)
- **Units:** dB threshold for knock detection
- **Purpose:** Sets per-cylinder knock sensitivity. Higher values = less sensitive (requires louder knock to register).
- **Safe range:** Don't raise more than 10-15% from stock. The VQ is a noisy engine — raising thresholds too much risks missing real knock events.

#### Knock Limit Tables (3×3)
- **High Octane / Regular Knock Limit**
- **Units:** Degrees retard allowed by knock control
- **Safe note:** Stock limits are conservative. Raising allows the ECU to pull more timing if knock is detected. Don't exceed 12° of retard authority.

#### Other Ignition Tables
| Map | Purpose |
|-----|---------|
| Cranking Advance | Timing during engine start, referenced by coolant temp |
| Ignition Timing Retard Limit | Maximum retard authority for timing |
| Fuel Cut Ignition Retard | Timing pull during fuel-cut decel |
| Idle Timing (GOV/GOV3/Clutch/Lean) | Idle stability tables — rarely need changes |
| MBT Flame Speed (4×4) | Advanced: models flame propagation for MBT calculation |
| Fresh Air Rate (4×4) | Advanced: fresh air/EGR rate map |
| Dwell RPM / Dwell Battery | Coil dwell time vs RPM and battery voltage — don't touch |

---

### ⛽ Fuel Maps

#### Fuel Target 16×16
- **Category:** Fuel
- **X-Axis:** Cylinder filling efficiency (25% → 199.2%)
- **Y-Axis:** Engine speed (800 → 6800 RPM)
- **Units:** AFR (Gasoline scale, 14.7 = stoich)
- **Stock range:** 10.17 → 14.7 AFR

| RPM ↓ / Load → | 25% | 62.5% | 87.5% | 125% | 162.5% | 199.2% |
|-----------------|-----|-------|-------|------|--------|--------|
| 800 | 14.7 | 14.7 | 13.25 | 13.25 | 13.25 | 13.25 |
| 2000 | 14.7 | 14.7 | 14.7 | 13.25 | 13.25 | 13.25 |
| 3200 | 14.7 | 14.7 | 14.7 | 13.07 | 11.47 | 11.47 |
| 4000 | 14.7 | 14.7 | 14.7 | 12.71 | 11.47 | 11.13 |
| 4800 | 14.7 | 14.7 | 13.34 | 12.30 | 10.88 | 10.88 |
| 5600 | 14.7 | 13.25 | 12.80 | 11.33 | 10.69 | 10.51 |
| 6800 | 14.7 | 12.38 | 11.54 | 10.40 | 10.17 | 10.17 |

> **Safe range:** 
> - **Idle/Cruise (low load):** 14.7 (stoich) — don't lean beyond this on pump gas
> - **Mid-load spool:** 13.0-13.5 — safe enrichment for turbo spool
> - **WOT peak torque:** 11.0-11.5 — rich enough to suppress knock on a turbo engine
> - **WOT redline:** 10.5-10.8 — extra enrichment for cooling and knock suppression
> - **Never leaner than 12.5 at WOT on a turbo VQ25DET** — piston damage risk
> - Lambda 0.75-0.78 (≈11.0-11.5 AFR gas) is the sweet spot for turbo torque vs safety

#### Fuel Compensation (16×16 — Warm)
- **Category:** Fuel
- **X-Axis:** TVO-N flow quantity (6.25 → 81.25)
- **Y-Axis:** Engine speed (400 → 6400 RPM)
- **Units:** % enrichment (100 = no change, >100 = richer, <100 = leaner)
- **Stock range:** 100.0% → 109.4%

> This map fine-trims the base fuel calculation. Values around 102-108% are normal factory enrichment. The stock map is already well calibrated — only adjust if AFR logging shows consistent deviation from the Fuel Target table.

#### Fuel Compensation Cold (16×16)
- **Category:** Fuel
- **Same axes as warm compensation**
- **Stock range:** 84.4% → 116.4%
- **Purpose:** Applied when coolant < ~70°C. Adds up to 16% extra fuel at high load when cold. Some cells lean out slightly (down to 84%) to prevent cold-start flooding.

#### Fuel Injection Multiplier (K Value)
- **Category:** Fuel
- **Stock value:** **27284.0**
- **Purpose:** Global fuel scaling constant. This is THE most important number for fuel tuning.
- **Scaling:** Higher K = richer globally, Lower K = leaner globally
- **When to adjust:** After changing injectors, MAF housing, or fuel pressure. Adjust this first before touching the compensation maps.

> 💡 **K Value math:** If swapping from stock 300cc → 440cc injectors: New K = 27284 × (300/440) = **18603**. Always verify with wideband logging.

---

### 🌬️ Airflow (MAF) Maps

#### MAF Sensor Calibration (64-point 2D)
- **Category:** Airflow
- **X-Axis:** MAF voltage (0.00V → 4.92V)
- **Y-Axis:** Airflow (raw units, scales with MAF housing size)
- **Stock range:** 0 → 62,385

| Voltage | Airflow | Voltage | Airflow | Voltage | Airflow |
|---------|---------|---------|---------|---------|---------|
| 0.00V | 0 | 1.64V | 4,947 | 3.44V | 18,753 |
| 0.47V | 1,701 | 2.03V | 6,539 | 3.91V | 27,674 |
| 0.94V | 2,860 | 2.50V | 8,850 | 4.30V | 38,722 |
| 1.25V | 3,790 | 2.97V | 12,043 | 4.92V | 62,385 |

> ⚠️ **Critical:** This table must match your MAF sensor and housing. If you change intake piping or MAF housing diameter, you MUST rescale this table. Wrong MAF scaling = wrong AFR everywhere.

#### MAF Factor
- **Category:** Airflow
- **Purpose:** Global MAF scaling multiplier — Nissan's equivalent of MAF scaling without touching the 64-point table. Adjust this for small intake changes.

#### MAF Voltage Limits
- **MAF voltage limits:** Expected voltage range (upper/lower). Triggers DTC if voltage goes outside bounds.
- **MAF idle voltage limits:** Voltage range at idle (USRV).
- **MAF low voltage range:** Acceptable low-range voltage bounds.

---

### 🚫 Limiter Maps

#### Rev Limit (Fuel Cut — 3D)
- **Category:** Limiters
- **Stock values (NM35 VQ25DET):**

| State | Fuel Cut (On) | Restore (Off) |
|-------|---------------|---------------|
| Normal (forward) | **6600 RPM** | **6300 RPM** |
| Reverse | 5000 RPM | 4600 RPM |

- **Stage 2/3 change:** Forward cut raised to 6800 RPM, restore to 6400 RPM
- **Stage 1:** Stock rev limits (unchanged)

> **Safe range:** 
> - VQ25DET stock bottom end: **7200 RPM max recommended** (rod bolts are the weak point)
> - VQ35DE oil pump gears tend to shatter above 7200 — VQ25DET may share this vulnerability
> - **Conservative:** 6800-7000 RPM fuel cut
> - **Aggressive (built engine):** 7200-7400 RPM
> - Always keep at least 200-300 RPM gap between cut and restore to prevent oscillation

#### Speed Limiter
- **Stock:** 180 km/h (JDM legal requirement)
- **Stage 1/2/3:** Raised to 300 km/h (effectively disabled)
- **Category:** Stored in ECU as a scalar or part of a speed limiter table

---

### 🔧 Variable Cam Timing (VCT/VTC)

#### Intake Cam Timing (16×16)
- **Category:** Variable Cam Timing
- **X-Axis:** Cylinder filling efficiency (25% → 162.5%)
- **Y-Axis:** Engine speed (400 → 6400 RPM)
- **Units:** Degrees advance (0-31°)
- **Stock behavior:**
  - **Idle/low RPM:** 0° advance (no cam advance)
  - **Mid-RPM (1200-4400):** Up to 31° advance — peak around 1600-3600 RPM at high load
  - **High RPM (>4400):** Tapers down to 5-15° then 0° above 5600 RPM

> **Safe range:**
> - The VQ25DET intake cam has ~31° of authority
> - Advancing shifts the power band DOWN (more mid-range torque)
> - Retarding shifts power UP (more top-end)
> - Piston-to-valve clearance is not an issue within the 31° range (OEM hardware)
> - **Street tune:** Keep peak advance in the 2000-4000 RPM range for driveability
> - **Don't run more than 25° advance above 5000 RPM** — pump inefficiency and power loss

#### Intake Cam Max Target
- **Purpose:** Hard limit on VTC advance angle — don't exceed this value in any cell of the timing map.

#### Exhaust Cam Timing (16×16)
- **Category:** Variable Cam Timing
- **Units:** Degrees retard
- **Note:** Only applicable if your VQ25DET has exhaust VTC (some JDM models do).

---

### ⚡ Torque Management Maps

#### QH0 Torque Conversion Map (16×16)
- **Category:** Torque Management
- **X-Axis:** QH0 Ratio (0.0 → ~100.0)
- **Y-Axis:** Engine speed (0 → 6000 RPM)
- **Units:** Torque (N·m)
- **Stock range:** −114.3 → +381.9 N·m

> These maps convert between the ECU's internal QH0 (torque request) value and actual engine torque. They must match between Map 1 and Map 2. The ECU uses these for:
> - AT shift pressure calculation
> - Traction control intervention
> - Throttle mapping during torque reduction events
> 
> **Tuning note:** If you increase boost, update the high-load cells to reflect the new torque output. Otherwise the TCU may apply wrong shift pressures.

#### Engine Torque Value Map
- **Currently empty/unused** in this ROM — present in some VQ variants.

---

### 🔬 Additional Maps in XML Definitions

| Map | Category | Purpose |
|-----|----------|---------|
| Fuel Target (8×8) / Warm | Fuel | Lower-resolution alternate AFR target |
| Regular Fuel Compensation | Fuel | Lambda compensation per RPM on regular fuel |
| Fuel Correction Limits % | Fuel | LTFT (Long Term Fuel Trim) authority range |
| Idle BFS Limits (ms) | Fuel | Base fuel schedule limits at idle |
| Learning Map TP/N Lattice | Fuel | ECU self-learning fuel trim reference points |
| Cranking Advance | Starting | Ignition timing during engine crank |
| Ignition Timing Retard Limit | Ignition | Global max retard authority |
| Fuel Cut Ignition Retard | Ignition | How much timing to pull on decel fuel cut |
| Idle Timing maps (GOV/GOV3/Clutch/Lean) | Ignition | Idle speed stability via timing |
| MBT Flame Speed | Ignition (Advanced) | Flame kernel propagation speed model |
| Fresh Air Rate | Ignition (Advanced) | Internal EGR/residual gas model |
| Dwell RPM / Dwell Battery | Ignition (Advanced) | Coil charging time maps |
| Intake Cam Timing Cold (8×8) | VCT | Cold-engine VTC map |
| Exhaust Cam Timing | VCT | Exhaust cam retard map (if equipped) |
| MAF Offset | Airflow | Subtractive MAF correction |
| OBD-II DTC tables | Diagnostics | BitwiseSwitch tables for disabling/setting DTCs |
| VIAS Solenoid Control | Intake | Variable intake air system solenoid |

---

## Map Audit — Key Ratios, Variables & Constants

### Engine Specifications (VQ25DET — NM35 Stagea)

| Parameter | Stock Value | Notes |
|-----------|-------------|-------|
| Engine code | VQ25DET | 2.5L V6 Turbo |
| Displacement | 2495 cc | Bore 85mm × Stroke 73.3mm |
| Compression ratio | 8.5:1 | Low compression — boost-friendly |
| Stock turbo | Garrett M24N | Single turbo, internally wastegated |
| Stock boost | ~0.6-0.7 bar (9-10 psi) | Mechanical wastegate |
| Injectors | ~300cc (estimated) | Side-feed, high impedance |
| MAF sensor | Nissan OEM (65mm housing) | Pull-type hotwire |
| ECU processor | SH7055 or SH7058 | Renesas SuperH, 512KB-1MB flash |
| Fuel pressure | 3.0 bar (43.5 psi) | Vacuum-referenced FPR |
| Stock power | 206 kW (276 hp) @ 6400 | JDM "gentlemen's agreement" rating |
| Stock torque | 407 N·m (300 lb-ft) @ 2800 | |

### Critical Scaling Ratios

| Parameter | Formula | Example |
|-----------|---------|---------|
| Injector scaling | `New K = Old K × (Old cc / New cc)` | 300→440cc: `27284 × 0.682` |
| MAF housing scaling | `New airflow = Old × (New dia² / Old dia²)` | 65→76mm: airflow × 1.367 |
| Fuel pressure scaling | `New flow = Old × √(New psi / Old psi)` | 43.5→50 psi: injectors × 1.072 |
| Lambda → AFR (gas) | `AFR = Lambda × 14.7` | 0.78λ = 11.47 AFR |
| Byte → Load % | `Load = byte × 0.78125` | byte 200 = 156.25% load |
| RPM byte → RPM | `RPM = byte × 50` | byte 128 = 6400 RPM |

### Torque Model Safety Limits

| Component | Conservative Limit | Aggressive Limit | Failure Mode |
|-----------|-------------------|------------------|--------------|
| Rod bolts (stock) | 6800 RPM | 7200 RPM | Rod bolt stretch → spun bearing |
| Oil pump gears | 7000 RPM | 7200 RPM | Gear shatter → instant oil pressure loss |
| Stock turbo (M24N) | 12 psi | 14 psi | Overspeed → shaft failure |
| Stock head gasket | 12 psi / ~350 hp | 15 psi / ~400 hp | Lifting under high boost |
| Stock pistons/rings | 350 hp | 400 hp | Ring land failure under detonation |
| 5AT transmission (RE5R05A) | 350 N·m input | 400 N·m input | Clutch pack slip, valve body wear |

---

## Safe Tuning Numbers — Research Summary

### Ignition Timing Safety

| Condition | Safe Advance | Aggressive | Notes |
|-----------|-------------|------------|-------|
| Idle (800 RPM) | 10-15° | 15-20° | Smooth idle |
| Cruise (2000-3000 RPM, low load) | 35-45° | 45-50° | Fuel economy gains here |
| Turbo spool (2500-3500 RPM, >75% load) | 10-20° | 15-25° | Knock-prone region — conservative! |
| Peak torque (3800-4800 RPM, WOT) | 8-15° | 12-18° | Highest cylinder pressure |
| Redline (6000-6800 RPM, WOT) | 15-25° | 20-30° | Piston speed high, timing naturally advances |

> 🔑 **Golden rule:** Every 1° of extra timing at peak torque on a turbo engine is worth ~5-8 hp but carries heavy knock risk. Add timing 1° at a time, log knock, and back off 2° at the first sign of knock.

### AFR Safety (Gasoline Scale)

| Condition | Safe AFR | Rich Limit | Lean Limit | Notes |
|-----------|----------|------------|------------|-------|
| Idle | 14.0-14.7 | 13.5 | 15.0 | Leaner = rougher idle |
| Cruise (low load) | 14.5-14.7 | 14.0 | 15.5 | Stoich for emissions/economy |
| Light acceleration | 13.5-14.0 | 13.0 | 14.7 | Transition zone |
| Turbo spool | 12.5-13.2 | 12.0 | 13.5 | Rich helps spool |
| WOT peak torque | 11.0-11.5 | 10.5 | 11.8 | Rich for knock suppression |
| WOT redline | 10.8-11.2 | 10.3 | 11.5 | Cooling + knock safety |

### Boost Safety (Stock M24N Turbo)

| Level | Boost (psi/bar) | Supporting Mods Needed |
|-------|-----------------|----------------------|
| **Stage 3 (street)** | 10-12 psi (0.7-0.8 bar) | Intercooler recommended, 98 RON fuel |
| **Stage 3+ (mild)** | 12-14 psi (0.8-1.0 bar) | FMIC, 98 RON, colder plugs (NGK #7) |
| **Aggressive** | 14-16 psi (1.0-1.1 bar) | Built engine, upgraded turbo, E85 |
| **Stock turbo limit** | ~14 psi sustained | M24N overspeeds beyond this |

### Rev Limit Safety

| Engine State | Recommended Limit |
|--------------|-------------------|
| Stock bottom end (safe) | 6800 RPM fuel cut / 6400 restore |
| Stock bottom end (limit) | 7000 RPM fuel cut / 6700 restore |
| Built engine (rods + bolts) | 7400+ RPM |

---

## Credits

- Tuning community of the VQ25DET / NM35 Stagea
- Nispro, Nistune, and RomRaider communities for tools and definitions
