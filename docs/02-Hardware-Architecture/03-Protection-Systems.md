# Protection Systems: Reverse Polarity and Inductive Kickback

Robust protection systems are paramount for the reliability and longevity of any motor driver, especially in applications where power sources can be inadvertently connected incorrectly or where inductive loads are switched rapidly. KISRAK-M1 incorporates two key protection mechanisms: reverse polarity protection and inductive kickback suppression.

## Q7 Reverse Polarity Protection:

Accidental reverse connection of the battery is a common mistake that can instantly destroy unprotected electronic circuits. KISRAK-M1 employs a P-Channel MOSFET (Q7) for reverse polarity protection, offering a more efficient solution than a simple series diode.

### Mechanism:

A P-Channel MOSFET is placed in series with the positive power supply line, with its drain connected to the battery's positive terminal and its source connected to the rest of the circuit's positive rail. The gate of the P-Channel MOSFET is connected to ground (or a voltage divider that ensures $V_{GS}$ is appropriate).

*   **Correct Polarity Connection:**
    *   When the battery is connected correctly, the positive terminal is at a higher potential than the gate (which is at ground).
    *   This creates a negative $V_{GS}$ (e.g., $V_{GS} = V_{gate} - V_{source} = 0V - V_{battery}$).
    *   If the magnitude of this negative $V_{GS}$ is greater than the MOSFET's threshold voltage ($V_{GS(th)}$), the P-Channel MOSFET turns ON, allowing current to flow from the battery to the circuit.
    *   The MOSFET's body diode will conduct initially, and once the MOSFET turns on, current flows through the channel, bypassing the body diode and significantly reducing voltage drop and power loss compared to a standard diode.

*   **Reverse Polarity Connection:**
    *   When the battery is connected in reverse, the positive terminal of the battery (now effectively ground) is connected to the drain, and the negative terminal (now at a negative potential) is connected to the source.
    *   The gate is still at ground.
    *   In this scenario, the $V_{GS}$ becomes positive (e.g., $V_{GS} = V_{gate} - V_{source} = 0V - (-V_{battery}) = +V_{battery}$).
    *   A positive $V_{GS}$ keeps the P-Channel MOSFET firmly OFF.
    *   Crucially, the body diode of the P-Channel MOSFET will be reverse-biased, preventing current flow into the circuit. The circuit remains protected.

### Advantages over Series Diode:

*   **Lower Voltage Drop:** A P-Channel MOSFET in its ON state has a very low $R_{DS(on)}$, resulting in a minimal voltage drop across it (typically tens of millivolts). A standard silicon diode, in contrast, has a forward voltage drop of 0.7V or more, leading to significant power loss and heat generation, especially at higher currents.
*   **Higher Efficiency:** Reduced voltage drop means less power is wasted as heat, improving the overall efficiency of the system.
*   **Less Heat:** Lower power dissipation means less heat generated, simplifying thermal management.

## SS34 Schottky Diodes for Inductive Kickback Suppression:

Motors are inductive loads. When the current flowing through an inductor is rapidly switched off (e.g., when a MOSFET in the H-bridge turns off), the inductor resists this change by generating a large voltage spike (known as "inductive kickback" or "flyback voltage") with a polarity that tries to maintain the current flow. This voltage spike can easily exceed the breakdown voltage of the MOSFETs, leading to their destruction.

KISRAK-M1 utilizes SS34 Schottky diodes as **freewheeling diodes** (also known as flyback diodes or snubber diodes) to protect the MOSFETs from these destructive voltage spikes.

### Mechanism:

In an H-bridge, each MOSFET has a freewheeling diode connected in parallel, but in reverse bias across its drain and source. When a MOSFET turns off, and the motor's inductance tries to maintain current flow, the voltage across the MOSFET rises. As soon as this voltage exceeds the forward voltage of the freewheeling diode, the diode turns ON, providing a path for the inductive current to circulate. This clamps the voltage across the MOSFET to a safe level (approximately $V_{supply} + V_{diode\_forward\_drop}$).

### Why Schottky Diodes (SS34)?

Schottky diodes are preferred over standard silicon diodes for freewheeling applications due to their:

1.  **Low Forward Voltage Drop ($V_F$):** SS34 has a typical $V_F$ of around 0.5V. A lower $V_F$ means less power is dissipated in the diode itself and, more importantly, the voltage spike seen by the MOSFET is clamped to a lower value, providing better protection.
2.  **Fast Recovery Time:** Schottky diodes have very fast reverse recovery times (ideally zero). This is crucial in high-frequency PWM applications, as it prevents current spikes during the transition when the diode switches from conducting to blocking. A slow recovery diode can momentarily act as a short circuit, leading to shoot-through or increased switching losses.

### Inductive Kickback Suppression Mathematics:

The energy stored in an inductor is given by:
$$E_L = \frac{1}{2} L I^2$$
Where:
*   $E_L$ is the stored energy in Joules.
*   $L$ is the inductance in Henrys.
*   $I$ is the current in Amperes.

When the switch opens, this energy must be dissipated. Without a freewheeling diode, the voltage across the switch can rise to very high levels, limited only by parasitic capacitances and the breakdown voltage of the switch. With a freewheeling diode, the inductive current is diverted through the diode and the power supply (or another part of the H-bridge), dissipating the energy safely.

The voltage across the MOSFET when the diode conducts will be approximately:
$$V_{MOSFET} \approx V_{supply} + V_{F(diode)}$$
This ensures that the MOSFET's $V_{DS}$ rating is not exceeded.

### Selected Schottky Diode: SS34

*   $V_{RRM}$: 40V
*   $I_F$: 3A
*   $V_F$: 0.5V @ 3A
*   Datasheet: [SS34 Datasheet](../datasheets/ss34-datasheet.pdf)

The SS34 provides excellent protection against inductive kickback due to its low forward voltage and fast switching characteristics, ensuring the longevity and reliability of the KISRAK-M1 motor driver.