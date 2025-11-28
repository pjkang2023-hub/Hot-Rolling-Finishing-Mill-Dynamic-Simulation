# 7-Stand Finishing Mill Simulator for Electrical Steel

A comprehensive digital twin and training simulator for hot rolling mill finishing operations, specifically designed for electrical steel production.

## Overview

This simulator models a 7-stand tandem finishing mill with realistic physics, control systems, and process parameters. It serves as both a training tool for mill operators and a digital twin for process optimization.

![Simulator Interface](hrm_finishing_mill_dynamic_sim_mvp1.html)

## Features

### Process Simulation
- **7-Stand Tandem Mill**: Complete F1-F7 finishing mill configuration
- **Real-time Physics**: Sims' rolling force model, Hansel-Spittel flow stress equations
- **Temperature Modeling**: Deformation heating, roll cooling, radiation/convection losses
- **Mass Flow Conservation**: Automatic speed calculation based on thickness reduction

### Control Systems

#### Cascaded FDT (Finishing Delivery Temperature) Control
```
┌─────────────────────────────────────────────────────────┐
│               CASCADED FDT CONTROL                      │
├─────────────────────────────────────────────────────────┤
│  FDT Error ──► ISC (Primary) ──► Spray Flow 0-100%     │
│                    │                                    │
│              If saturated                               │
│                    ▼                                    │
│              Speed (Secondary) ──► Mill Speed ±30%     │
└─────────────────────────────────────────────────────────┘
```

- **ISC Auto Mode**: Inter-stand cooling spray control (primary, fast response)
- **Speed Control**: Mill speed adjustment (secondary, activates when ISC saturates)
- **Saturation Detection**: Automatic handoff between control modes

#### Additional Control Systems
- **AGC (Automatic Gauge Control)**: Feedforward, Monitor, and Mass Flow modes
- **Looper Control**: Tension and angle regulation between stands
- **Crown Control**: Strip flatness via bending force and CVC shift

### Inter-Stand Cooling (ISC) System
- 6 cooling zones (ISC1-2 through ISC6-7)
- Individual zone flow control (0-100%)
- Real-time heat removal calculation
- Optimize button for automatic flow calculation

### Materials Database
| Grade | Description | Target FDT |
|-------|-------------|------------|
| 50W800 | Non-Oriented Electrical Steel | 860-920°C |
| 35W300 | High-Grade Non-Oriented | 840-900°C |
| B35A300 | Premium Non-Oriented | 830-890°C |
| 27ZDKH95 | Grain-Oriented Electrical Steel | 900-950°C |

## User Interface

### Main Tabs

1. **Overview**: Mill schematic, thickness profile (bar chart), temperature profile
2. **Setup**: Material selection, dimensions, schedule calculation
3. **Control**: AGC settings, looper parameters, control system toggles
4. **Cooling**: ISC zone controls, temperature trends, FDT monitoring
5. **Optimization**: Schedule optimization settings, reduction distribution
6. **Trends**: Real-time charts for thickness, temperature, force, power, tension

### Key Displays
- **Mill Overview**: Visual schematic showing all 7 stands with rolls and strip
- **Profile Chart**: Bar chart showing thickness reduction through the mill
- **Zone Temperature Trends**: Real-time temperature at each stand exit
- **Cooling Summary**: Total water flow, heat removed, FDT error, control mode

## Operating Instructions

### Basic Operation

1. **Setup Phase**
   - Select material grade from dropdown
   - Set entry thickness (typically 25-40mm)
   - Set target exit thickness (typically 2-4mm)
   - Set strip width (typically 900-1300mm)
   - Set entry temperature (typically 1050-1200°C)

2. **Calculate Schedule**
   - Click "Calculate Schedule" button
   - Select reduction distribution method (Hybrid, Front-loaded, Balanced, etc.)
   - Review constraint violations if any

3. **Start Simulation**
   - Click "Start" button
   - Monitor real-time parameters
   - Observe FDT control response

### FDT Control Modes

