# PWM Frequency Selection: Why 8kHz - 20kHz is Ideal

Pulse Width Modulation (PWM) is the fundamental technique I use in KISRAK-M1 to control the speed and direction of DC motors. The choice of PWM frequency is a critical design parameter that impacts motor performance, efficiency, audible noise, and electromagnetic interference (EMI). For KISRAK-M1, I have selected an ideal PWM frequency range of 8kHz to 20kHz based on a balance of these factors.

## Factors Influencing PWM Frequency Selection:

1.  **Audible Noise:**
    *   Human hearing typically ranges from 20Hz to 20kHz.
    *   PWM frequencies below 8kHz can often produce an audible whining or buzzing sound from the motor, which can be undesirable in many applications, especially in quiet environments or for user experience.
    *   By selecting a frequency above 8kHz, the switching noise is pushed towards the upper limit of human hearing, making it less noticeable or inaudible. Frequencies above 20kHz are generally considered ultrasonic and completely inaudible to humans.

2.  **Motor Inductance and Current Ripple:**
    *   DC motors are inductive loads. When a PWM voltage is applied, the motor's inductance smooths out the current, preventing it from rising and falling instantaneously.
    *   A higher PWM frequency results in a smaller current ripple (peak-to-peak variation in motor current).
    *   **Benefits of smaller current ripple:**
        *   **Reduced Motor Heating:** Large current ripples increase $I^2 R$ losses in the motor windings, leading to more heat.
        *   **Smoother Operation:** Less ripple results in smoother torque delivery and reduced vibrations.
        *   **Improved Efficiency:** Lower ripple current generally means better motor efficiency.
    *   However, excessively high frequencies can lead to increased switching losses in the MOSFETs (see point 3).

3.  **MOSFET Switching Losses:**
    *   Every time a MOSFET switches (turns ON or OFF), there are associated switching losses. These losses are proportional to the switching frequency.
    *   $$P_{switching} = (E_{on} + E_{off}) \times f_{PWM}$$
        Where $E_{on}$ and $E_{off}$ are the energy lost during turn-on and turn-off transitions, respectively.
    *   As the PWM frequency increases, switching losses increase. This generates more heat in the MOSFETs, potentially reducing efficiency and requiring more robust thermal management.
    *   A balance must be struck: high enough frequency for low audible noise and current ripple, but not so high that MOSFETs overheat.

4.  **Microcontroller Capabilities:**
    *   The chosen PWM frequency must be achievable by the microcontroller (MCU) generating the PWM signal. Most modern MCUs (like STM32) can easily generate PWM signals in the 8kHz-20kHz range with sufficient resolution (e.g., 8-bit or 10-bit duty cycle).
    *   Higher frequencies demand faster clock speeds and more sophisticated timer peripherals, which might limit duty cycle resolution.

5.  **Electromagnetic Interference (EMI):**
    *   Rapid switching of high currents can generate EMI. Higher frequencies can sometimes lead to more challenging EMI suppression, as the energy content shifts to higher frequency bands. However, well-controlled switching transitions (as achieved with the Totem-Pole driver) can help manage EMI.

## Why 8kHz - 20kHz is Ideal for KISRAK-M1:

*   **8kHz as a Lower Bound:** This frequency is generally considered the minimum to push most of the audible switching noise out of the most sensitive human hearing range. It provides a good balance for applications where some faint noise might be acceptable, but efficiency is paramount.
*   **20kHz as an Upper Bound:** This frequency is at the very edge of human hearing, making the motor virtually silent. It also provides a good balance with MOSFET switching losses for the chosen discrete components (UT2305G, UTM2054G) and their gate drive characteristics. Beyond 20kHz, the increase in switching losses often outweighs the benefits of further reduced current ripple or inaudibility for typical Micromouse motors.

This range ensures a quiet, efficient, and responsive motor control experience, perfectly suited for the demanding environment of Micromouse robotics. The KISRAK-M1's design, with its optimized gate drive, is capable of handling these frequencies effectively.