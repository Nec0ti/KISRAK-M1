# Cross-Conduction (Shoot-through) Risk Analysis and Mitigation

**Cross-conduction**, often referred to as **shoot-through**, is a critical failure mode in H-bridge motor drivers that can lead to catastrophic damage to the power stage MOSFETs and significant power loss. It occurs when both the high-side and low-side MOSFETs in the same half-bridge leg are simultaneously turned ON, even for a very brief period.

## The Mechanism of Cross-Conduction:

Consider a single half-bridge leg consisting of a high-side MOSFET (e.g., P-Channel) and a low-side MOSFET (e.g., N-Channel). Ideally, when one is ON, the other must be OFF.

*   If the high-side MOSFET is still conducting (or hasn't fully turned OFF) when the low-side MOSFET begins to turn ON, a direct, low-resistance path is created between the positive power supply rail ($V_{bat}$) and ground.
*   This creates a very large, uncontrolled current surge that bypasses the motor load, flowing directly through both MOSFETs.
*   This surge current is limited only by the parasitic resistances and inductances of the power traces and the MOSFETs themselves.

## Consequences of Cross-Conduction:

1.  **Catastrophic MOSFET Failure:** The extremely high current flowing through the MOSFETs generates immense heat in a very short period ($P = I^2 R$). This can quickly exceed the MOSFETs' thermal limits, leading to permanent damage, often manifesting as a short circuit within the device.
2.  **Power Supply Stress:** The sudden, large current draw can stress the power supply, potentially causing voltage dips or triggering overcurrent protection.
3.  **Reduced Efficiency:** Even if the MOSFETs survive, repeated shoot-through events (even brief ones) lead to significant power dissipation, drastically reducing the overall efficiency of the motor driver.
4.  **Electromagnetic Interference (EMI):** The rapid current spikes generate considerable EMI, which can interfere with other sensitive electronic components in the system.

## Sources of Cross-Conduction Risk:

*   **Propagation Delays:** Different MOSFETs and gate drivers have varying turn-on and turn-off propagation delays. If not properly accounted for, these delays can cause overlap.
*   **Gate Charge/Discharge Times:** The time it takes to charge and discharge the gate capacitance of a MOSFET directly impacts its switching speed. If the gate is not fully discharged before the complementary MOSFET turns on, shoot-through can occur.
*   **Miller Effect:** The Miller effect can slow down the turn-off process, especially for the high-side MOSFET, making it more susceptible to overlap.
*   **Noise:** Electrical noise on the gate drive signals can inadvertently turn on a MOSFET.

## KISRAK-M1's Mitigation Strategy: Dead-Time Insertion

The primary strategy employed in KISRAK-M1 to eliminate the risk of cross-conduction is the careful insertion of **dead-time**. As detailed in the previous sections, the RC-D network (47Ω, 2.2nF, 1N4148) is specifically designed for this purpose.

### How the RC-D Circuit Eliminates Shoot-Through:

1.  **Asymmetric Switching:** The RC-D circuit ensures that the turn-on of a MOSFET is intentionally delayed (slower rising edge) while its turn-off is accelerated (faster falling edge).
2.  **Guaranteed OFF State:** By delaying the turn-on, the circuit guarantees a period (the dead-time) during which both the high-side and low-side MOSFETs in a half-bridge are in the OFF state.
3.  **Sequential Operation:**
    *   When switching from one MOSFET to its complement, the currently ON MOSFET is first commanded to turn OFF.
    *   The RC-D circuit ensures a rapid discharge of its gate, quickly turning it OFF.
    *   Only after a predefined dead-time (determined by the RC time constant and the asymmetric switching) does the complementary MOSFET begin its (slower) turn-on sequence.
    *   This sequential operation, with a guaranteed non-overlap period, effectively prevents any direct short-circuit path from forming across the power rails.

### The Role of the 1N4148 Diode:

The 1N4148 diode is crucial for creating the fast discharge path, ensuring that the MOSFET turns off quickly. This rapid turn-off is just as important as the delayed turn-on in preventing shoot-through. If the turn-off is too slow, the dead-time might not be sufficient to prevent overlap.

By carefully designing the RC-D network to provide an appropriate dead-time, KISRAK-M1 effectively eliminates the risk of cross-conduction, ensuring the robustness, efficiency, and longevity of the motor driver. This passive dead-time generation method is simple, reliable, and highly effective for the intended application.