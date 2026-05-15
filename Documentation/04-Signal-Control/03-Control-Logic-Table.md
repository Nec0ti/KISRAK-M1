# Control Logic Table for MCUs

Interfacing the KISRAK-M1 motor driver with a microcontroller (MCU) like an STM32 requires a clear understanding of the control signals needed to achieve desired motor movements. The KISRAK-M1 H-bridge is designed to be controlled by two PWM signals and two direction signals per motor, or a simplified two-pin control if the MCU can generate complementary PWM with dead-time. For simplicity and maximum flexibility, we'll consider a common four-pin control scheme per H-bridge, where two pins control the high-side MOSFETs and two pins control the low-side MOSFETs.

However, a more common and efficient approach for a full H-bridge is to use two PWM signals (one for each half-bridge) and two direction pins, or even just two PWM pins if the driver handles the complementary logic internally. Given the discrete nature of KISRAK-M1, the most direct control involves setting the gate signals for each of the four MOSFETs.

Let's define the control pins for one motor (one H-bridge):

*   `PWM_A`: Controls the high-side P-Channel MOSFET (e.g., Q1) and low-side N-Channel MOSFET (e.g., Q3) of one half-bridge.
*   `PWM_B`: Controls the high-side P-Channel MOSFET (e.g., Q2) and low-side N-Channel MOSFET (e.g., Q4) of the other half-bridge.

The KISRAK-M1 uses a complementary drive scheme where for each half-bridge, when the high-side P-channel is ON, the low-side N-channel is OFF, and vice-versa, with dead-time in between. The input to the Totem-Pole driver will determine the state of the MOSFETs.

Let's assume a simplified control where two input pins (`IN1`, `IN2`) from the MCU control the direction and speed of one motor. The internal logic of the KISRAK-M1 (via the Totem-Pole drivers and dead-time circuits) will then generate the appropriate gate signals for the four power MOSFETs.

## Simplified Control Logic (Two-Pin Input per Motor):

This table illustrates how two input pins from an MCU can control the direction and speed of a single motor connected to the KISRAK-M1 H-bridge. The PWM signal is applied to one of the input pins, while the other is held HIGH or LOW to set the direction.

| `IN1` (MCU Pin) | `IN2` (MCU Pin) | Motor Action                               |
| :-------------- | :-------------- | :----------------------------------------- |
| PWM             | LOW             | Motor rotates in Direction 1 (Speed controlled by PWM duty cycle) |
| LOW             | PWM             | Motor rotates in Direction 2 (Speed controlled by PWM duty cycle) |
| HIGH            | HIGH            | Motor Brake (Fast Decay / Short Brake)     |
| LOW             | LOW             | Motor Freewheel (Coast / Slow Decay)       |

### Explanation:

*   **PWM / LOW (Direction 1):** When `IN1` receives a PWM signal and `IN2` is held LOW, the H-bridge is configured to drive the motor in one direction. The duty cycle of the PWM on `IN1` determines the average voltage applied to the motor, thus controlling its speed.
*   **LOW / PWM (Direction 2):** Conversely, when `IN2` receives a PWM signal and `IN1` is held LOW, the motor rotates in the opposite direction, with speed controlled by the PWM on `IN2`.
*   **HIGH / HIGH (Fast Decay / Short Brake):** Both inputs HIGH effectively turns on both low-side MOSFETs (or high-side, depending on the exact internal logic of the driver stage), shorting the motor terminals. This provides a strong, rapid braking action by dissipating the motor's kinetic energy as heat in its windings.
*   **LOW / LOW (Slow Decay / Coast):** Both inputs LOW effectively turns off all active drive to the motor, allowing it to freewheel. The motor's back-EMF will circulate current through the freewheeling diodes and the low-side MOSFETs (if they are configured to be ON during this state), leading to a slower deceleration.

## Detailed Control Logic (Four-Pin Input per Motor - for direct gate control understanding):

For a deeper understanding, if the MCU were to directly control the gate drivers for each of the four MOSFETs in the H-bridge (assuming `GH1`, `GL1` for one half-bridge and `GH2`, `GL2` for the other, where `G` is Gate, `H` is High-side, `L` is Low-side, and 1/2 denote the half-bridge):

| `GH1` | `GL1` | `GH2` | `GL2` | Motor Action                               |
| :---- | :---- | :---- | :---- | :----------------------------------------- |
| PWM   | LOW   | LOW   | HIGH  | Motor rotates in Direction 1 (Speed controlled by PWM) |
| LOW   | HIGH  | PWM   | LOW   | Motor rotates in Direction 2 (Speed controlled by PWM) |
| LOW   | HIGH  | LOW   | HIGH  | Motor Brake (Fast Decay)                   |
| HIGH  | LOW   | HIGH  | LOW   | Motor Brake (Fast Decay)                   |
| LOW   | LOW   | LOW   | LOW   | Motor Freewheel (Coast)                    |

*Note: This table assumes active-high logic for N-channel MOSFETs and active-low for P-channel MOSFETs, and that the PWM is applied to the appropriate high-side or low-side switch for the desired direction. The actual implementation in KISRAK-M1 uses the Totem-Pole drivers to translate simpler MCU signals into the complex gate drive waveforms, including dead-time.*

Firmware developers should refer to the KISRAK-M1 schematic to understand the exact mapping of MCU pins to the driver's input signals. The simplified two-pin control per motor is generally preferred as it reduces the number of GPIOs required and offloads some of the complex timing (like dead-time generation) to the hardware.

This control logic forms the basis for implementing various motor control algorithms, including PID control for speed and position, crucial for high-performance robotic applications like Micromouse.