## 1. Document Control & Metadata

| Document ID | Revision | Date | Author / Lead | Status |
| :--- | :---: | :---: | :--- | :---: |
| `COMPONENT-SWARM-001` | `v0.1` | 2026-10-06 | Anish Rangarajan | Draft |

---

## 2. Microcontroller (MCU)
### 2.1 Requirements Trace
* `[REQ-SW-02]` ARM Cortex-M core with hardware FPU, $\ge 100\text{ MHz}$.
* `[REQ-IF-01..04]` Minimum 3x SPI, 2x I2C, and 6x Hardware Timer blocks.

### 2.2 Candidate Comparison

| Component | Core & Clock | Package / Size | SPI Lines | I2C Lines | Timers | Flash / RAM | Operation Voltage | Current Draw | Price (DigiKey CAD) | Price (JLC CAD) | Stock (JLC)
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- | :--- | :--- | :--- | :--- |
| **STM32F405RGT7TR** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$21.24| ~$12.94 | 3
| **STM32F415RGT6** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$21.64 | ~$9.65 |0
| **STM32F405RGT7** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$22.96 | ~$16.82 |14
| **STM32F415RGT6TR** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$21.64 | ~$13.25 |0
| **STM32F405RGT6W** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$26.86 | ~$20.18 | 0|
---

### 2.3 Technical Analysis & Takeaways
* **JLCPCB is much cheaper:** Buying through JLCPCB/LCSC saves roughly 40% to 50% compared to DigiKey across all variants.
* **`TR` means machine-ready:** The **`TR`** suffix means Tape & Reel packaging. This lets JLCPCB’s assembly machines mount the chip automatically without extra handling fees or delays.
* **Stock:** Only two parts are in stock at JLCPCB right now: **`STM32F405RGT7TR`** (3 units left) and **`STM32F405RGT7`** (14 units left). All `F415` versions have zero stock and require a 3–5 day pre-order transfer.
* **Skip the crypto (`F415`):** The `F415` includes extra hardware encryption chips that we do not need for flight control math. The standard `F405` is the cleaner choice.
* **Zero layout risk:** Every single option shares the exact same pin layout and physical package (LQFP-64, $10 \times 10\text{ mm}$). Components can be  swapped without touching the PCB schematic or board layout. 

### 2.4 Final Verdict
* Given that all candidates share functionally identical specs regarding I/O, peripherals, timers, and clock speeds, the selection comes down to price and stock:
* **Primary Pick: `STM32F405RGT7TR`**
* If that is out of stock, any of the other choices will be used based on its current availability and price.
<!------------------------------------------------------------------------------------------ -->

## 3. Inertial Measurement Unit (IMU)
### 3.1 Requirements Trace
- `[REQ-SW-01]` High ODR (≥ 1 kHz) and low gyro noise to sustain the deterministic 1 kHz flight loop.
- `[REQ-IF-01]` Dedicated high-speed SPI bus (≥ 10 MHz) to prevent bus contention with SPI Flash or UWB peripherals.
- `[REQ-SAF-01]` Low-latency angular velocity and attitude updates to enable < 10 ms emergency tilt shutdown if vehicle pitch/roll exceeds 60°.
- `[REQ-HW-06]` To maximise battery life, components should attempt to minimise current draw
---

### 3.2 Candidate Comparison

| Component | Max SPI Speed | Gyro Noise | Accelerometer Noise | Max ODR | Package / Size | Voltage | Current Usage| Price (JLC CAD) |
| :--- | :---: | :---: | :---: | :---: | :--- | :--- | :---: | :---: |
| **ICM-42688-P**   | 24 MHz | 2.8 mdps/√Hz     | 70 µg/√Hz         | 32 kHz                     | LGA-14 (2.5 × 3.0 mm) | 1.71V – 3.6V  | 0.88 mA   | ~$3.85 |
| **BMI270**        | 10 MHz | 7.0 mdps/√Hz     | 160 µg/√Hz        | 6.5kHz(gyro),1.6 kHz(acc)  | LGA-14 (2.5 × 3.0 mm) | 1.71V – 3.6V  | 0.685 mA  | ~$2.45 |
| **LSM6DSOXTR**    | 10 MHz | 3.8 mdps/√Hz     | 110 µg/√Hz(16g)   | 6.66 kHz                   | LGA-14 (2.5 × 3.0 mm) | 1.71V – 3.6V  | 0.55 mA   | ~$3.10 |
| **ICM-20602**     | 10 MHz | 4.0 mdps/√Hz     | 100 µg/√Hz        | 8 kHz                      | LGA-16 (3.0 × 3.0 mm) | 1.71V – 3.45V | 2.79  mA  | ~$4.50 |
| **MPU6000**       | 20 MHz | ~5.0 mdps/√Hz    | 400 µg/√Hz        | 8 kHz                      | QFN-24 (4.0 × 4.0 mm) | 2.37V – 3.46V | 3.80 mA   | ~$6.50 |
| **MPU6050**       | N/A    | ~5.0 mdps/√Hz    | 400 µg/√Hz        | 1 kHz                      | QFN-24 (4.0 × 4.0 mm) | 2.37V – 3.46V | 3.80 mA   | ~$2.50 |
---
### 3.3 Technical Analysis & Takeaways

