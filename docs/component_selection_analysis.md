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

### 2.3 Observations
* **JLCPCB is much cheaper:** Buying through JLCPCB/LCSC saves roughly 40% to 50% compared to DigiKey across all variants.
* **`TR` means machine-ready:** The **`TR`** suffix means Tape & Reel packaging. This lets JLCPCB’s assembly machines mount the chip automatically without extra handling fees or delays.
* **Stock:** Only two parts are in stock at JLCPCB right now: **`STM32F405RGT7TR`** (3 units left) and **`STM32F405RGT7`** (14 units left). All `F415` versions have zero stock and require a 3–5 day pre-order transfer.
* **Skip the crypto (`F415`):** The `F415` includes extra hardware encryption chips that we do not need for flight control math. The standard `F405` is the cleaner choice.
* **Zero layout risk:** Every single option shares the exact same pin layout and physical package (LQFP-64, $10 \times 10\text{ mm}$). Components can be  swapped without touching the PCB schematic or board layout. 

### 2.4 Final Verdict
* Given that all candidates share functionally identical specs regarding I/O, peripherals, timers, and clock speeds, the selection comes down to price and stock:
* **Primary Pick: `STM32F405RGT7TR`**
* If that is out of stock, any of the other choices will be used based on its current availability and price.


## 3. Inertial Measurement Unit (IMU)

## 4. Barometer

## 5. Time of Flight

## 6. Ultra Wide Band (UWB)

## 7. Battery and Power Regulation

## 8. Motors and Driving circuitry

## 9. Radio Module

## 10. External Memory