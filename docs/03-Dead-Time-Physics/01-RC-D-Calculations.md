# RC-D Circuit Time Constant Calculations

In an H-bridge motor driver, **dead-time** is a crucial concept referring to a short delay intentionally inserted between the turn-off of one MOSFET and the turn-on of its complementary MOSFET in the same half-bridge leg. This delay is essential to prevent **cross-conduction** (also known as shoot-through), a condition where both MOSFETs in a half-bridge are simultaneously ON, creating a direct short circuit across the power supply.

KISRAK-M1 implements a passive dead-time generation circuit using an RC-D (Resistor-Capacitor-Diode) network. This circuit, typically consisting of a 47Ω resistor, a 2.2nF capacitor, and a 1N4148 small signal diode, is placed in the gate drive path to introduce the necessary delay.

## The RC-D Network:

The RC-D circuit is designed to create an asymmetric delay in the gate signal.

*   **Resistor (R):** 47Ω
*   **Capacitor (C):** 2.2nF
*   **Diode (D):** 1N4148

### Time Constant ($\tau$) Calculation:

The fundamental characteristic of an RC circuit is its **time constant ($\tau$)**, which represents the time it takes for the capacitor voltage to reach approximately 63.2% of its final value during charging or to discharge to 36.8% of its initial value. It is calculated as:

$$\tau = R \times C$$

For the KISRAK-M1's RC-D circuit:
*   $R = 47 \, \Omega$
*   $C = 2.2 \, nF = 2.2 \times 10^{-9} \, F$

Therefore, the time constant is:
$$\tau = 47 \, \Omega \times 2.2 \times 10^{-9} \, F = 103.4 \times 10^{-9} \, s = 103.4 \, ns$$

This time constant of approximately 103.4 nanoseconds is a key parameter in determining the dead-time. It dictates the charging time of the capacitor, which in turn influences the turn-on delay of the MOSFET.

## Role of the Diode (1N4148):

The 1N4148 small signal diode is critical for creating the **asymmetric switching** behavior.

*   **Charging (Turn-ON Delay):** When the gate drive signal goes high, the capacitor charges through the resistor (R). The diode is reverse-biased during this phase (or has minimal effect if placed in parallel with R for charging), and the charging path is primarily through R and C. The time constant $\tau = R \times C$ governs this charging.
*   **Discharging (Turn-OFF Speed):** When the gate drive signal goes low, the capacitor needs to discharge quickly to turn off the MOSFET. The diode is placed in parallel with the resistor, but oriented to provide a low-resistance path for the discharge current, effectively bypassing the resistor. This allows the capacitor to discharge much faster than it charged, leading to a rapid turn-off.

The 1N4148 is a fast switching diode, which is important for ensuring that the discharge path is indeed low impedance and that the diode itself does not introduce significant delays.

## Importance of Dead-Time:

The calculated time constant and the asymmetric switching provided by the RC-D network directly contribute to the dead-time. This dead-time ensures that:

1.  One MOSFET is fully OFF before its complementary MOSFET begins to turn ON.
2.  This prevents shoot-through currents, which can damage the MOSFETs and waste significant power.

The precise value of dead-time is a trade-off. Too short, and shoot-through can occur. Too long, and efficiency can be reduced due to increased conduction losses in the body diodes of the MOSFETs during the dead-time period. The 103.4ns time constant provides a suitable delay for the chosen MOSFETs and switching frequencies in KISRAK-M1.

Datasheet for 1N4148: [1N4148WS Datasheet](../Docs/datasheets/1n4148ws-datasheet.pdf)