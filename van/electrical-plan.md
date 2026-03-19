# VAN ELECTRICAL SYSTEM PLAN

**2008 Ford Transit Bus - Complete 12V System Design**

**System Capacity:** 400Ah LiFePO4 (2x 200Ah) @ 12.8V = 5,120Wh usable

---

## Table of Contents

1. [System Overview](#system-overview)
2. [Current System Inventory](#current-system-inventory)
3. [Components to Install](#components-to-install)
4. [Load Calculations](#load-calculations)
5. [Solar Array Design](#solar-array-design)
6. [Battery Configuration](#battery-configuration)
7. [Wire Sizing Chart](#wire-sizing-chart)
8. [Master Wiring Diagram](#master-wiring-diagram)
9. [Fuse Block Layout](#fuse-block-layout)
10. [Component Placement](#component-placement)
11. [Installation Sequence](#installation-sequence)
12. [Testing Checklist](#testing-checklist)

---

## System Overview

**Design Philosophy: Battery-Centric**
- All loads run from batteries (no transfer switch)
- Shore power charges batteries only
- Batteries act as buffer/UPS
- Simple, reliable, easy to troubleshoot

**Power Sources:**
- Solar: 520W (2x 260W panels in parallel)
- Alternator: 50A DC-DC charger
- Shore Power: 25A AC charger (~320W 12V equivalent)

**Total Daily Generation Potential:**
- Solar (5 sun hours): ~2,600Wh
- Driving (2 hours): ~1,280Wh
- Shore Power (overnight 8hr): ~2,560Wh

---

## Current System Inventory

### Installed Components

| Component | Specs | Location | Connection Status |
|-----------|-------|----------|-------------------|
| Battery #1 | 200Ah LiFePO4 | Floor, behind driver seat | Connected to bus bars |
| Positive Bus Bar | - | Wall behind driver seat | Live |
| Negative Bus Bar | - | Wall behind driver seat | Live |
| Renogy 50A DCDC | With MPPT, 50V/660W max | Wall behind driver seat | Connected to starter battery, solar, bus bars |
| 1500W Inverter | 12V DC to 240V AC, 3000W surge | Wall behind driver seat | Connected to bus bar (NO FUSE YET) |
| 12V Power Hub | Cigarette lighter outputs | TBD | Anderson to bus bars, inline fuse |
| Dometic Fridge | 12V top-opening | TBD | Will connect to 12V hub |
| Solar Panels | 4x 260W (30.6V, 8.5A) | Not mounted | 2 panels connected to DCDC |

**Critical Notes:**
- ⚠️ **Inverter needs fuse immediately** (150A ANL fuse)
- ⚠️ **Solar limited to 2 panels** with current DCDC (50V/660W max)
- 2 spare panels available for future upgrade

---

## Components to Install

### Priority 1: Safety & Monitoring

| Component | Purpose | Specs/Model | Priority |
|-----------|---------|-------------|----------|
| Battery #2 | Double capacity to 400Ah | 200Ah LiFePO4 | HIGH |
| Shunt | Battery monitoring | 500A shunt | HIGH |
| Renogy ONE M1 | Battery monitor display | Bluetooth enabled | HIGH |
| Isolator Switch | Emergency disconnect | 300A+ rated | HIGH |
| 150A ANL Fuse | Inverter protection | For inverter main feed | **URGENT** |

### Priority 2: Shore Power System

| Component | Purpose | Specs | Priority |
|-----------|---------|-------|----------|
| AC Charger | Shore power charging | Victron Blue Smart 25A | HIGH |
| Shore Power Inlet | External 240V connection | 15A inlet, weatherproof | HIGH |
| RCD + Circuit Breaker | AC safety | 16A double-pole | HIGH |
| 240V outlets | AC power inside van | 2-3 outlets | MEDIUM |

### Priority 3: 12V Distribution

| Component | Purpose | Specs | Priority |
|-----------|---------|-------|----------|
| Fuse Block | 12V circuit distribution | 12-circuit Blue Sea or similar | HIGH |
| Switches | Light control | SPST + dimmers | MEDIUM |
| USB Outlets | Device charging | 12V USB, 2-4 outlets | MEDIUM |

### Priority 4: Loads

| Component | Purpose | Current Draw | Priority |
|-----------|---------|--------------|----------|
| MaxxAir Deluxe Fan | Ventilation | 2-5A (speed dependent) | HIGH |
| LED Downlights | Main lighting | 1-2A each, 6-8 total | HIGH |
| LED Strip Lights | Ambient lighting | 1-2A per 5m | MEDIUM |
| Reading Lights | Task lighting | 1-2A each, 2-4 total | MEDIUM |
| Water Pump | Water system | Shurflo 4009, 5-7A | FUTURE |
| Sound System | Entertainment | 3-7A | FUTURE |

### Optional: Smart Devices

| Component | Purpose | Notes |
|-----------|---------|-------|
| Zigbee Smart Relay | Automation | Control loads via phone |
| Door/Window Sensor | Monitoring | Security/monitoring |
| Temp Sensor | Climate monitoring | Track interior temp |

*Note: Zigbee devices controlled via phone, no dedicated hub*

---

## Load Calculations

### Daily Power Budget (Typical Usage)

| Load | Watts | Amps @12V | Hours/Day | Wh/Day | Notes |
|------|-------|-----------|-----------|--------|-------|
| **Lighting** |
| LED Downlights (6x) | 72W | 6A | 4 hours | 288Wh | Evening use |
| LED Strip Lights | 24W | 2A | 3 hours | 72Wh | Ambient |
| Reading Lights (2x) | 24W | 2A | 2 hours | 48Wh | Bedtime |
| **Appliances** |
| Dometic Fridge | 45W | 3.75A | 24h (30% duty) | 324Wh | Cycling compressor |
| MaxxAir Fan | 36W | 3A | 6 hours | 216Wh | Medium speed |
| Water Pump | 60W | 5A | 0.5 hours | 30Wh | Intermittent |
| **Devices** |
| USB Charging | 36W | 3A | 3 hours | 108Wh | Phones, tablets |
| Laptop (via inverter) | 65W | 5.4A | 4 hours | 260Wh | Work/entertainment |
| **Entertainment** |
| Sound System | 48W | 4A | 2 hours | 96Wh | Optional |
| **Monitoring** |
| Battery Monitor | 2W | 0.17A | 24 hours | 48Wh | Always on |
| Zigbee Devices | 5W | 0.4A | 24 hours | 120Wh | If used |
| **TOTAL** | | | | **1,610Wh/day** | Comfortable usage |

### Load Analysis

**Daily Consumption:** ~1,610Wh
**Battery Capacity:** 5,120Wh usable (400Ah × 12.8V)
**Days of Autonomy:** 3.2 days (no charging)

**Conservative Buffer:** Use only 80% depth of discharge = 4,096Wh available
**Realistic Autonomy:** 2.5 days

**Solar Generation (2x 260W panels, 5 sun hours):** 2,600Wh/day
**Net Daily Balance:** +990Wh (surplus)

✅ **System is well-sized for typical usage**

### Peak Load Analysis

**Maximum Simultaneous Draw:**

| Scenario | Loads Running | Total Amps | Total Watts | Notes |
|----------|---------------|------------|-------------|-------|
| Normal Evening | Lights (6A) + Fridge (4A) + Fan (3A) + USB (2A) | 15A | 192W | Typical |
| Cooking Time | Above + Inverter (laptop 5A) + Pump (5A) | 25A | 320W | Short duration |
| Heavy Use | All above + Sound (4A) | 29A | 371W | Maximum |

**Inverter Peak:**
- Laptop + phone chargers: ~100W AC (~9A DC)
- Kettle (if 500W model): 500W AC (~45A DC) - **10 minutes max**

⚠️ **Critical:** Avoid running kettle + other heavy loads simultaneously

---

## Solar Array Design

### Panel Specifications

**Your Panels (4x available):**
- Maximum Power: 260W
- Max Power Voltage (Vmp): 30.6V
- Max Power Current (Imp): 8.5A
- Open Circuit Voltage (Voc): ~37V (typical)
- Short Circuit Current (Isc): 9A
- Max Series Fuse: 15A
- Min Wire Size: 4mm²

### Renogy DCC50S Limits

**Critical Specifications:**
- Max Solar Input Voltage: **50V**
- Max Solar Input Power: **660W**
- MPPT Voltage Range: 12-50V

### Wiring Configuration

**OPTION 1: 2 Panels in Parallel (RECOMMENDED)**

```
Panel 1 (260W, 30.6V, 8.5A)    Panel 2 (260W, 30.6V, 8.5A)
         (+)    (-)                     (+)    (-)
          │      │                       │      │
          └──┬───┘                       └──┬───┘
             │                               │
         [Junction Box on Roof]
             │
        Parallel Connection:
        • Voltage: 30.6V ✓
        • Current: 17A ✓
        • Power: 520W ✓
             │
        4mm² solar cable (RED + BLK)
             │
        [Cable Entry Gland]
             │
        [25A Inline Fuse]
             │
        Renogy DCC50S Solar Input
```

**Specifications:**
- Combined Voltage: 30.6V ✓ (under 50V limit)
- Combined Current: 17A ✓ (within MPPT range)
- Combined Power: 520W ✓ (under 660W limit)
- Wire Size: 4mm² (adequate for 17A)
- Fuse: 25A (1.5× Isc = 1.5 × 18A = 27A, use 25A)

**OPTION 2: 2 Series Pairs in Parallel (DOES NOT WORK)**

❌ **Cannot use this configuration**
- 2 panels in series = 61.2V (EXCEEDS 50V limit)
- Would damage DCDC charger

**OPTION 3: 3-4 Panels in Parallel (EXCEEDS POWER LIMIT)**

❌ **Not recommended**
- 3 panels: 780W (exceeds 660W limit by 18%)
- 4 panels: 1040W (exceeds limit by 57%)
- May work but risks damaging DCDC or reduced efficiency

### Spare Panels: Future Options

**You have 2 spare 260W panels. Options:**

1. **Keep for future upgrade**
   - Upgrade to Victron SmartSolar 100/30 (~$300-400)
   - Max input 100V, 440W solar (still only 2 panels in parallel)
   - OR Victron SmartSolar 100/50 (~$500-600)
   - Max input 100V, 700W solar (2 panels series OR 3 parallel)

2. **Add dedicated MPPT controller**
   - Run 2 panels to Renogy DCDC (520W)
   - Run 2 panels to separate 30A MPPT controller (520W)
   - Total: 1040W solar to batteries
   - Cost: ~$200-300 for MPPT + wiring

3. **Sell spare panels**
   - Fund other build components
   - 520W is adequate for your 400Ah system

**Recommendation:** Keep spare panels, sell if you need funds now. 520W is plenty for your usage.

---

## Battery Configuration

### Parallel Connection (2x 200Ah = 400Ah)

**Battery Specifications:**
- Chemistry: LiFePO4 (Lithium Iron Phosphate)
- Voltage: 12.8V nominal (12V system compatible)
- Capacity: 200Ah each
- Total System: 400Ah @ 12.8V = 5,120Wh

**Wiring Configuration:**

```
    Battery #1 (200Ah)           Battery #2 (200Ah)

    [+]              [-]         [+]              [-]
     │                │           │                │
     │ 2/0 AWG        │           │ 2/0 AWG        │
     │ (70mm²)        │           │ (70mm²)        │
     │                │           │                │
     │         ┌──────┴───────────┘                │
     │         │                                   │
     └─────────┼───────────────────────────────────┘
               │
          [500A SHUNT]                         [Common Ground]
               │
          1/0 or 2/0 AWG
               │
         [+ BUS BAR]                          [- BUS BAR]
```

**Critical Wiring Rules:**

1. **Equal Length Cables**
   - Both positive cables SAME length (within 10cm)
   - Both negative cables SAME length (within 10cm)
   - Ensures balanced charging/discharging

2. **Shunt Placement**
   - MUST be on negative side
   - ALL negative loads connect AFTER shunt (on bus bar side)
   - Battery negative connects directly to shunt
   - Shunt to negative bus bar

3. **Wire Size**
   - Main battery cables: 2/0 AWG (70mm²) or 1/0 AWG (50mm²)
   - Rated for 200A+ continuous
   - Short runs (<1m) can use 2/0 AWG safely

4. **Fuses on Positive**
   - Each battery should have ANL fuse near terminal
   - 200-250A ANL fuse per battery
   - Protects against short circuit in cable

### Battery Mounting

**Physical Installation:**
- Battery #1: Currently on floor behind driver seat
- Battery #2: Mount as close as possible to Battery #1
- Secure both with ratchet straps or battery boxes
- Use rubber mat underneath to dampen vibration

**Spacing:**
- Maintain 10-20mm gap between batteries (air flow)
- Connect with short, equal-length cables

---

## Wire Sizing Chart

**Australian Standard: Minimum wire sizes for 12V systems, 3% voltage drop**

### Main Power Cables

| Circuit | Current (A) | Wire Size (AWG / mm²) | Length (one way) | Fuse Size | Notes |
|---------|-------------|----------------------|------------------|-----------|-------|
| Battery to Shunt | 200A | 2/0 AWG (70mm²) | <1m | 250A ANL | Main battery positive |
| Shunt to Pos Bus Bar | 200A | 2/0 AWG (70mm²) | <1m | N/A | After shunt |
| Battery Negative | 200A | 2/0 AWG (70mm²) | <1m | N/A | To common ground |

### High Current Loads

| Circuit | Current (A) | Wire Size (AWG / mm²) | Length | Fuse Size | Notes |
|---------|-------------|----------------------|--------|-----------|-------|
| Inverter + | 150A | 1/0 AWG (50mm²) | <2m | 150A ANL | Bus bar to inverter |
| Inverter - | 150A | 1/0 AWG (50mm²) | <2m | N/A | To negative bus bar |
| AC Charger + | 50A | 6 AWG (16mm²) | <2m | 60A | Bus bar to AC charger |
| AC Charger - | 50A | 6 AWG (16mm²) | <2m | N/A | To negative bus bar |
| DCDC to Bus Bar | 50A | 6 AWG (16mm²) | <2m | 60A | Already installed |

### Medium Current Loads

| Circuit | Current (A) | Wire Size (AWG / mm²) | Length | Fuse Size | Notes |
|---------|-------------|----------------------|--------|-----------|-------|
| 12V Power Hub | 20A | 12 AWG (4mm²) | 2-3m | 25A | Anderson connectors |
| Fuse Block Feed | 60A | 6 AWG (16mm²) | 1-2m | 80A | Main 12V distribution |

### Low Current Loads (from Fuse Block)

| Circuit | Current (A) | Wire Size (AWG / mm²) | Max Length | Fuse Size | Notes |
|---------|-------------|----------------------|------------|-----------|-------|
| MaxxAir Fan | 6A | 14 AWG (2.5mm²) | 5m | 10A | Ceiling mount |
| LED Downlights (group) | 10A | 14 AWG (2.5mm²) | 5m | 15A | 6-8 lights total |
| LED Strip Lights | 3A | 16 AWG (1.5mm²) | 5m | 5A | Ambient lighting |
| Reading Lights (pair) | 4A | 16 AWG (1.5mm²) | 3m | 5A | Bedside/desk |
| USB Outlets | 5A | 14 AWG (2.5mm²) | 3m | 7.5A | Multiple locations |
| Water Pump | 10A | 14 AWG (2.5mm²) | 4m | 15A | Under sink |
| Sound System | 8A | 14 AWG (2.5mm²) | 4m | 10A | Ceiling speakers |

### Solar Array

| Circuit | Current (A) | Wire Size (AWG / mm²) | Length | Fuse Size | Notes |
|---------|-------------|----------------------|--------|-----------|-------|
| Panels to Junction Box | 9A per panel | 4mm² (12 AWG) | 2-3m | 15A each | Per panel |
| Junction to DCDC | 18A combined | 4mm² (12 AWG) | 5-8m | 25A | Main solar feed |

### Monitoring & Control

| Circuit | Current (A) | Wire Size (AWG / mm²) | Length | Fuse Size | Notes |
|---------|-------------|----------------------|--------|-----------|-------|
| Shunt Sense Cables | <1A | 18-20 AWG (0.75mm²) | 3m | 2A | Comes with shunt |
| Battery Monitor | <1A | 18 AWG (0.75mm²) | 3m | 2A | Low voltage data |

**Wire Color Codes:**
- **RED** = Positive (12V+)
- **BLACK** = Negative (Ground)
- **YELLOW** = Switched positive (ignition, etc.)
- **BLUE** = Auxiliary/accessories
- **WHITE** = Ground/negative (alternative)

---

## Master Wiring Diagram

### ASCII Block Diagram

```
                                    SOLAR ARRAY
                          ┌─────────────────────────────┐
                          │  Panel 1      Panel 2       │
                          │  260W         260W          │
                          │  30.6V        30.6V         │
                          │  8.5A         8.5A          │
                          └──────┬──────────┬───────────┘
                                 │          │
                                 │  4mm² Parallel in Junction Box
                                 │  Combined: 30.6V, 17A, 520W
                                 │
                            [Cable Entry Gland]
                                 │
                            [25A Fuse]
                                 │
                                 ▼
                    ┌────────────────────────┐
                    │   Renogy DCC50S        │
                    │   50A DCDC + MPPT      │
                    │   Max: 50V / 660W      │
                    └─┬────────────────────┬─┘
                      │                    │
          Alternator  │                    │ Output: 50A max
          Connection  │                    │
                      │                    ▼
              ┌───────┴──────┐        [60A Fuse]
              │  Starter     │             │
              │  Battery     │             │
              │  (Engine)    │             │
              └──────────────┘             │
                                           ▼
┌──────────────────────────────────────────────────────────────────┐
│                         MAIN BATTERY BANK                         │
│                                                                   │
│   ┌─────────────────┐         ┌─────────────────┐               │
│   │  Battery #1     │         │  Battery #2     │               │
│   │  200Ah LiFePO4  │         │  200Ah LiFePO4  │               │
│   │  12.8V          │         │  12.8V          │               │
│   └────┬────────┬───┘         └────┬────────┬───┘               │
│        │        │                  │        │                    │
│        │ +      │ -                │ +      │ -                  │
│        │        │                  │        │                    │
│    [250A ANL]  │              [250A ANL]    │                    │
│        │        │                  │        │                    │
│        │  2/0   │  2/0             │  2/0   │  2/0               │
│        │  AWG   │  AWG             │  AWG   │  AWG               │
│        │        │                  │        │                    │
│        └────┬───┴──────────────────┴────────┘                    │
│             │              Common Negative                        │
│             │                     │                               │
│             │                     ▼                               │
│             │              [500A SHUNT]────►[Battery Monitor]    │
│             │                     │           Renogy ONE M1      │
│             │                     │                               │
│             ▼                     ▼                               │
│      [+ POSITIVE BUSBAR]   [- NEGATIVE BUSBAR]                   │
│             │                     │                               │
└─────────────┼─────────────────────┼───────────────────────────────┘
              │                     │
              │                     │
    ┌─────────┼─────────────────────┼──────────────┐
    │         │                     │              │
    │    ┌────┴───┐            ┌────┴───┐         │
    │    │        │            │        │         │
    ▼    ▼        ▼            ▼        ▼         ▼
┌────────┐   ┌────────┐   ┌────────┐  │   ┌─────────┐
│Inverter│   │AC Chrg │   │12V Hub │  │   │Fuse Blk │
│1500W   │   │25A     │   │Power   │  │   │12-Ckt   │
│        │   │Victron │   │Anderson│  │   │Blue Sea │
└───┬────┘   └────────┘   └───┬────┘  │   └────┬────┘
    │                         │        │        │
[150A ANL]              [inline fuse]  │    [80A fuse]
    │                         │        │        │
    │                         ▼        │        │
    │                    ┌────────┐    │        │
    │                    │ Dometic│    │        │
    │                    │ Fridge │    │        │
    │                    └────────┘    │        │
    │                                  │        │
    │                                  │        ├───► MaxxAir Fan (10A)
    │                                  │        ├───► LED Downlights (15A)
    │                                  │        ├───► LED Strips (5A)
    │                                  │        ├───► Reading Lights (5A)
    │                                  │        ├───► USB Outlets (7.5A)
    │                                  │        ├───► Water Pump (15A)
    │                                  │        └───► Sound System (10A)
    │                                  │
    ▼                                  │
┌────────────┐                        │
│ 240V AC    │                        │
│ Outlets    │                        │
│ (via inv.) │                        │
└────────────┘                        │
                                      │
        SHORE POWER                   │
        (When Plugged In)             │
              │                       │
    ┌─────────┴────────┐              │
    │  15A Inlet       │              │
    │  Exterior Mount  │              │
    └─────────┬────────┘              │
              │                       │
    ┌─────────▼────────┐              │
    │  RCD + Breaker   │              │
    │  16A Double Pole │              │
    └─────────┬────────┘              │
              │                       │
         ┌────┴────┐                  │
         │         │                  │
         ▼         ▼                  │
  ┌──────────┐  ┌──────────┐         │
  │AC Charger│  │240V Outs │         │
  │(to batt) │  │(direct)  │         │
  └──────────┘  └──────────┘         │
                                     │
           [Isolator Switch]◄────────┘
           Main Battery Disconnect
           (Emergency shutdown)
```

---

## Fuse Block Layout

### Recommended: Blue Sea 5025 Fuse Block (12-Circuit)

**Specifications:**
- 12 individual circuits
- 100A max per fuse block
- ATO/ATC blade fuses
- Negative bus bar included
- Cover protects fuses

### Proposed Circuit Layout

| Position | Circuit Name | Fuse Size | Wire Gauge | Load | Notes |
|----------|--------------|-----------|------------|------|-------|
| 1 | MaxxAir Fan | 10A | 14 AWG | 6A max | Ceiling mount, switched |
| 2 | Downlights Zone 1 | 10A | 14 AWG | 3-4 lights | Main cabin lights |
| 3 | Downlights Zone 2 | 10A | 14 AWG | 3-4 lights | Kitchen/bed area |
| 4 | LED Strip - Ambient | 5A | 16 AWG | 2-3A | Under cabinet/bed |
| 5 | Reading Lights | 5A | 16 AWG | 2-4A | Bedside + desk |
| 6 | USB Outlets Zone 1 | 5A | 14 AWG | Kitchen/work area | 2 outlets |
| 7 | USB Outlets Zone 2 | 5A | 14 AWG | Bed/living area | 2 outlets |
| 8 | Water Pump | 15A | 14 AWG | 5-7A | Future install |
| 9 | Sound System | 10A | 14 AWG | 4-7A | Future install |
| 10 | Zigbee Smart Relay | 5A | 16 AWG | Variable | Controlled loads |
| 11 | Spare | - | - | - | Future expansion |
| 12 | Spare | - | - | - | Future expansion |

**Feed to Fuse Block:**
- Positive: 6 AWG from positive bus bar, 80A fuse
- Negative: 6 AWG to fuse block's integrated negative bus

**Total Fuse Block Load:** ~40A max (all circuits on simultaneously - unlikely)

---

## Component Placement

### Electrical Cabinet (Behind Driver Seat, Wall Mounted)

**Current Layout:**
- Renogy DCC50S (wall mounted)
- Positive bus bar (wall mounted)
- Negative bus bar (wall mounted)
- 1500W Inverter (wall mounted)

**To Add to Cabinet:**

| Component | Mounting | Height | Notes |
|-----------|----------|--------|-------|
| Fuse Block | Wall mount | Mid-level | Easy access to fuses |
| AC Charger (25A) | Wall mount | Upper | Near bus bars |
| Isolator Switch | Wall mount, prominent | Easy reach | Emergency access |
| Battery Monitor Display | Wall mount, visible | Eye level | Renogy ONE M1 |
| Shore power distribution | Wall mount | Lower | RCD + breaker box |

**Suggested Arrangement (Top to Bottom):**

```
┌────────────────────────────────┐
│  Battery Monitor (Display)     │  ← Eye level, easy to read
├────────────────────────────────┤
│  Isolator Switch (RED)         │  ← Prominent, easy to reach
├────────────────────────────────┤
│  Fuse Block (Blue Sea)         │  ← Mid level, access fuses
├────────────────────────────────┤
│  AC Charger (Victron 25A)      │  ← Near bus bars
├────────────────────────────────┤
│  Renogy DCC50S (existing)      │  ← Already installed
├────────────────────────────────┤
│  + Bus Bar    - Bus Bar        │  ← Bus bars side by side
├────────────────────────────────┤
│  1500W Inverter (existing)     │  ← Already installed
└────────────────────────────────┘
```

### Battery Compartment (Floor Level)

**Current:**
- Battery #1 on floor in front of electrical cabinet

**To Add:**
- Battery #2 next to (or near) Battery #1
- 500A Shunt (between batteries and positive bus bar cable)
- ANL fuse holders (2x 250A, one per battery positive)

**Layout:**

```
┌─────────────────────────────────────┐
│  Battery #1          Battery #2     │
│  200Ah LiFePO4       200Ah LiFePO4  │
│  ┌──────────┐        ┌──────────┐   │
│  │  [+] [-] │        │  [+] [-] │   │
│  └──┬────┬──┘        └──┬────┬──┘   │
│     │    │              │    │      │
│  [250A] │           [250A]  │      │
│   ANL   │            ANL    │      │
│     │   └──────┬────────────┘      │
│     │          │                    │
│     │     [500A Shunt]              │
│     │          │                    │
│     └──────────┴──► To + Bus Bar    │
│                                     │
│     Common Negative to - Bus Bar    │
└─────────────────────────────────────┘
```

### Solar Panel Placement (Roof)

**Panels:** 2x 260W (using), 2x 260W (spare)

**Roof Mounting:**
- Mount to roof rack (Phase 6)
- Position panels at rear of roof (near electrical cabinet below)
- Minimize cable run length

**Junction Box:**
- Weatherproof solar junction box on roof
- Parallel connection of 2 panels
- 25A fuse before cable entry

### Cable Entry Point

**Solar Cable Entry:**
- Use double cable entry gland (IP68 rated)
- Mount on roof near electrical cabinet
- Seal with Sikaflex 221/291
- Cable runs down inside wall to DCDC charger

### Shore Power Inlet

**Location:** Driver side exterior (near electrical cabinet inside)
- 15A weatherproof inlet
- Sealed with marine sealant
- Cable runs through wall to RCD/breaker inside cabinet

---

## Installation Sequence

### Phase 1: Safety & Monitoring (DO THIS FIRST)

**Critical: Make System Safe**

1. **Install Isolator Switch**
   - Mount in electrical cabinet (prominent position)
   - Install on positive bus bar (before all loads except DCDC)
   - Test operation: OFF = no power to loads

2. **Add Fuse to Inverter** ⚠️ **URGENT**
   - Disconnect inverter positive wire from bus bar
   - Install 150A ANL fuse holder inline
   - Reconnect to bus bar
   - Test: Inverter should power on normally

3. **Replace DCDC Fuse (if needed)**
   - Check current fuse size on DCDC output
   - Should be 60A for 50A charger
   - Replace if undersized or damaged

### Phase 2: Battery Bank Expansion

4. **Install Shunt**
   - Disconnect Battery #1 negative from bus bar
   - Install 500A shunt between battery and bus bar
   - Battery negative → Shunt → Negative bus bar
   - Tighten all connections

5. **Install Battery Monitor**
   - Connect shunt sense wires to shunt terminals
   - Mount Renogy ONE M1 display in cabinet (eye level)
   - Power monitor from fuse block (2A fuse)
   - Configure for 400Ah total capacity

6. **Add Battery #2**
   - Position Battery #2 near Battery #1
   - Install 250A ANL fuse on each battery positive
   - Connect positives in parallel (equal length cables, 2/0 AWG)
   - Connect negatives in parallel (equal length cables, 2/0 AWG)
   - Both negatives to shunt
   - Update battery monitor to 400Ah

7. **Test Battery System**
   - Check voltage at bus bars: Should be 12.8-13.6V
   - Verify battery monitor shows correct voltage and capacity
   - Load test: Turn on inverter + fridge, check current draw

### Phase 3: 12V Distribution

8. **Install Fuse Block**
   - Mount Blue Sea fuse block in cabinet (mid-level)
   - Run 6 AWG positive from bus bar to fuse block input (80A fuse on bus bar)
   - Run 6 AWG negative from negative bus bar to fuse block negative terminal
   - Label all circuit positions (see Fuse Block Layout)

9. **Run Wiring for Future Loads**
   - Before installing wall panels, run ALL 12V wiring:
     - MaxxAir fan: 14 AWG to ceiling location
     - Downlights: 14 AWG to each light location (8x)
     - LED strips: 16 AWG along cabinet/bed areas
     - Reading lights: 16 AWG to bedside + desk locations
     - USB outlets: 14 AWG to 4 locations (2 zones)
     - Water pump: 14 AWG to future sink location
     - Sound speakers: 14 AWG to 4 ceiling locations
   - Leave 300mm extra wire at each end
   - Label all wires with masking tape

10. **Install Switches**
    - Mount switches in convenient locations:
      - Main lights: Near entry door
      - Reading lights: Bedside + desk
      - MaxxAir fan: Near fan (or use fan's built-in control)
      - LED strips: Near bed/kitchen
    - Run switch wiring before panels go up

### Phase 4: Solar Array

11. **Mount Solar Panels to Roof Rack**
    - Install roof rack first (see plan.md Phase 6)
    - Mount 2x 260W panels at rear of rack
    - Secure with proper mounting hardware
    - Store 2 spare panels for later

12. **Wire Solar Array**
    - Install weatherproof junction box on roof
    - Connect Panel 1 and Panel 2 in parallel
    - Positive to positive, negative to negative
    - Verify: 30.6V, 17A combined

13. **Install Cable Entry Gland**
    - Drill hole in roof near DCDC location
    - Install double cable entry gland (IP68)
    - Thread 4mm² solar cable through (RED + BLACK)
    - Seal with Sikaflex 221/291

14. **Connect Solar to DCDC**
    - Run solar cable from roof junction box to DCDC
    - Install 25A inline fuse on positive wire (near DCDC)
    - Connect to DCDC solar input terminals
    - Test: DCDC should show solar input (sunny day)

### Phase 5: Shore Power System

⚠️ **IMPORTANT: Licensed electrician required for 240V AC work (AS/NZS 3001.2)**

15. **Install Shore Power Inlet**
    - Choose location on driver side exterior (near cabinet)
    - Cut hole for 15A inlet, weatherproof cover
    - Mount inlet, seal with marine sealant
    - Run 2.5mm² TPS cable through wall to cabinet

16. **Install RCD + Circuit Breaker**
    - Mount 16A double-pole RCD/breaker in cabinet
    - Connect shore power inlet to RCD input
    - Install small distribution board if running multiple AC outlets

17. **Install AC Charger**
    - Mount Victron Blue Smart 25A in cabinet (near bus bars)
    - Connect AC charger INPUT to RCD output (240V AC)
    - Connect AC charger OUTPUT to bus bars:
      - Positive to positive bus bar (50A fuse inline)
      - Negative to negative bus bar
    - Configure charger for LiFePO4 profile

18. **Install 240V Outlets (Optional)**
    - If desired, wire 2-3 double-pole 240V outlets inside van
    - Connect to RCD output
    - Follow AS/NZS 3001.2 standards

19. **Test Shore Power**
    - Plug into shore power (15A caravan park outlet)
    - RCD should NOT trip
    - AC charger should activate, display charging
    - Battery monitor should show charging current (~25A)
    - Test 240V outlets (if installed)

### Phase 6: Install Loads (As Build Progresses)

20. **MaxxAir Fan** (Phase 1 of build)
    - Install fan in roof cutout
    - Connect to pre-run 14 AWG wiring
    - Connect to fuse block Position 1 (10A fuse)
    - Test fan on all speeds

21. **LED Lighting** (Phase 2 of build)
    - Install downlights in ceiling (after panels up)
    - Install LED strips under cabinets
    - Install reading lights at bedside/desk
    - Connect all to fuse block (see layout)
    - Test all lights and switches

22. **USB Outlets** (Phase 5 of build)
    - Mount 12V USB outlets in convenient locations
    - Connect to fuse block (Positions 6 & 7)
    - Test charging phone/tablet

23. **Water Pump** (Phase 4 of build)
    - Install Shurflo 4009 pump (future)
    - Connect to fuse block Position 8 (15A fuse)
    - Wire to tap switch

24. **Sound System** (Phase 5 of build, optional)
    - Install ceiling speakers (future)
    - Wire to Bluetooth amplifier
    - Connect to fuse block Position 9 (10A fuse)

### Phase 7: Final Testing & Documentation

25. **System Load Test**
    - Turn on all loads simultaneously
    - Check battery monitor current draw
    - Should not exceed 60A total
    - Verify no voltage drop at bus bars

26. **Charging Test**
    - Solar: Verify charging on sunny day
    - Alternator: Verify DCDC charges while driving
    - Shore: Verify AC charger works when plugged in

27. **Photo Document System**
    - Photograph all wiring connections
    - Label all cables permanently
    - Create wiring diagram with actual wire colors
    - Store photos on phone + cloud backup

28. **Create Load Schedule**
    - Document normal daily loads and times
    - Track battery % over 3-5 days typical use
    - Verify system meets needs

---

## Testing Checklist

### Pre-Power-On Checks

- [ ] **Visual Inspection**
  - [ ] All wire connections tight (no loose terminals)
  - [ ] Correct wire polarity (RED = positive, BLACK = negative)
  - [ ] No bare wire exposed (heat shrink on all connections)
  - [ ] All fuses installed in holders
  - [ ] No wires touching metal van body

- [ ] **Resistance Testing (Multimeter - Ohms Mode)**
  - [ ] Positive bus bar to negative bus bar: Should be OPEN (infinite resistance) when loads off
  - [ ] Positive battery terminal to positive bus bar: Should be <0.1Ω
  - [ ] Negative battery terminal to shunt: Should be <0.1Ω
  - [ ] Shunt to negative bus bar: Should be <0.1Ω

### Initial Power-On

- [ ] **Battery Connection**
  - [ ] Isolator switch OFF
  - [ ] Connect Battery #1 positive (expect small spark from bus bar capacitance)
  - [ ] Connect Battery #1 negative to shunt
  - [ ] Measure voltage at bus bars: Should be 12.8-13.6V (LiFePO4 resting voltage)

- [ ] **Battery Monitor Test**
  - [ ] Battery monitor display shows correct voltage
  - [ ] Battery monitor shows 200Ah capacity (before Battery #2 installed)
  - [ ] Monitor updates when load applied

- [ ] **Add Battery #2**
  - [ ] Isolator switch OFF
  - [ ] Connect Battery #2 in parallel
  - [ ] Measure voltage: Should remain 12.8-13.6V (not double!)
  - [ ] Update battery monitor to 400Ah
  - [ ] Monitor should show ~0A current draw (idle)

### Load Testing

- [ ] **Inverter Test**
  - [ ] Turn on isolator switch
  - [ ] Turn on inverter
  - [ ] Plug in low-power device (phone charger, 10W)
  - [ ] Battery monitor should show small current draw (~1A)
  - [ ] Measure inverter output voltage: Should be 230-240V AC
  - [ ] Turn off inverter

- [ ] **12V Hub Test**
  - [ ] Plug device into 12V hub cigarette lighter socket
  - [ ] Device should power on
  - [ ] Battery monitor shows current draw
  - [ ] Disconnect device

- [ ] **Fuse Block Test (Each Circuit)**
  - [ ] Connect test load (12V bulb) to each fuse position
  - [ ] Turn on circuit switch
  - [ ] Bulb should light
  - [ ] Battery monitor shows current draw
  - [ ] Test fuse: Remove fuse, bulb should turn off
  - [ ] Repeat for all 12 positions

### Charging System Testing

- [ ] **Solar Charging Test**
  - [ ] Sunny day, panels facing sun
  - [ ] DCDC display should show solar input
  - [ ] Battery monitor should show positive current (charging)
  - [ ] Typical: 15-20A on good sunny day (2x 260W panels)
  - [ ] Measure voltage at DCDC solar input: Should be ~30-36V

- [ ] **Alternator Charging Test**
  - [ ] Start engine
  - [ ] DCDC should activate (indicator light)
  - [ ] Battery monitor should show charging current (~40-50A)
  - [ ] Drive for 10 minutes, battery % should increase

- [ ] **Shore Power Charging Test**
  - [ ] Plug into shore power (240V)
  - [ ] RCD should NOT trip
  - [ ] AC charger should activate (Victron displays charging)
  - [ ] Battery monitor should show ~25A charging current
  - [ ] Let charge for 1 hour, battery % should increase

### Safety Testing

- [ ] **Isolator Switch Test**
  - [ ] Turn isolator switch OFF
  - [ ] All loads should lose power (except DCDC from alternator)
  - [ ] Battery monitor should show ~0A
  - [ ] Turn isolator ON, power returns

- [ ] **Fuse Testing**
  - [ ] Create deliberate short circuit on one fuse block position (using fused test circuit)
  - [ ] Fuse should blow immediately
  - [ ] Other circuits should remain powered
  - [ ] Replace fuse, circuit works again

- [ ] **Voltage Drop Test (Under Load)**
  - [ ] Turn on inverter with 500W load (kettle or heater)
  - [ ] Measure voltage at battery terminals: Record voltage
  - [ ] Measure voltage at inverter input: Should be within 0.3V of battery
  - [ ] If >0.3V drop, check wire size and connections

### Final System Validation

- [ ] **Peak Load Test**
  - [ ] Turn on: Lights + fridge + fan + inverter (laptop) + USB charging
  - [ ] Battery monitor current draw: Should be 25-35A
  - [ ] Voltage at bus bar: Should stay above 12.5V
  - [ ] Run for 30 minutes, no issues
  - [ ] Turn off loads

- [ ] **Autonomy Test**
  - [ ] Fully charge batteries (13.6V)
  - [ ] Disconnect all charging sources
  - [ ] Run typical daily loads for 24 hours
  - [ ] Record battery % at end of day
  - [ ] Should use 30-40% of battery (1600Wh / 5120Wh)

- [ ] **Documentation Complete**
  - [ ] Photograph all wiring
  - [ ] Label all circuits
  - [ ] Record all fuse sizes
  - [ ] Create emergency shutdown procedure
  - [ ] Test emergency shutdown (isolator switch)

---

## Notes & Recommendations

### Critical Safety Items

1. **NEVER work on system with batteries connected** - Use isolator switch or disconnect batteries
2. **Always fuse positive wires** - Within 300mm of power source
3. **Use correct wire gauge** - Undersized wire = fire risk
4. **Double-check polarity** - Reversing polarity destroys electronics
5. **Secure all cables** - Use cable ties, prevent chafing on metal edges

### Upgrade Path (Future)

**If you want to add 2 more solar panels later:**

**Option 1: Upgrade DCDC Charger**
- Replace Renogy DCC50S with Victron SmartSolar 100/50 (~$500-600)
- Can handle 700W solar, up to 100V input
- Could run 2 panels in series (61.2V, 520W) - more efficient long cable runs
- OR 3-4 panels in parallel

**Option 2: Add Dedicated MPPT**
- Keep Renogy DCDC for alternator + 2 panels
- Add separate Victron SmartSolar 30A MPPT for other 2 panels (~$200-300)
- Both chargers feed batteries independently
- Total: 1040W solar

### Smart Device Integration

**Zigbee Devices (Smart Relay, Sensors):**
- Control via phone app (no dedicated hub)
- Can automate loads (e.g., turn on fan when temp > 30°C)
- Monitor door/window status remotely
- Low power draw (<5W total)

**Suggested Automations:**
- Smart relay controls MaxxAir fan based on temp sensor
- Door sensor triggers interior lights (entry light)
- Battery monitor integrates with phone alerts (low battery warning)

### Troubleshooting Guide

| Problem | Check | Solution |
|---------|-------|----------|
| No power to loads | Isolator switch position | Turn ON |
| | Battery voltage | Charge batteries |
| | Fuse blown | Replace fuse, find short |
| Battery not charging (solar) | Solar panel connections | Check polarity, tight connections |
| | DCDC display | Should show solar input |
| | Shading | Ensure panels not shaded |
| Battery not charging (alternator) | Engine running? | DCDC only works with engine on |
| | DCDC indicator light | Should illuminate when charging |
| | Fuse on DCDC output | Check 60A fuse |
| Battery not charging (shore) | RCD tripped? | Reset RCD, check for fault |
| | AC charger display | Should show charging mode |
| | Fuse on AC charger output | Check 50A fuse |
| Inverter not working | Isolator switch ON? | Turn ON |
| | Battery voltage | Must be >11.5V |
| | Inverter fuse | Check 150A ANL fuse |
| High voltage drop under load | Wire gauge too small | Upgrade to larger wire |
| | Poor connections | Clean and tighten terminals |
| | Wire length too long | Shorten wire run or upsize |
| Battery monitor inaccurate | Shunt wiring | Check sense wire connections |
| | Calibration | Recalibrate monitor (full charge to full discharge) |
| | Capacity setting | Set to 400Ah in monitor settings |

---

## Shopping List - Electrical Components

**See shopping-list.md for supplier details. Below is electrical-specific summary.**

### Priority 1: Immediate Safety

- [ ] 150A ANL fuse + holder (for inverter) - **URGENT**
- [ ] 2x 250A ANL fuse + holders (battery protection)
- [ ] 500A shunt (for battery monitoring)
- [ ] Renogy ONE M1 battery monitor
- [ ] Isolator switch (300A+ rated)

### Priority 2: Battery #2 & Wiring

- [ ] Second 200Ah LiFePO4 battery
- [ ] 2/0 AWG (70mm²) battery cable - RED + BLACK (need ~3-4m total)
- [ ] Battery terminals and lugs (for 2/0 AWG)
- [ ] Battery boxes or ratchet straps (secure batteries)

### Priority 3: Shore Power

- [ ] Victron Blue Smart 25A AC charger (~$350-450)
- [ ] 15A shore power inlet (weatherproof, Clipsal/Transco)
- [ ] 16A RCD + circuit breaker combo
- [ ] 2.5mm² TPS cable (~10m)
- [ ] Small distribution board/enclosure
- [ ] 15A extension lead (10m) for caravan parks

### Priority 4: 12V Distribution

- [ ] Blue Sea 5025 fuse block (12-circuit) or similar (~$80-150)
- [ ] Assorted ATO/ATC blade fuses (5A, 10A, 15A, 20A)
- [ ] 6 AWG (16mm²) wire - RED + BLACK (~5m each)
- [ ] 14 AWG (2.5mm²) wire - RED + BLACK (~30m each)
- [ ] 16 AWG (1.5mm²) wire - RED + BLACK (~20m each)
- [ ] Crimp terminals (ring, butt, spade - variety pack)
- [ ] Heat shrink tubing (assorted sizes)

### Priority 5: Solar Components

- [ ] Solar cable entry gland (double cable, IP68 rated)
- [ ] 25A inline fuse holder + fuse
- [ ] 4mm² solar cable - RED + BLACK (~10m each)
- [ ] MC4 connectors (if not on panels already)
- [ ] Weatherproof junction box (roof-mounted)
- [ ] Cable ties, UV-resistant

### Priority 6: Switches & Controls

- [ ] SPST switches (4-6x) for lighting circuits
- [ ] Dimmer switches (2x) for main lights - optional
- [ ] Rocker switches or similar (depending on preference)

### Tools (If You Don't Have)

- [ ] Crimping tool (for insulated terminals)
- [ ] Wire strippers (14-10 AWG range)
- [ ] Multimeter (voltage, current, resistance)
- [ ] Cable cutter (for large gauge wire)
- [ ] Heat gun (for heat shrink)
- [ ] Label maker or masking tape + marker

**Estimated Total for Electrical Components: $1,500 - $2,500**
*(Includes battery #2, AC charger, fuse block, all wiring, shore power, monitoring)*

---

*Last Updated: January 2026*
