# Fluid Bed Coffee Roaster (500g)

A DIY **500 g fluid-bed / spouted-bed coffee roaster** designed around a 120 V electrical supply, variable airflow, electric heating, and digital roast monitoring and control.

The project is intended to develop a compact home coffee roaster while exploring airflow, heat transfer, embedded controls, and roast-profile automation.

> **Status:** Early design / Rev A prototype

---

## Project Goals

The initial design targets are:

| Parameter | Target |
|---|---|
| Maximum batch | 500 g |
| Nominal batch | 350–400 g |
| Power | 120 VAC / 20 A |
| Heater | ~1.7–1.8 kW |
| Roast method | Spouted / fluid bed |
| Roast chamber | ~100 mm ID |
| Air nozzle | Interchangeable 30 / 35 / 40 mm |
| Blower | Variable-speed centrifugal |
| Controller | ESP32 |
| Temperature sensing | K-type thermocouples |
| Roast software | Artisan initially |
| Future software | Custom roast controller |

The design will evolve as airflow and thermal testing provide real operating data.

---

## System Concept

The roaster uses a high-velocity stream of heated air to circulate and roast the coffee.

```text
ROOM AIR
   │
   ▼
┌─────────────┐
│ CENTRIFUGAL │
│   BLOWER    │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   HEATER    │
│ ~1.7–1.8 kW │
└──────┬──────┘
       │
       ▼
┌─────────────┐
│   PLENUM    │
└──────┬──────┘
       │
       ▼
   30 / 35 / 40 mm
       NOZZLE
       │
       ▼
      ↑↑↑
    ↑↑↑↑↑↑
   ↑ COFFEE ↑
  ↓         ↓
┌─────────────┐
│    ROAST    │
│   CHAMBER   │
└──────┬──────┘
       │
       ▼
    CYCLONE
       │
       ├────► CHAFF
       │
       ▼
    EXHAUST
```

The central air jet lifts the beans through the middle of the chamber. As the air velocity decreases, the beans move outward and fall along the chamber walls before returning toward the nozzle.

The objective is to produce continuous bean circulation with significantly less total airflow than would be required to uniformly fluidize the entire bed.

---

## Repository Structure

The project is organized by engineering discipline.

```text
500g-fluid-bed-roaster/
│
├── README.md
│
├── mechanical/
│   ├── cad/
│   └── drawings/
│
├── electrical/
│   ├── wiring/
│   └── components/
│
├── controls/
│   ├── firmware/
│   └── artisan/
│
├── testing/
│
└── docs/
    ├── design.md
    ├── calculations.md
    └── bom.csv
```

### Mechanical

Mechanical design and fabrication files.

Includes:

- Roast chamber
- Bottom cone
- Interchangeable air nozzles
- Air plenum
- Chaff cyclone
- Bean dump mechanism
- Cooling tray
- Frame/enclosure
- CAD models
- Manufacturing drawings

### Electrical

Power distribution and high-power electrical systems.

Includes:

- 120 VAC input
- Heater
- Blower power
- Solid-state relay / power controller
- Contactors
- Fuses and circuit protection
- Emergency stop
- Over-temperature protection
- Airflow safety interlock
- Grounding
- Electrical enclosure
- Wiring diagrams

### Controls

Embedded electronics and software.

Includes:

- ESP32 firmware
- Thermocouple interfaces
- Pressure/airflow sensing
- Heater control
- Blower speed control
- Artisan integration
- Data logging
- Future custom roast-control software

### Testing

Experimental results and test procedures.

Testing will include:

- Cold-flow testing
- Nozzle comparison
- Minimum spouting airflow
- Bed pressure measurements
- Thermal testing
- Heater characterization
- Roast testing
- Roast profiles
- Artisan logs

### Docs

General engineering documentation.

Includes:

- System design
- Engineering calculations
- Component selection
- Bill of materials
- Design assumptions
- Design decisions

---

## Rev A Design

The first prototype is focused on validating the airflow system.

Current baseline geometry:

```text
Batch
    350–400 g nominal
    500 g maximum

Roast chamber
    ~100 mm ID
    ~275 mm straight section

Nozzles
    30 mm
    35 mm  ← baseline
    40 mm

Airflow
    ~25–45 CFM target

Blower
    High-static-pressure centrifugal
    Variable speed

Heater
    ~1.7–1.8 kW
    120 VAC
```

