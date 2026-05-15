# KISRAK-M1 Motor Driver

KISRAK-M1 is a high-efficiency, discrete H-Bridge motor driver designed for micro-robotics. 

## Technical Specifications
- **Type:** Discrete Complementary MOSFET H-Bridge.
- **MOSFETs:** UT2305G (P-Ch) & UTM2054G (N-Ch) logic-level pairs.
- **Logic Level:** 3.3V compatible (Tested with STM32).
- **Protection:** Passive Hardware Dead-Time & Reverse Polarity Protection.

## Passive Dead-Time Design
This project uses an RC-D circuit (47R, 2.2nF, 1N4148) to create asymmetrical switching times, preventing shoot-through without complex MCU PWM dead-time configuration.
