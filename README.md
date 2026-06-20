# Aegis-Grid: 4-Bit Binary Target Tracker

A fully hardware-based 4x4 autonomous target-tracking and fire-control system implemented using standard Transistor-Transistor Logic (TTL) ICs. The system dynamically captures coordinate data from a matrix of light sensors, evaluates it against a predefined guard coordinate, and manages a secure firing sequence—entirely bypassing the need for microcontrollers or software programming.

### 🌟 Key Features
* **Matrix Grid Mapping:** Efficiently tracks a 16-position grid using only 8 LDR sensors (4 for the X-axis, 4 for the Y-axis) to optimize component use and simplify wiring.
* **Priority Signal Handling:** Utilizes inherent priority encoding to natively resolve multi-target interference (multiple laser hits) without causing logical deadlocks.
* **Dual-Condition Interlock:** Employs a hardware safety lock that requires both a perfect spatial coordinate match and a physical "Master Arm" switch engagement to trigger the final payload.
* **Live Status Indicators:** Real-time visual feedback using LEDs to show if the target is Left/Up (Yellow), Right/Down (Blue), Locked (Green), or Firing (Red).

### 🏗️ Technical Architecture
The system processes signals through a zero-latency, 5-stage hardware pipeline:
* **The Matrix (Sensors):** 8 Light Dependent Resistors (LDRs) configured in active-low voltage dividers.
* **Coordinate Encoding (`74LS148`):** Two priority encoders compress the physical grid hits into a 2-bit active-low binary coordinate per axis.
* **Bit Inversion (`74LS04`):** A hex inverter flips the active-low signals back to normal, active-high unsigned binary formats.
* **Magnitude Comparison (`74LS85`):** A 4-bit comparator continuously matches the live sensor target position (Input B) against the hardcoded Guard Position (Input A) set via DIP switches.
* **Fire Logic (`74LS08`):** An AND gate executes the final interlock logic, outputting HIGH only when A=B AND the Master Arm toggle is ON.

### 📂 Component Inventory
* `2x 74148` — Priority Encoders (X and Y axes coordinate translation)
* `1x 7485` — 4-bit Magnitude Comparator (Decision brain)
* `1x 7404` — NOT Gate (Logic Inverter)
* `1x 7408` — AND Gate (Safety interlock execution)
* `1x LM7805` — Voltage Regulator (Maintains strict 5V rails)
* `8x LDRs` — Light sensors for grid tracking
* `1x 4-DIP Switch` — Manual target coordinate input

### 🚀 How to Operate
1. **Power Up:** Connect a 9V battery or DC supply to the breadboard (regulated cleanly down to 5V via the LM7805).
2. **Set the Guard Position:** Use the 4-DIP switch array to set a target 4-bit binary coordinate (A3-A0).
3. **Track:** Move a light source across the LDR grid. Watch the Blue and Yellow LEDs track the target's relative magnitude.
4. **Lock & Fire:** When the target hits the exact Guard Position, the Green LED activates. Flip the Master Arm safety switch to trigger the final Red Fire LED and buzzer.