| ISC Auto | Temp Ctrl | Behavior |
|----------|-----------|----------|
| OFF | OFF | Manual - No automatic FDT control |
| OFF | ON | Speed Only - Uses speed to control FDT |
| ON | OFF | ISC Only - Spray control, no speed backup |
| ON | ON | **Cascade** - ISC primary, Speed assists when saturated |

### Control Mode Indicators
- **ISC Auto (Primary)** - Green: Normal ISC control
- **ISC Saturated → Speed Assist** - Yellow: Speed helping ISC
- **ISC Saturated (Speed OFF)** - Red: ISC at limit, no backup
- **Speed Control Only** - Blue: Only speed control active
- **Manual** - Gray: No automatic control

## Technical Specifications

### Mill Configuration
| Parameter | F1 | F2 | F3 | F4 | F5 | F6 | F7 |
|-----------|----|----|----|----|----|----|-----|
| Work Roll Dia (mm) | 700 | 680 | 660 | 640 | 620 | 600 | 580 |
| Backup Roll Dia (mm) | 1400 | 1400 | 1400 | 1400 | 1400 | 1400 | 1400 |
| Max Force (MN) | 35 | 35 | 32 | 30 | 28 | 26 | 24 |
| Max Power (MW) | 8 | 8 | 7 | 6 | 5 | 5 | 4 |
| Mill Modulus (MN/mm) | 5.0 | 5.2 | 5.4 | 5.6 | 5.8 | 6.0 | 6.2 |

### Thermal Model Parameters
| Component | Parameter | Value |
|-----------|-----------|-------|
| Roll Cooling | HTC | 5000 W/(m²·K) |
| | Roll Temperature | 80°C |
| | Scale Factor | 0.15 |
| Interstand | Emissivity | 0.65 |
| | Ambient Temperature | 100°C |
| | Convection Coefficient | 8 W/(m²·K) |
| | Scale Factor | 0.60 |
| ISC Spray | HTC at 100% | 2500 W/(m²·K) |
| | Spray Length | 450mm |
| | Water Temperature | 28°C |

### Control System Tuning
| Controller | Parameter | Value |
|------------|-----------|-------|
| ISC Control | Kp | 0.8-1.5 (adaptive) |
| | Ki | 0.015-0.03 |
| | Saturation Threshold | 5% / 95% |
| Speed Control | Kp | 0.005 |
| | Ki | 0.001 |
| | Range | ±30% |

## Physics Models

### Rolling Force (Sims' Model)
```
F = σ_m × w × L_d × Q_p
```
Where:
- σ_m = Mean flow stress (Hansel-Spittel equation)
- w = Strip width
- L_d = Deformed contact length
- Q_p = Geometric factor

### Temperature Change per Stand
```
ΔT_net = ΔT_deformation - ΔT_roll_cooling - ΔT_interstand - ΔT_spray
```

### Flow Stress (Hansel-Spittel)
```
σ = A × exp(m1×T) × ε^m2 × ε̇^m3 × exp(m4/ε)
```

## File Information

- **File**: `finishing_mill_v20_fixed.html`
- **Type**: Single-file HTML application
- **Size**: ~7000 lines
- **Dependencies**: None (standalone)
- **Browser**: Modern browsers (Chrome, Firefox, Edge)

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| Space | Start/Pause simulation |
| R | Reset simulation |
| 1-6 | Switch to tab 1-6 |

## Known Limitations

1. Single coil simulation (no coil-to-coil transitions)
2. Simplified crown/flatness model
3. No roll wear modeling
4. Fixed interstand distances

## Future Enhancements

- [ ] Individual zone ISC optimization
- [ ] Coil head/tail temperature compensation
- [ ] Roll wear and thermal crown evolution
- [ ] Data export to CSV/Excel
- [ ] Multiple coil queue simulation
- [ ] Model Predictive Control (MPC) option

## Version History

| Version | Date | Changes |
|---------|------|---------|
| v20 | Nov 2024 | Cascaded FDT control, thermal model calibration, white theme UI, bar chart profile |

## Author

Developed as a training and digital twin tool for electrical steel finishing mill operations.

---

*For questions or issues, please refer to the simulation parameters and adjust as needed for your specific mill configuration.*
