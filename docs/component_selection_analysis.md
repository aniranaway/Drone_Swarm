## 1. Document Control & Metadata

| Document ID | Revision | Date | Author / Lead | Status |
| :--- | :---: | :---: | :--- | :---: |
| `COMPONENT-SWARM-001` | `v0.1` | 2026-10-06 | Anish Rangarajan | Draft |

---

## 2. Microcontroller (MCU)
### 2.1 Requirements Trace
* `[REQ-SW-02]` ARM Cortex-M core with hardware FPU, $\ge 100\text{ MHz}$.
* `[REQ-IF-01..04]` Minimum 3x SPI, 2x I2C, and 6x Hardware Timer blocks.

### 2.3 Candidate Comparison

### 2.3 Candidate Comparison (Pricing in CAD)

| Component | Core & Clock | Package / Size | SPI Lines | I2C Lines | Timers | Flash / RAM | Operation Voltage | Current Draw | Price (DigiKey CAD) | Price (LCSC CAD) |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- | :--- | :--- | :--- | :--- |
| **STM32F405RGT7TR** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$21.24| ~$13.04 |
| **STM32F415RGT6** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$21.64 | ~$10.11 |
| **STM32F405RGT7** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$22.96 | ~$17.67 |
| **STM32F415RGT6TR** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$21.64 | ~$13.04 |
| **STM32F405RGT6W** | Cortex-M4F @ 168 MHz | LQFP-64 ($10 \times 10\text{ mm}$) | 3x SPI | 3x I2C | 14x Timers | 1 MB / 192 KB | 1.8V – 3.6V | ~45–60 mA | ~$26.86 | ~$19.44 |
---

### 2.4 Observations
* 

### 2.5 Final Verdict
*