These values are preliminary and will be updated based on testing.

---

## Development Plan

### Rev A.1 — Cold Flow

Build the airflow system without a heater.

```text
Blower
   ↓
Plenum
   ↓
Interchangeable nozzle
   ↓
Roast chamber
```

Test:

- 100 g
- 250 g
- 350 g
- 500 g

For each batch size, determine:

- Minimum airflow for bean movement
- Minimum airflow for stable spouting
- Desired airflow for continuous circulation
- Bed pressure
- Effect of nozzle diameter
- Excessive airflow / bean carryover

This phase will determine the final blower requirements.

### Rev A.2 — Heated Prototype

Add:

- ~1.7–1.8 kW heater
- Heater power control
- Bean-temperature thermocouple
- Inlet-air thermocouple
- Exhaust thermocouple
- ESP32 controller
- Hardware safety systems
- Artisan integration

The objective is to produce the first controlled roast.

### Rev A.3 — Complete Prototype

Add:

- Chaff cyclone
- Bean dump mechanism
- Cooling tray
- Electrical enclosure
- Frame
- Improved controls

At this stage the prototype should operate as a complete coffee roaster.

---

## Controls Architecture

The ESP32 will act as the primary machine controller.

```text
                    ┌─────────────┐
                    │   ARTISAN   │
                    │             │
                    │ Roast Graph │
                    │ Logging     │
                    │ Profiles    │
                    └──────┬──────┘
                           │
                          USB
                           │
                           ▼
                    ┌─────────────┐
                    │    ESP32    │
                    └──────┬──────┘
                           │
            ┌──────────────┼──────────────┐
            │              │              │
            ▼              ▼              ▼
      Thermocouples   Heater Control   Blower Control
            │              │              │
            ▼              ▼              ▼
       Temperature         SSR        PWM / Analog
```

Initial development will use **Artisan** for roast visualization and logging.

A custom interface may be developed later while maintaining Artisan compatibility.

---

## Instrumentation

Planned measurements include:

| Measurement | Purpose |
|---|---|
| Bean temperature | Primary roast measurement |
| Inlet temperature | Heater output |
| Exhaust temperature | Process monitoring |
| Bed pressure | Fluidization characterization |
| Airflow | Blower characterization |
| Heater output | Roast logging |
| Blower command | Roast logging |

Not every development sensor will necessarily remain in the final machine.

---

## Safety

This project involves **mains electricity, high-current resistive heating, high temperatures, moving air, and combustible coffee/chaff**.

Software will not be treated as the primary safety mechanism.

Planned hardware protections include:

```text
120 VAC
   │
Circuit Protection
   │
Emergency Stop
   │
Main Contactor
   │
Airflow Interlock
   │
Independent High-Temperature Limit
   │
Heater Contactor
   │
Solid-State Power Controller
   │
Heater
```

The heater should not be capable of operating without adequate airflow.

All exposed conductive components will require appropriate protective grounding.

The final electrical design must remain within the capacity of the intended branch circuit and use appropriately rated wiring, connectors, switching devices, insulation, and over-current protection.

---

## Software

Initial software stack:

- **ESP32** — machine control and sensor acquisition
- **Artisan** — roast visualization, profiles, and logging
- **USB serial** — initial communication interface

Future development may include:

- Custom web interface
- Automated roast profiles
- Heater and airflow curves
- Rate-of-rise control
- Roast database
- Wi-Fi connectivity
- Automatic profile playback

---

## Current Priorities

1. Finalize roast chamber geometry.
2. Select a centrifugal blower for cold-flow testing.
3. Fabricate the 100 mm roast chamber.
4. Fabricate 30 / 35 / 40 mm nozzle inserts.
5. Measure airflow and bed pressure with 500 g of green coffee.
6. Select the final blower based on measured operating conditions.
7. Design the heater and electrical system.
8. Integrate ESP32 and Artisan.
9. Perform the first heated roast.

---

## Project Philosophy

The roaster will be developed experimentally rather than assuming the first design is correct.

In particular:

> **Measure first, size second.**

Cold-flow testing will establish the actual airflow and pressure required to circulate 500 g of coffee before the final blower and heater system are selected.

This allows later design decisions to be based on measured system behavior rather than theoretical estimates alone.
