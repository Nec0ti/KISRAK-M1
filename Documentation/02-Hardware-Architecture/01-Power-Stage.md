# Power Stage: MOSFET Selection and Thermal Efficiency

The power stage of the KISRAK-M1 motor driver is a critical component, directly responsible for delivering current to the motor. It utilizes a full H-bridge configuration built with discrete P-Channel (UT2305G) and N-Channel (UTM2054G) MOSFETs. The selection of these specific MOSFETs is driven by a careful balance of performance, efficiency, and cost, with a strong emphasis on minimizing power losses and managing thermal dissipation.

## MOSFET Selection Criteria:

1.  **Voltage Rating ($V_{DS}$):** The drain-source voltage rating must be sufficiently higher than the maximum expected battery voltage to provide a safety margin against voltage spikes and transients. For KISRAK-M1, operating typically from a 1S or 2S LiPo battery (3.7V - 8.4V nominal), MOSFETs with a $V_{DS}$ rating of 20V or 30V are generally suitable. The UT2305G (P-Channel) and UTM2054G (N-Channel) both offer a 20V rating, providing an adequate margin.
2.  **Current Rating ($I_D$):** The continuous drain current rating must exceed the maximum expected motor current. Micromouse motors can draw significant peak currents during acceleration. The selected MOSFETs offer continuous drain currents (e.g., UT2305G: -4.6A, UTM2054G: 5.3A at $V_{GS}=4.5V$), which are sufficient for typical Micromouse applications, especially when considering the pulsed nature of PWM.
3.  **Gate-Source Voltage ($V_{GS}$):** The MOSFETs must be fully turned on (saturated) by the gate drive voltage provided by the microcontroller or gate driver. Both selected MOSFETs are "logic-level" compatible, meaning they can be fully turned on with a $V_{GS}$ as low as 2.5V or 4.5V, making them suitable for direct driving from a 3.3V or 5V microcontroller.
4.  **Gate Charge ($Q_g$):** This parameter is crucial for switching losses. A lower gate charge means less energy is required to charge and discharge the gate capacitance, leading to faster switching times and reduced switching losses, especially at higher PWM frequencies.
5.  **On-State Resistance ($R_{DS(on)}$):** This is arguably the most critical parameter for thermal efficiency in the power stage.

## The Impact of $R_{DS(on)}$ on Thermal Efficiency:

$R_{DS(on)}$ is the resistance between the drain and source terminals of a MOSFET when it is fully turned on. When current flows through the MOSFET in its on-state, power is dissipated as heat due to this resistance. This power loss is calculated by:

$$P_{conduction} = I_D^2 \times R_{DS(on)}$$

Where:
*   $P_{conduction}$ is the power dissipated as heat due to conduction.
*   $I_D$ is the drain current flowing through the MOSFET.
*   $R_{DS(on)}$ is the on-state resistance.

**Why lower $R_{DS(on)}$ is crucial:**

*   **Reduced Heat Generation:** A lower $R_{DS(on)}$ directly leads to less power being converted into heat for a given current. This is vital for compact designs like KISRAK-M1, where heat dissipation can be challenging.
*   **Improved Efficiency:** Less power lost as heat means more power delivered to the motor, increasing the overall efficiency of the motor driver. This translates to longer battery life and better performance.
*   **Prevention of Thermal Runaway:** Excessive heat can increase $R_{DS(on)}$ (for N-channel MOSFETs, $R_{DS(on)}$ typically increases with temperature), leading to a positive feedback loop where more heat generates more resistance, leading to even more heat. This can eventually destroy the MOSFET. Selecting MOSFETs with very low $R_{DS(on)}$ helps mitigate this risk.
*   **Smaller Heatsinking Requirements:** With less heat generated, the need for large heatsinks or extensive copper pours for thermal dissipation is reduced, allowing for a more compact PCB design.

### Selected MOSFETs:

*   **P-Channel MOSFET (High-Side): UT2305G**
    *   $V_{DS}$: -20V
    *   $I_D$: -4.6A
    *   $R_{DS(on)}$: 45mΩ @ $V_{GS}=-4.5V$, 65mΩ @ $V_{GS}=-2.5V$
    *   Datasheet: [UT2305G Datasheet](../Docs/datasheets/ut2305g-datasheet.pdf)

*   **N-Channel MOSFET (Low-Side): UTM2054G**
    *   $V_{DS}$: 20V
    *   $I_D$: 5.3A
    *   $R_{DS(on)}$: 25mΩ @ $V_{GS}=4.5V$, 35mΩ @ $V_{GS}=2.5V$
    *   Datasheet: [UTM2054G Datasheet](../Docs/datasheets/utm2054g-datasheet.pdf)

The chosen MOSFETs offer a good balance of low $R_{DS(on)}$ for their package size (SOT-23), sufficient current and voltage ratings, and logic-level compatibility, making them ideal for the KISRAK-M1's high-performance, compact design. The difference in $R_{DS(on)}$ between P-channel and N-channel MOSFETs is typical, as P-channel devices generally have higher on-resistance for a given die size due to the lower mobility of holes compared to electrons. This is why N-channel MOSFETs are preferred for low-side switching where possible.