# Dual Pump Water Supply Control System

This project demonstrates an automated **dual-pump water management system** managed by a PLC and inverter-driven pumps.  
The system’s purpose is to maintain a stable water level in the secondary storage tank while ensuring reliability, balanced usage, and protection against dry-running conditions.

---

## 1. System Overview

The system controls two pumps (**Pump 1** and **Pump 2**) through frequency inverters using **4–20 mA analog signals**.  
Two operation modes are available — **Manual** and **Automatic** — allowing operators to choose between direct local control or fully automated level management.

![System Overview](https://github.com/hadao169/automation-project/water_supply_system/SCADA.png)

---

## 2. Operating Modes

### 2.1 Manual Mode

- Activated using the **Manual Mode selector or button** on the control panel.  
- Allows operators to **start/stop each pump individually** and **open/close the drain valve** via physical pushbuttons.  
- The active mode is clearly indicated by a **Manual Operation status lamp**.  
- Manual mode is intended for:
  - Maintenance and inspection
  - Functional testing of pumps, valves, and indicators
  - Local operation when the PLC is not controlling the process  

Safety interlocks remain active, preventing operation under unsafe tank conditions.

---

### 2.2 Auto Mode

In **Automatic Mode**, the PLC independently maintains the water level in **Tank 2** within defined limits using feedback from an analog level sensor.

#### Level Control Logic
1. When the tank level falls below the **Low Threshold**, one pump starts automatically.  
2. The active pump runs until the water reaches the **High Threshold**, then stops.  
3. On the next cycle, the **second pump** takes over automatically — this **alternating sequence** ensures balanced mechanical wear between Pump 1 and Pump 2.  

#### Failure Handling
- If a pump fails to start or maintain flow, the system automatically **switches to the standby pump**.  
- If **both pumps fail**, the PLC triggers a **Master Alarm**, halts operation, and prompts operator intervention.  

#### Safety Interlocks
- A **float switch in Tank 1** monitors the inlet supply level.  
- If the water level in Tank 1 drops too low, the PLC initiates a **15‑second delay** before stopping both pumps to prevent **dry running**.
- The system remains in locked standby until the water condition returns to normal.

---

## 3. Control Logic Summary

```plaintext
Tank 1 → Float Switch → Safety Interlock  
Tank 2 → Analog Level Sensor → Level Control

Manual / Auto Selector
     ↓
PLC (Control Logic)
     ↓
Analog Output (4–20 mA)
     ↓
Inverters
     ↓
Pump 1 / Pump 2
```

- Mode selection determines control priority.  
- Automatic mode manages operation based on sensor feedback and internal timers.  
- Manual commands bypass auto sequencing but retain safety protection.

---

## 4. Alarm and Indicator Summary

| Condition                        | Action Taken                           | Visual Indication                |
|----------------------------------|----------------------------------------|----------------------------------|
| Low level in Tank 1 (after 15 s) | Stop both pumps                        | Red alarm lamp on                |
| Pump failure                     | Switch to standby pump                 | Yellow warning lamp              |
| Both pumps failed                | Stop system, trigger master alarm      | Flashing red + buzzer            |
| Auto Mode active                 | System running autonomously            | Green Auto lamp on               |
| Manual Mode active               | Operator local control                 | White Manual lamp on             |

---

## 5. Operation Sequence

1. Select **Manual** or **Auto** mode from panel or HMI.  
2. In **Manual Mode**, use local buttons for individual pump/valve operations.  
3. In **Auto Mode**, system monitors Tank 2 level and operates pumps automatically.  
4. System switches pumps on each cycle for **wear balancing**.  
5. Safety interlocks (Tank 1 float) and fault checks run continuously.  
6. Operator responds to any alarms displayed on the HMI or indicator panel.

---

## 6. System Benefits

- **Reliable water level control** and balanced pump usage.  
- **Full redundancy** for uninterrupted operation during individual pump failure.  
- **Integrated safeguards** prevent dry running and hardware damage.  
- Supports both **local maintenance** and **automatic supervision** modes.

---

## Author

**Ha Dao**  
Automation Engineer — South Ostrobothnia, Finland  
📧 [thaiha.dao@seamk.fi](mailto:thaiha.dao@seamk.fi)  
🔗 [LinkedIn](https://www.linkedin.com/in/ha-dao-a60a13297/)

---

> _“Automation ensures stability; protection ensures longevity.”_