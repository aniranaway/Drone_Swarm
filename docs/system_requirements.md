# System Requirements Document

## Introduction
The purpose of this document is to clearly set out the requirements of the project and provide a guideline for the component selection and firmware design.

## 1. Document Control & Metadata

| Document ID | Revision | Date | Author / Lead 
| :--- | :---: | :---: | :--- |
| `SRD-SWARM-001` | `v0.1` | 2026-10-04 | Anish Rangarajan 

## Revision History
| Revision | Date | Description of Changes |
| :---: | :---: | :--- |
| `v0.1` | 2026-10-06 | Initial draft. |

---

## 2. System Overview & Scope
The purpose of this project is to design, manufacture, and program an autonomous, ultra-wideband (UWB) localized micro-drone swarm platform from first principles. This document defines the engineering requirements governing hardware component selection, electrical interfaces, and real-time firmware execution.

---

## 3. Physical & Power Constraints

* **[REQ-HW-01] Maximum Takeoff Mass:** The fully assembled drone (including PCB, motors, frame, props, UWB transceiver, and battery) shall not exceed **40 grams**.
* **[REQ-HW-02] Primary Power Source:** System power shall be supplied by a single-cell LiPo battery (1S, nominal voltage 3.7V, operational voltage 3.0V – 4.2V).
* **[REQ-HW-03] Onboard Power Regulation:** The power distribution network shall regulate a stable **3.3V power rail** ($\pm 2\%$) for logic, state estimation sensing, and RF transceivers across the entire battery discharge curve.
* **[REQ-HW-04] EMI & Motor Noise Handling:** Motor drive channels shall incorporate hardware suppression (Schottky flyback diodes and decoupling bulk capacitance) to isolate inductive switching noise from sensor and digital logic buses.
* **[REQ-HW-05] Physical Dimensions:** Drone frame will not exceed 110mm and drone props will not exceed 70mm to meet space constraints. PCB dimensions will not exceed 40mm X 40mm
---

## 4. Compute & Software Performance

* **[REQ-SW-01] Loop Execution Rate:** The core flight control loop (IMU read, state update, PID execution, PWM write) shall execute deterministically at a minimum frequency of **1 kHz** with loop timing jitter <5%.
* **[REQ-SW-02] Processing Core Acceleration:** The onboard microcontroller shall feature an ARM Cortex-M architecture with hardware Floating Point Unit (FPU) operating at a minimum clock frequency of **100 MHz**.
* **[REQ-SW-03] Spatial Positioning Rate:** The Extended Kalman Filter (EKF) shall incorporate UWB position updates at a minimum refresh rate of **20 Hz** with static positioning accuracy within **±10 cm**.
* **[REQ-SW-04] Telemetry Transmission:** System state telemetry (attitude, battery voltage, position, system status) shall transmit over a wireless link at a selectable rate of **10 Hz to 50 Hz**.

---

## 5. Subsystem & Data Interfaces

* **[REQ-IF-01] Hardware SPI Allocation:** The MCU shall provide at least **3 independent hardware SPI buses** to isolate high-rate IMU reads, UWB frame transfers, and telemetry logging.
* **[REQ-IF-02] Hardware I2C Allocation:** The MCU shall provide at least **2 independent hardware I2C buses** to interface non-SPI sensors (e.g., barometers, optical flow) and external peripheral expanders.
* **[REQ-IF-03] Hardware Timer Peripherals:** The MCU shall utilize at least **6 hardware timer peripherals** dedicated to high-resolution PWM motor generation, microsecond-accurate loop timing execution, and telemetry scheduling.
* **[REQ-IF-04] Actuation PWM Channels:** Motor outputs shall be driven by low-side N-channel MOSFETs controlled via **4 independent hardware timer PWM channels**.
* **[REQ-IF-05] Debug Interface:** The hardware design shall break out a 4-pin SWD (Serial Wire Debug) header (VCC, GND, SWDIO, SWCLK) for programming and real-time hardware debugging.
* **[REQ-IF-06] RF & Antenna Layout:** The PCB design shall incorporate a **$50\ \Omega$ coplanar waveguide / microstrip transmission line** with adequate ground plane keep-out around the antenna region to ensure optimal RF impedance matching and power transfer for UWB and telemetry links.

---

## 6. Safety & Failsafes

* **[REQ-SAF-01] Emergency Tilt Shutdown:** The flight controller shall disarm all motor outputs within **10 ms** if vehicle pitch or roll angles exceed **60 degrees**.
* **[REQ-SAF-02] Telemetry Timeout Handling:** The flight controller shall transition to an automated failsafe landing or emergency disarm state if the primary telemetry/ground command link is lost for **>500 ms**.
* **[REQ-SAF-03] Low Voltage Protection:** The MCU shall continuously sample main battery voltage, triggering a low-battery warning flag at **3.4V** and executing an emergency disarm/landing sequence if cell voltage drops below **3.1V**.
* **[REQ-SAF-04] Hardware Diagnostic & Status LEDs:** The hardware design shall incorporate status LEDs driven by dedicated GPIO pins to provide visual error indications (e.g., IMU/UWB initialization failure, sensor bus packet drops, or active failsafe states).

