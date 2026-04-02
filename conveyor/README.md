# Project Report: PLC Conveyor Station Control System (v3.1)

## 1. Hardware & Software Specifications
This project is built on the Siemens industrial automation ecosystem, ensuring high reliability and standard communication protocols.

### 1.1 Control Hardware (Siemens Ecosystem)
* **PLC CPU:** Siemens SIMATIC **S7-1500** CPU (standard for lab automation).
* **Digital I/O Modules:** * **Inputs:** Interface for cylinder reed switches and photoelectric sensors.
    * **Outputs:** Control for solenoid valves and motor contactors/relays.
* **HMI Panel:** Siemens SIMATIC **KTP Basic/Comfort** Touch Panel.
* **Communication:** All devices are networked via **PROFINET** (Industrial Ethernet).

### 1.2 Actuators & Sensors
* **Conveyor Motor:** DC/AC motor equipped with an **Encoder** to provide feedback for the High-Speed Counter (HSC).
* **Pneumatics:** 2x Double-acting cylinders with 5/2-way solenoid valves.
* **Feedback Sensors:** * 4x Magnetic reed switches for cylinder end-position detection.
    * 1x Photoelectric sensor for workpiece detection at the conveyor end.

### 1.3 Programming Software
* **TIA Portal (Totally Integrated Automation):** Used for Hardware Configuration, PLC logic (LAD/SCL), and HMI screen design (WinCC).

---

## 2. System Functions

### 2.1 Control Modes
* **Manual Mode:** Direct control of actuators via the HMI buttons. 
    * Conveyor: Manual Move Left/Right.
    * Cylinders: Toggle (Extend/Retract) with every button press.
    * **Safety Interlock:** Motor movement is automatically disabled if any cylinder is not fully retracted.
* **Automatic Mode:** Executes the sorting sequence after a successful "Reference Run."

### 2.2 Reference Run (Initialization)
Before the system enters Auto mode, it must be calibrated to a known state:
1.  **Retract All:** All cylinders move to the home position.
2.  **Clear Belt:** Conveyor runs **Right** for 10 seconds.
3.  **Homing:** The run stops if the end sensor detects a workpiece or the timer expires.
4.  **Status:** A "Reference OK" signal is displayed on the HMI.

### 2.3 Monitoring & HMI Interaction
* **Real-time Feedback:** HMI displays current status (e.g., "Reference Run", "Sorting", "Paused").
* **Smart Start Button:** Sorting destination depends on press duration:
    * **Short press (< 1s):** Piece is sorted by **Cylinder 1**.
    * **Long press (> 1s):** Piece is sorted by **Cylinder 2**.

---

## 3. Operational Sequence (Auto Cycle)

1.  **Trigger:** Cycle starts when a workpiece is detected at the end sensor and the Start button is pressed.
2.  **Primary Move:** Conveyor moves the piece to the **Left** using HSC (High-Speed Counter) pulse counts for distance.
3.  **Dwell:** 1-second delay for stability.
4.  **Targeting:** Conveyor moves **Right** and stops precisely at the pre-selected cylinder (Cyl 1 or Cyl 2).
5.  **Dwell:** 1-second delay.
6.  **Sorting:** The designated cylinder kicks the piece off the conveyor.
7.  **Reset:** System resets the counter and sequence, awaiting the next workpiece.

---

## 4. Safety & Interlocks
* **Emergency Stop:** Physical/HMI stop that immediately cuts power to all actuators.
* **Auto/Man Switch:** Turning the physical switch to "Auto" triggers an automatic reset of manual overrides and retracts all cylinders.
* **Pause Function:** Allows the operator to halt the sequence and resume from the exact same step without a full system reset.

---

## 5. Technical I/O Mapping (Highlights)
| Address | Tag Name | Type | Description |
| :--- | :--- | :--- | :--- |
| **Q4.00** | Y1 | Output | Cylinder 1 Solenoid |
| **Q4.01** | Y2 | Output | Cylinder 2 Solenoid |
| **Q4.03** | M1_Right | Output | Motor Direction: Right |
| **Q4.04** | M1_Left | Output | Motor Direction: Left |
| **I4.05** | rCyl1- | Input | Cyl 1 Retracted Sensor |
| **I5.01** | rConvend | Input | Workpiece End Sensor |