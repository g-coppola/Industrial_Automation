# Industrial Automation Project: Plastic Recycling Factory

## Overview
This repository contains an industrial automation project developed for the Academic Year 2024-2025. The objective of the project is to simulate a sorting and processing section of a plastic recycling factory. The system receives raw materials (boxes) via conveyor belts, processes them into finished parts, and automatically sorts the output (Bases and Lids) based on their color (Blue and Green).

## Repository Structure
This repository contains two main files required to run the simulation and the PLC logic:
- `Production_FIO.factoryio`: The 3D factory simulation scene. To be opened with **Factory I/O**.
- `Bases_Lids_Production.project`: The PLC control logic. To be opened with **Codesys**.

## Features
- **Automated Machining:** Simulates the intake and processing of raw materials.
- **Color-Based Sorting:** Uses vision sensors and Pick-and-Place robots to sort products by color (Blue and Green).
- **Production Tracking:** Features digital displays that keep a real-time count of produced Bases and Lids.
- **Control Panel:** Full manual control over the system state (Start, Pause, Reset).

## System Components (Factory I/O)

### Actuators & Mechanical Parts
- **Control Buttons:**
  - **Start:** Initiates the production cycle.
  - **Stop:** Pauses the system.
  - **Reset:** Resets the machinery state and clears the production counters.
- **Status Lights:** - **Green:** System is running (executing sequence).
  - **Yellow:** System is paused.
  - **Red:** System is stopped.
- **Displays:** Show the current count of processed Bases and Lids.
- **Conveyor Belts:** Move materials from the input, through the machining center, and out to the sorting areas.
- **Pusher:** Pushes incoming raw material boxes into the Machining Center.
- **Chain Transfers:** Align and redirect products to the correct lanes.
- **Pick-and-Place Robots:** Transfer Bases and Lids to specific exit conveyors based on their detected color.
- **Emitters & Removers:** Automatically generate raw materials and remove finished products from the simulation.

### Sensors
- **Diffuse Sensors:** Detect the physical presence and passage of objects on the belts.
- **Vision Sensors:** Identify the color of the passing Bases and Lids. The color is encoded using 4 bits to trigger the correct sorting logic.

## Control Logic (Codesys - Ladder Diagram)

The PLC logic is written in **Ladder Diagram (LD)** and is divided into several key functional blocks:

1. **System State & Indicators:** Manages the Start/Stop/Reset logic and toggles the Green, Yellow, and Red lights depending on the current operational state.
2. **Product Counters:** Increments the displays every time a product passes the exit sensor. The WORD signal from the sensor is converted using a `TO_INT` block. Pressing the Reset button clears these variables.
3. **Input Processing (Pusher Logic):** When an incoming box triggers the `NO_BASE_SENS` sensor, the pusher activates to load the material into the machine. The pusher retracts automatically when the `LB_FRONT` sensor detects the action is complete. Timers are implemented to prevent material jams and queues.
4. **Color Sorting:** The Vision Sensor scans the items on the conveyor belt. When a Blue or Green item is detected, the logic interprets the 4-bit code and activates the corresponding Pick-and-Place robot to pick up the item.
5. **Pick-and-Place Reset Sequence:** After a Pick-and-Place robot successfully transfers an object, a 4-second timer starts. Once elapsed, the robot resets to its standard home position, and the conveyor belts (which are temporarily paused during the transfer) resume operation.

## Author
**Giuseppe Coppola** (Mat: 263624)
