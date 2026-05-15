# Initial Power-Up Protocol (Smoke Test)

After assembling any new electronic circuit, especially a power electronics board like the KISRAK-M1 motor driver, the very first power-up is a critical moment. This initial test is often referred to as a "smoke test" – the goal being to ensure that no magic smoke escapes from the components, indicating a catastrophic failure. A systematic approach is essential to prevent damage and ensure the board is safe for further testing.

## Objectives of the Smoke Test:

*   **Prevent Catastrophic Failure:** Identify major assembly errors (e.g., shorts, reversed components) before they cause permanent damage.
*   **Verify Basic Functionality:** Confirm that the power rails are stable and that the protection circuits are operational.
*   **Ensure Safety:** Protect both the circuit and the user from potential hazards.

## Recommended Smoke Test Protocol for KISRAK-M1:

**Tools Required:**
*   Current-limited DC power supply (highly recommended) or a battery with a series current-limiting resistor/fuse.
*   Digital Multimeter (DMM).
*   Magnifying glass or microscope (for visual inspection).

---

### Step 1: Thorough Visual Inspection (Power OFF)

Before applying any power, perform a meticulous visual inspection of the entire assembled PCB.

*   **Solder Joints:** Check every solder joint for bridges, cold joints, or insufficient solder. Ensure all pins are properly connected.
*   **Component Orientation:** Verify the correct orientation of all polarized components:
    *   **Diodes:** Ensure the cathode band is in the correct position.
    *   **Electrolytic Capacitors:** Check polarity.
    *   **Integrated Circuits (if any):** Verify pin 1 orientation.
    *   **MOSFETs/BJTs:** Ensure they are not rotated 180 degrees.
*   **Short Circuits:** Look for any accidental solder bridges between adjacent pads or traces. Pay close attention to fine-pitch components.
*   **Cleanliness:** Ensure the board is free of solder balls, wire clippings, or other debris that could cause shorts.

---

### Step 2: Continuity Checks (Power OFF)

Use a DMM in continuity mode to perform several critical checks.

*   **Power Rail to Ground:**
    *   Place one DMM probe on the main power input (e.g., $V_{bat}$) and the other on ground.
    *   There should be NO continuity (open circuit or high resistance). A beep indicates a direct short, which MUST be resolved before proceeding.
    *   Repeat for any other power rails (e.g., 5V logic supply if present).
*   **Motor Outputs to Ground/Power:**
    *   Check for shorts between motor output terminals and ground or $V_{bat}$. There should be no direct shorts.
*   **Component Pins:**
    *   Verify continuity for critical connections (e.g., MOSFET gate to driver output, power pins of ICs).
    *   Check for shorts between adjacent pins of ICs or transistors.

---

### Step 3: Current-Limited Power-Up

This is the actual "smoke test" phase. **Crucially, use a current-limited power supply.**

1.  **Set Power Supply:**
    *   Set the voltage to the lowest operating voltage (e.g., 3.7V for a 1S LiPo).
    *   Set the current limit to a very low value (e.g., 50mA - 100mA). This acts as a safety fuse.
2.  **Connect Power:** Connect the power supply to the KISRAK-M1's power input terminals, ensuring correct polarity.
3.  **Observe:**
    *   Carefully watch the power supply's current meter. If it immediately hits the current limit, there's likely a short circuit. Disconnect power immediately and re-inspect.
    *   Feel components for any signs of excessive heat. Any component getting hot quickly indicates a problem.
    *   Listen for any unusual sounds (e.g., buzzing, crackling).
    *   Smell for any burning odors.
4.  **Measure Voltages:**
    *   If no immediate issues are observed, use the DMM to measure voltages at key points:
        *   Verify the voltage across the main power rails (e.g., $V_{bat}$ after the reverse polarity protection).
        *   Check the logic supply voltage (if applicable).
        *   Measure the voltage at the gate driver outputs (should be low or high depending on the default state).
5.  **Gradually Increase Voltage/Current Limit:**
    *   If all looks good, slowly increase the voltage to the nominal operating voltage (e.g., 7.4V).
    *   Gradually increase the current limit to a higher, but still safe, value (e.g., 500mA - 1A).
    *   Continue to monitor current draw, component temperatures, and voltages. The quiescent current draw (with no motor connected and no PWM applied) should be very low (a few milliamps).

---

### Step 4: Test Protection Circuits (Optional but Recommended)

*   **Reverse Polarity Protection (Q7):** Briefly and carefully connect the power supply in reverse (with a very low current limit, e.g., 10mA). The circuit should draw virtually no current, and no components should heat up. This confirms Q7 is working. Immediately correct the polarity.

---

If the board passes the smoke test, it's ready for functional testing, starting with gate signal analysis. This methodical approach significantly reduces the risk of damaging components and provides confidence in the assembly quality.