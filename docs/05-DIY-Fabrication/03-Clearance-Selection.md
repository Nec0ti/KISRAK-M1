# 0.4mm Clearance Selection for Voltage Isolation

In PCB design, **clearance** refers to the minimum distance between two uninsulated conductors. This distance is critical for ensuring electrical isolation, preventing short circuits, and mitigating the risk of voltage breakdown or arcing, especially in the presence of contaminants or humidity. For KISRAK-M1, a 0.4mm (approximately 15.7 mil) clearance has been selected as a practical and safe value for DIY single-layer PCB fabrication.

## Importance of Clearance:

1.  **Preventing Short Circuits:** The most immediate function of clearance is to physically separate conductors, preventing unintended electrical connections.
2.  **Voltage Breakdown (Dielectric Strength):** Air (or the PCB substrate) acts as an insulator. If the voltage difference between two conductors exceeds the dielectric strength of the insulating medium over a given distance, an electrical breakdown can occur, leading to arcing or flashover.
3.  **Contamination and Humidity:** Dust, dirt, moisture, and other contaminants on the PCB surface can reduce the effective insulation distance, making the board more susceptible to leakage currents or breakdown. Larger clearances provide a greater margin of safety against these environmental factors.
4.  **Manufacturing Tolerances:** PCB manufacturing processes have inherent tolerances. A larger clearance provides a buffer against slight inaccuracies in etching or drilling, ensuring that traces remain adequately separated.

## Factors Influencing Clearance Selection:

1.  **Voltage Difference:** The primary factor is the maximum voltage difference that can exist between adjacent conductors. Higher voltages require larger clearances. KISRAK-M1 operates at relatively low voltages (typically 3.7V to 8.4V from a LiPo battery), which allows for smaller clearances compared to high-voltage applications.
2.  **Pollution Degree:** This refers to the amount of conductive contamination expected in the operating environment.
    *   **Pollution Degree 1:** No pollution or only dry, non-conductive pollution.
    *   **Pollution Degree 2:** Only non-conductive pollution, but condensation may occur.
    *   **Pollution Degree 3:** Conductive pollution, or dry non-conductive pollution that becomes conductive due to condensation.
    *   Micromouse robots often operate in indoor, relatively clean environments, but dust and occasional humidity are possible, suggesting Pollution Degree 2.
3.  **Insulating Material:** The PCB substrate material (e.g., FR-4) has its own dielectric properties.
4.  **Manufacturing Process:** DIY toner transfer etching typically has lower precision than professional PCB fabrication. This necessitates more generous clearances to ensure reliable results.

## Why 0.4mm for KISRAK-M1?

*   **Safety Margin for Low Voltage:** For the typical operating voltages of KISRAK-M1 (up to ~8.4V), a 0.4mm clearance provides a very generous safety margin against voltage breakdown in air. Standard industrial guidelines (e.g., IPC-2221) suggest much smaller clearances for these voltage levels (e.g., 0.1mm for up to 15V in Pollution Degree 2).
*   **DIY Fabrication Robustness:** The primary reason for choosing 0.4mm is to ensure high success rates for DIY toner transfer etching.
    *   **Etching Consistency:** Achieving fine details and consistent trace/space widths can be challenging with toner transfer. A 0.4mm clearance is relatively easy to achieve consistently, reducing the risk of unintended shorts due to over-etching or under-etching.
    *   **Toner Transfer Quality:** Ensuring complete toner transfer for thin isolation lines between copper areas is easier with wider gaps.
*   **Mechanical Robustness:** Wider clearances also make the board more mechanically robust against minor scratches or debris that could otherwise bridge smaller gaps.

## Impact on Design:

While a 0.4mm clearance provides excellent reliability for DIY fabrication, it does come with a trade-off:

*   **Increased Board Size:** Larger clearances mean that traces cannot be packed as densely, potentially leading to a larger overall PCB footprint. For Micromouse, where space is at a premium, this is a significant consideration.
*   **Routing Challenges:** It can make routing more challenging on a single-layer board, as it limits the available space for traces.

The KISRAK-M1 design balances these factors, prioritizing ease of DIY fabrication and reliability over absolute minimum board size. The chosen 0.4mm clearance ensures that even with less precise home etching methods, the electrical isolation is robust, contributing to a reliable and safe motor driver.