* **Vibration Rejection & Filtering (`[REQ-SW-01]`):** Coreless 8520 motors generate significant high-frequency mechanical noise ($150\text{--}300\text{ Hz}$). The **`ICM-42688-P`** ($2.8\text{ mdps}/\sqrt{\text{Hz}}$), **`LSM6DSOXTR`** ($3.8\text{ mdps}/\sqrt{\text{Hz}}$), and **`ICM-20602`** ($4.0\text{ mdps}/\sqrt{\text{Hz}}$) all meet the $\le 4.0\text{ mdps}/\sqrt{\text{Hz}}$ gyro noise threshold.
* **Bus Throughput & Control Loop Timing (`[REQ-IF-01]`):** Operating the `ICM-42688-P` at its maximum $24\text{ MHz}$ SPI clock ceiling enables full 6-axis frame reads within a short time frame without taking up the whole time budget. I2C devices like the `MPU6050` ($400\text{ kHz}$ max) suffer severe packet latency and are non-viable.
* **Power Efficiency (`[REQ-HW-06]`):** Modern MEMS IMUs (`ICM-42688-P`, `LSM6DSOXTR`, `BMI270`) draw $0.55\text{--}0.88\text{ mA}$ in full 6-axis Low-Noise mode, whereas legacy architectures (`ICM-20602`, `MPU6000`, `MPU6050`) draw $2.79\text{--}3.80\text{ mA}$ ($3\times$ to $5\times$ higher power consumption).
* **Accelerometer Noise & EKF State Estimation:** Accelerometer noise directly impacts hover stability and Z-axis position estimation. The `ICM-42688-P` and `LSM6DSOXTR` lead performance with $70\ \mu\text{g}/\sqrt{\text{Hz}}$, outperforming legacy sensors ($400\ \mu\text{g}/\sqrt{\text{Hz}}$).
* **PCB Layout & Decoupling:** Placement should be near the PCB's geometric center (or compensated via software lever-arm offsets in the EKF). Requires an unbroken Layer 2 solid ground plane beneath the package and $0.1\ \mu\text{F}$ + $2.2\ \mu\text{F}$ ceramic decoupling capacitors placed $< 2\text{ mm}$ from $V_{\text{DD}}$ and $V_{\text{DDIO}}$ pins.

---
### 3.4 Final Verdict

* **Primary Pick: `ICM-42688-P`**
  * **Why:** Checks all the boxes for `[REQ-SW-01]`, `[REQ-IF-01]`, and `[REQ-SAF-01]`, featuring best-in-class gyro noise density ($2.8\text{ mdps}/\sqrt{\text{Hz}}$), lowest accel noise ($70\ \mu\text{g}/\sqrt{\text{Hz}}$), native $24\text{ MHz}$ SPI throughput, $0.88\text{ mA}$ active draw, and high stock availability on JLCPCB.
* **Secondary Backup: `LSM6DSOXTR`**
  * **Why:** Excellent alternate candidate satisfying `[REQ-SW-01]` with $3.8\text{ mdps}/\sqrt{\text{Hz}}$ gyro noise in High-Performance mode, $70\ \mu\text{g}/\sqrt{\text{Hz}}$ accel noise, and ultra-low $0.55\text{ mA}$ power consumption.
<!------------------------------------------------------------------------------------------ -->
## 4. Barometer
### 4.1 Requirements Trace
* `[REQ-SW-05]` Barometer will have an error no greater than ($\pm 0.5\text{ m}$ precision)
* `[REQ-HW-06]` To maximise battery life, components should attempt to minimise current draw.

