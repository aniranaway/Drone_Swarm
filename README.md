# Autonomous UWB Drone Swarm

A fully custom, low-level embedded hardware and firmware platform designed for autonomous multi drone swarms. The system integrates custom STM32-based flight controller PCBs, coreless DC motor drive arrays for a micro quadcopter form factor, Ultra-Wideband (UWB) 3D spatial localization, and custom control stacks built entirely from first principles—bypassing off-the-shelf flight controllers and third-party firmware stacks like PX4 or Betaflight.

---
## Grand Objectives

* **Bare-Metal Flight Control Stack:** Implement real-time 6-DoF sensor fusion and cascaded PID control loops running directly on STM32 microcontrollers.
* **Advanced State Estimation:** Design and implement a custom Extended Kalman Filter (EKF) pipeline to fuse multi-sensor measurements (IMU, Barometer, UWB), effectively suppressing sensor noise and drift for precise real-time 6-DoF state estimation.
* **UWB Spatial Positioning:** Achieve indoor positioning and tracking using UWB hardware operating across fixed anchor networks.
* **Autonomous Swarm Dynamics:** Execute coordinated multi-agent formation flight, collision avoidance algorithms, and telemetry links managed via a custom ground control station.
* **Rigorous Systems Engineering:** Maintain end-to-end engineering discipline across the full lifecycle—from System Requirements Documents (SRD) and component analysis to PCB layout, manufacturing, and firmware deployment.

## Project Documentation
Detailed engineering decisions and specifications are broken down in the `docs/` directory:
* [System Requirements Document (SRD)](docs/system_requirements.md)
* [Component Selection Analysis](docs/component_selection_analysis.md)
* [System Architecture & Block Diagrams](docs/architecture.md)
* [Bill of Materials (BOM)](docs/bill_of_materials.md)

---
## Repository Structure
* **/docs** - System engineering documents, trade studies, and requirements.
* **/hardware** - KiCad/Altium schematic captures and PCB layout files.
* **/firmware** - STM32 C/C++ application logic (CMake/Ninja toolchain).

## Development Roadmap

This project follows a structured hardware-software co-design lifecycle, progressing from systems engineering and hardware fabrication to low-level control, state estimation, and multi-agent coordination:

- [ ] **Phase 1: Requirements & Component Selection** *(In Progress)*  
  Establish System Requirements Documents (SRD), execute trade studies for core components (MCU, IMU, UWB, Power), and calculate mass/power budgets.

- [ ] **Phase 2: Firmware & System Architecture Design**  
  Define MCU pinouts, system block diagrams, task state transitions, and the firmware project structure.

- [ ] **Phase 3: Schematic Capture & Circuit Design**  
  Design power regulation, coreless motor MOSFET drive arrays, RF layout rules for radio, SWD interfaces, sensor layouts, and dedicated test pads.

- [ ] **Phase 4: PCB Layout, Manufacturing & Board Bring-Up**  
  Perform 4-layer PCB layout with impedance matching, manage fabrication/assembly, and execute board bring-up (power rail verification, current draw checks, and SWD flashing).

- [ ] **Phase 5: Low-Level Drivers & Telemetry Baseline**  
  Develop register-level/HAL SPI drivers for the IMU, I2C drivers for the barometer, PWM timer outputs for motor control, UART telemetry output, and status LED error handlers.

- [ ] **Phase 6: Airframe Assembly & Hardware Integration**  
  Mount flight controllers to micro frames, wire motor connections, calibrate motor spin directions, and verify physical center of mass.

- [ ] **Phase 7: Advanced State Estimation & Flight Control**  
  Implement digital low-pass filtering, build a custom Extended Kalman Filter (EKF) pipeline for IMU/Baro/UWB fusion, write cascaded PID controllers, and perform tethered flight tuning.

- [ ] **Phase 8: Swarm Simulation & Waypoint Planning**  
  Develop simulation frameworks and Ground Control Station (GCS) telemetry interfaces to validate trajectory generation and collision avoidance prior to physical swarm deployment.

- [ ] **Phase 9: UWB Infrastructure & Multi-Agent Setup**  
  Deploy and calibrate fixed indoor UWB anchors, assemble secondary flight hardware, and establish inter-node wireless communications.

- [ ] **Phase 10: Swarm Coordination & Autonomous Flight**  
  Execute decentralized/centralized swarm algorithms for coordinated formation flight, multi-agent pathing, and real-time telemetry tracking.

---
## Core Toolchain & Software Stack
* **GNU Arm Embedded Toolchain (`arm-none-eabi-gcc`)** — Cross-compiler toolchain used to compile bare-metal C/C++ application logic into ARM Cortex-M machine code.
* **CMake** — Meta-build system used to manage cross-compilation targets, include directories, linker scripts (`.ld`), and hardware macros in a clean, maintainable structure.
* **Ninja** — High-speed build system executed by CMake to handle fast, parallelized incremental compilation.
* **STM32CubeMX** — Graphical initialization utility used to configure MCU clock trees, pin assignments, and hardware peripherals (SPI, I2C, UART, TIM) while generating isolated HAL setup files.
* **OpenOCD** — Open On-Chip Debugger used to flash binary targets (`.elf`/`.hex`) to the STM32 MCU and provide a live GDB server over Serial Wire Debug (SWD) via ST-LINK.
* **VS Code & Cortex-Debug** — Primary IDE environment leveraging the Cortex-Debug extension for hardware breakpoints, register inspection, and integrated CMake builds.

---
## Core Hardware Stack
* **Microcontroller:** (In Progress - See Component Selection Analysis)
* **Spatial Positioning:** (In Progress - See Component Selection Analysis)
* **IMU:** (In Progress - See Component Selection Analysis)
* **Barometer:** (In Progress - See Component Selection Analysis)
* **Power:** (In Progress - See Component Selection Analysis)

---
## Getting Started
*(Instructions for cloning, compiling with CMake, and flashing the STM32 will go here).*