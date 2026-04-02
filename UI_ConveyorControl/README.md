# Conveyor Control System 🚀

A  conveyor control system integrating **C# WinForms** for the User Interface and **Beckhoff TwinCAT 3** for low-level control logic.

---

## 📌 Project Overview
This project is designed to monitor and control industrial conveyor operations. By combining WinForms and TwinCAT, the system leverages the flexible UI capabilities of .NET and the high-performance, real-time stability of PLC logic.

* **UI/UX:** C# WinForms (HMI).
* **Logic Control:** TwinCAT 3 (IEC 61131-3).
* **Communication:** TwinCAT.Ads (Automation Device Specification).

---

## 🛠 System Architecture

The system operates on a Client-Server model using the ADS protocol:
1.  **TwinCAT Runtime:** Manages the control logic for motors, sensors, conveyor.
2.  **C# Application:** Acts as the HMI to send commands (Start/Stop/Reset) and visualize system data (Speed, Faults, Product Count).

---

## ✨ Key Features

### 1. Control Logic (TwinCAT)
-   **Manual/Auto Mode:** Toggle between manual override and fully automated sequences.
-   **Safety Interlocks:** Emergency Stop (E-Stop) handling and motor overload protection logic.
-   **Sensor Logic:** Real-time product detection.

### 2. Monitoring UI (WinForms)
-   **Dashboard:** Visual indicators for Motor status (On/Off, Running/Fault).
-   **Real-time Telemetry:** Live sensor feedback.
-   **Alarm Management:** A dedicated log for tracking system errors and hardware faults.
---

## 💻 Installation & Setup

### Prerequisites:
* Visual Studio 2019/2022.
* TwinCAT 3.1 (XAE or XAR).
* `Beckhoff.TwinCAT.Ads` library (available via NuGet).

### Step-by-Step Setup:
1.  **TwinCAT Side:**
    * Open the project solution in TwinCAT.
    * **Activate Configuration** and ensure the PLC is in **Run Mode**.
2.  **C# Side:**
    * Open the WinForms project.
    * Verify the `AmsNetId` in the source code (default is usually `127.0.0.1.1.1`).
    * Build and run the application.

---

## 📂 Folder Structure

```text
├── TwinCAT Project2/          # TwinCAT 3 Source Code (PLC Logic)
│   ├── POUs/                      # Program Organization Units
│   └── GVLs/                      # Global Variable Lists (ADS Tags)
├── WinFormApp1/               # C# WinForms Source Code
│   ├── Forms/                     # UI Layouts and Design
│   └── Services/                  # ADS Communication Logic
└── README.md