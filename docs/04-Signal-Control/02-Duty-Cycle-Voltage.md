# Duty Cycle and Average Voltage Calculation

Pulse Width Modulation (PWM) is a powerful technique for controlling the effective voltage supplied to a DC motor, thereby regulating its speed and torque. The core concept of PWM revolves around varying the **duty cycle** of a square wave signal.

## Understanding Duty Cycle:

The duty cycle ($D$) of a PWM signal is defined as the ratio of the pulse ON time ($T_{on}$) to the total period ($T$) of the signal. It is typically expressed as a percentage or a fraction between 0 and 1.

$$D = \frac{T_{on}}{T}$$

Where:
*   $T_{on}$ is the duration for which the signal is HIGH (ON).
*   $T$ is the total period of one PWM cycle ($T = T_{on} + T_{off}$).

## Average Voltage Calculation:

For a DC motor, which acts as an inductive load, the mechanical inertia and electrical inductance effectively average out the rapidly switching PWM voltage. The motor responds to the **average voltage** applied across its terminals.

The average voltage ($V_{out}$) delivered to the motor can be calculated directly from the battery voltage ($V_{bat}$) and the PWM duty cycle ($D$):

$$V_{out} = V_{bat} \times D$$

**Example:**
If the battery voltage ($V_{bat}$) is 7.4V (2S LiPo nominal) and the PWM duty cycle ($D$) is 50% (or 0.5), the average voltage applied to the motor will be:

$$V_{out} = 7.4V \times 0.5 = 3.7V$$

If the duty cycle is 100% (or 1), the motor receives the full battery voltage ($V_{out} = V_{bat}$). If the duty cycle is 0% (or 0), the motor receives 0V ($V_{out} = 0$).

## Implications for Motor Control:

*   **Speed Control:** By varying the duty cycle from 0% to 100%, the average voltage supplied to the motor can be smoothly controlled from 0V to $V_{bat}$, allowing for precise speed regulation.
*   **Torque Control:** Since motor torque is proportional to the current, and current is influenced by the applied voltage, duty cycle also indirectly controls the motor's torque output.
*   **Efficiency:** While PWM is efficient, it's important to note that the motor's efficiency can vary with speed and load. The KISRAK-M1 driver aims to deliver the PWM signal to the motor with minimal losses in the power stage.

## Practical Considerations:

*   **PWM Resolution:** Microcontrollers generate PWM signals using timers. The resolution of the duty cycle (how many distinct steps between 0% and 100%) depends on the timer's period and clock frequency. Higher resolution allows for finer control over motor speed.
*   **Minimum/Maximum Duty Cycle:** In practice, it's often not possible or desirable to use 0% or 100% duty cycle due to dead-time requirements, gate driver limitations, or motor characteristics. The effective range might be slightly narrower (e.g., 1% to 99%).
*   **Motor Characteristics:** Different motors will respond differently to the same average voltage due to their internal resistance, inductance, and back-EMF characteristics. Firmware must be tuned to the specific motors used.

By understanding the relationship between duty cycle, battery voltage, and average output voltage, firmware developers can accurately implement speed control algorithms for the KISRAK-M1 motor driver.