### 4.2 Candidate Comparison
| Component | Max SPI Speed | Barometer Noise | Max ODR | Voltage | Current Usage| Price (JLC CAD) | Stock
| :--- | :---: | :---: | :---: | :---: | :--- | :--- |:--- |
| **BMP390** | 10 MHz | ±0.03 hPa (~0.25 m) | 200 Hz | 1.65V – 3.6V | 3.2 µA | ~$5.16 | 0
|**BMP388**  | 10 MHz | ±0.08 hPa (~0.66 m) | 200 Hz | 1.65V – 3.6V | 3.4 µA | ~$5.56 | 239
|**BMP581**  | 12 MHz | ±0.06 hPa (~0.5 m)  | 240 Hz | 1.71V – 3.6V | 1.3 µA | ~$2.81 | 208 |
| **DPS310** | 10 MHz | ±0.002 hPa (~0.02 m)| 200 Hz | 1.7V – 3.6V  | 1.7 µA | ~$1.80 | 0 |
| **MS5611** | 20 MHz | ±0.012 mbar (~0.1 m) | ~100 Hz (at max OSR) | 1.8V – 3.6V | 1.0 µA | ~$0.0382 | 0 |
| **LPS22HB**| 10 MHz | ±0.1 hPa (~0.8 m)   | 75 Hz | 1.7V – 3.6V | 3 µA | ~$3.13 | 260 |

### 4.3 Technical Analysis & Takeaways

1. **The Noise vs. Environment Reality:** 
   As the drone will be primarily flown indoors, internal air pressure, HVAC systems, drone props etc will cause the barometer to behave incorrectly unless properly filtered. As such the noise levels of the DPS310 while impressive may not be too benficial here.

2. **Stock Considerations** 
   Given the LGA profile of the Barometers, attempting hand soldering could lead to issues with component damage. As such, SMT assembly by JLC will be prefereed. Therefore availability on the JLC assembly store is extremely important

---

### 4.4 Final Verdict
* **Primary Pick: `BMP581`**
  * **Why:** Perfect balance of power consumption (1.3 µA), high ODR/SPI bandwidth capabilities, and stock on JLCPCB/LCSC (200+ units available). It eliminates the need for manual LGA hand-soldering while seamlessly fitting into the future EKF altitude-fusion pipeline.

<!------------------------------------------------------------------------------------------ -->
## 5. Time of Flight
### 5.1 Requirements Trace
- `[REQ-SW-05]` Time of Flight sensor shall provide distance measurements up to 4 meters with millimeter-to-centimeter level precision for altitude hold.
- `[REQ-HW-06]` To maximise battery life, components should attempt to minimise current draw.

### 5.2 Candidate Comparison
| Component | Max Range | Field of View (FOV) | Max I2C Speed | Accuracy | Max ODR | Voltage | Current Usage | Price (JLC CAD) | Stock |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **VL53L1X** | 4.0 m | 27° | 400 kHz | ± 5 mm | 50 Hz | 2.8V - 3.3V | ~19 mA (active) | $3.30 | 12601 |
| **VL53L0X** | 1.2 - 2.0 m | 25° | 400 kHz | ± 3 mm | 50 Hz | 2.8V - 3.3V | ~20 mA (active) | $2.55 | 19685 |
| **VL53L5CX** | 4.0 m | 63° x 63° (8x8 grid) | 1 MHz | ± 5 mm | 60 Hz | 2.8V - 3.3V | ~15 mA (low power) | $7.18 | 827 |

### 5.3 Technical Analysis & Takeaways
The Vl53L5CX while being a stronger sensor overall, appears to be overkill for this project. Object collision avoidance and multi zonal tracking are the not key feature requirements and as such to balance complexity with performance, the VL53L0X or VL53L1X are far better choices
### 5.4 Final Verdict
The **VL53L1X** is selected as the primary Time of Flight sensor due to its ideal 4-meter range, reliable precision, and general stock availability for SMT assembly.
<!------------------------------------------------------------------------------------------ -->

## 6. Ultra Wide Band (UWB)
### 6.1 Requirements Trace

### 6.2 Candidate Comparison

### 6.3 Technical Analysis & Takeaways

### 6.4 Final Verdict

<!------------------------------------------------------------------------------------------ -->
## 7. Battery and Power Regulation
### 7.1 Requirements Trace

### 7.2 Candidate Comparison

### 7.3 Technical Analysis & Takeaways

### 7.4 Final Verdict
<!------------------------------------------------------------------------------------------ -->
## 8. Motors and Driving circuitry
### 8.1 Requirements Trace

### 8.2 Candidate Comparison

### 8.3 Technical Analysis & Takeaways

### 8.4 Final Verdict
<!------------------------------------------------------------------------------------------ -->
## 9. Radio Module
### 9.1 Requirements Trace

### 9.2 Candidate Comparison

### 9.3 Technical Analysis & Takeaways

### 9.4 Final Verdict
<!------------------------------------------------------------------------------------------ -->
## 10. External Memory
### 10.1 Requirements Trace

### 10.2 Candidate Comparison

### 10.3 Technical Analysis & Takeaways

### 10.4 Final Verdict

