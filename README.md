# KISRAK-M1 Motor Driver

KISRAK-M1 is a high-efficiency, discrete H-Bridge motor driver designed for micro-robotics. 

## Technical Specifications
- **Type:** Discrete Complementary MOSFET H-Bridge.
- **MOSFETs:** UT2305G (P-Ch) & UTM2054G (N-Ch) logic-level pairs.
- **Logic Level:** 3.3V compatible (Tested with STM32).
- **Protection:** Passive Hardware Dead-Time & Reverse Polarity Protection.

## Passive Dead-Time Design
This project uses an RC-D circuit (47R, 2.2nF, 1N4148) to create asymmetrical switching times, preventing shoot-through without complex MCU PWM dead-time configuration.

## Documentation
For in-depth technical details, design philosophy, and fabrication guides, please refer to the [official documentation](docs/README.md).

## Micromouse Inspiration
To understand the competitive robotics context for which KISRAK-M1 is designed, watch this excellent overview of Micromouse competitions:

[![Micromouse Competition](https://img.youtube.com/vi/ZMQbHMgK2rw/0.jpg)](https://www.youtube.com/watch?v=ZMQbHMgK2rw)