## 7. Verification Cross-Reference Matrix (VCRM)

The Verification Cross-Reference Matrix maps every system requirement to its corresponding verification method and pass/fail criteria to ensure complete engineering traceability.

### 7.1 Verification Methods Legend
* **Inspection (I):** Visual or dimensional measurement without active system power or specialized test equipment.
* **Analysis (A):** Theoretical validation using calculations, circuit simulations, link budget analysis, or CAD modeling.
* **Demonstration (D):** Qualitative operational verification of system functionality without precise quantitative measurement instruments.
* **Test (T):** Quantitative measurement using calibrated bench instruments (oscilloscope, logic analyzer, power supply, motion capture, digital scale) under controlled test conditions.

---

### 7.2 Traceability Matrix

| Requirement ID | Requirement Description | Method | Verification Procedure & Pass / Fail Criteria |
| :--- | :--- | :---: | :--- |
| `[REQ-HW-01]` | Max Takeoff Mass $\le 40\text{g}$ | **I** | Place fully assembled vehicle with battery on digital scale. Mass must read $\le 40.0\text{ g}$. |
| `[REQ-HW-02]` | Primary Power Source | **I / T** | System powered exclusively by 1S LiPo (3.0V–4.2V operating range). |
| `[REQ-HW-03]` | 3.3V Power Regulation | **T** | Oscilloscope confirms 3.3V rail remains within $3.3\text{V} \pm 0.066\text{V}$ under 100% motor throttle step inputs. |
| `[REQ-HW-04]` | EMI & Noise Suppression | **T** | Flyback diodes and decoupling capacitors prevent motor switching noise from causing I2C/SPI bus resets or packet corruption. |
| `[REQ-HW-05]` | Envelope Dimensions | **I** | Vernier calipers verify PCB size $\le 40 \times 40\text{ mm}$, diagonal wheelbase $\le 110\text{ mm}$, and prop diameter $\le 70\text{ mm}$. |
| `[REQ-SW-01]` | 1 kHz Control Loop Rate | **T** | Logic analyzer connected to dedicated debug GPIO toggle pin confirms loop frequency of $1.0\text{ kHz} \pm 5\%$ ($50\ \mu\text{s}$ max jitter). |
| `[REQ-SW-02]` | Compute Acceleration | **A / I** | MCU datasheet verifies ARM Cortex-M core with hardware FPU operating at clock frequency $\ge 100\text{ MHz}$. |
| `[REQ-SW-03]` | UWB Positioning Rate | **D / T** | EKF incorporates UWB position updates at $\ge 20\text{ Hz}$ with static position estimation error $\le \pm 10\text{ cm}$. |
| `[REQ-SW-04]` | Telemetry Transmission | **T** | Packet capture verifies configurable state transmission rate between 10 Hz and 50 Hz without packet drops. |
| `[REQ-IF-01]` | Hardware SPI Allocation | **I / A** | Schematic and layout review verifies 3 independent hardware SPI buses routed to MCU pins. |
| `[REQ-IF-02]` | Hardware I2C Allocation | **I / A** | Schematic and layout review verifies 2 independent hardware I2C buses routed to MCU pins. |
| `[REQ-IF-03]` | Timer Peripherals | **A / T** | Firmware assignment allocates $\ge 6$ hardware timer blocks for PWM generation, loop execution, and scheduling. |
| `[REQ-IF-04]` | Actuation PWM Channels | **T** | Low-side N-channel MOSFET gates are driven by 4 distinct hardware timer PWM outputs. |
| `[REQ-IF-05]` | Debug Interface | **I / T** | 4-pin SWD header (VCC, GND, SWDIO, SWCLK) enables successful firmware flashing and live GDB debugging. |
| `[REQ-IF-06]` | RF & Antenna Layout | **A / I** | Layout tool confirms $50\ \Omega$ coplanar waveguide line; Gerber review verifies antenna ground clearance keep-out zone. |
| `[REQ-SAF-01]` | Emergency Tilt Shutdown | **T** | Rotating drone past $60^\circ$ pitch/roll on test rig drops motor PWM duty cycle to $0\%$ within $<10\text{ ms}$. |
| `[REQ-SAF-02]` | Telemetry Timeout | **T** | Disconnecting ground control RF link for $>500\text{ ms}$ triggers immediate disarm/failsafe landing state. |
| `[REQ-SAF-03]` | Low Voltage Protection | **T** | Variable DC power supply voltage drop triggers telemetry warning flag at $3.4\text{V}$ and emergency shutdown at $3.1\text{V}$. |
| `[REQ-SAF-04]` | Diagnostic Status LEDs | **D** | Simulating sensor bus disconnects or initialization faults illuminates corresponding diagnostic LED error codes. |