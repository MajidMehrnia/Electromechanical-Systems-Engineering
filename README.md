# Electromechanical Systems Engineering

## Description

This repository supports a New Product Development (NPD) program focused on the design, dynamic modeling, performance analysis, and optimization of coupled electromechanical, rotating, and thermo-fluid components for an electric vehicle thermal-management system. The work integrates **electric drives, motors, pumps, blowers, and thermal-fluid circuits coupled with a scroll compressor** to evaluate **system-level performance, efficiency, and reliability**.



A key objective of this work is to develop an **[AI-Enabled Engineering Value & Optimization Platform](#ai-enabled-engineering-value--optimization-platform)** for the design and optimization of a system across a broad range of electrical and mechanical configurations, enabling data-driven engineering decision-making, **optimized cost positioning**, and engineering-to-supply-chain scenario analysis. The platform explores how engineering data and component demand signals can support cost, demand, capacity, and supply-chain optimization, with potential integration into enterprise planning environments such as Kinaxis Maestro.


## Project Structure

1. [System Architecture](#01-system-architecture)
2. [Motor & Drive](#02-motor--drive)
3. [Pumps](#03-pumps)
4. [Compressor](#04-compressor)
5. [Control & Embedded Firmware Development](#05-control--embedded-firmware-development)  
6. [Electro-Thermal Co-Simulation](#06-electro-thermal-co-simulation)
7. [ECAD / MCAD / DFM Integration](#07-ecad--mcad--dfm-integration)
8. [AI/ML Modeling](#08-aiml-modeling)
9. [NPI Flowchart](#09-npi-flowchart)
10. [Results](#10-results)

   
## 01. System Architecture

The figure below shows the complete electromechanical system architecture co-simulated using Simulink, Simscape, and GT-SUITE. Blue signal and physical lines represent the thermodynamic and fluid networks, including refrigerant loops, coolant channels, the chiller, radiator, and evaporator circuits. Brown lines denote the electrical power distribution and control interconnections between the high-voltage battery, DC-DC converter, charger, PTC heater, and electric motor drive. This integrated Simscape environment enables precise multi-physics dynamic simulation to evaluate transient thermal responses and energy efficiency across demanding vehicle drive cycles.
<br><br>
<img width="1280" height="596" alt="image" src="https://github.com/user-attachments/assets/305941ec-ccae-4513-a4bd-e29ad5ff7c39" />
<br><br>
<img width="1024" height="572" alt="image" src="https://github.com/user-attachments/assets/08c991d2-481e-4477-9fc6-348f638395f1" />
<h3 align="center">Integrated Electromechanical & Thermal Management System</h3>
<br>

<div align="center">

<table>
  <thead>
    <tr>
      <th align="left">Parameter</th>
      <th align="left">Specification</th>
    </tr>
  </thead>
  <tbody>
    <!-- Header 1 -->
    <tr>
      <td colspan="2" align="center"><b>Motor & Drive Specifications</b></td>
    </tr>
    <tr>
      <td><b>E-Motor Max Continuous Power</b></td>
      <td>100 kW</td>
    </tr>
    <tr>
      <td><b>E-Motor Max Continuous Torque</b></td>
      <td>200 N·m</td>
    </tr>
    <tr>
      <td><b>Rotor Inertia</b></td>
      <td>5 × 10<sup>-6</sup> kg·m²</td>
    </tr>
    <tr>
      <td><b>Torque Control Response (T<sub>c</sub>)</b></td>
      <td>2 ms</td>
    </tr>
    <!-- Header 2 -->
    <tr>
      <td colspan="2" align="center"><b>Inverter & Electrical Architecture</b></td>
    </tr>
    <tr>
      <td><b>High Voltage (HV) DC-Link</b></td>
      <td>800 VDC Nominal</td>
    </tr>
    <tr>
      <td><b>Low Voltage (LV) Board Net</b></td>
      <td>24 VDC</td>
    </tr>
    <tr>
      <td><b>Inverter Control Strategy</b></td>
      <td>Field-Oriented Control (FOC) / Vector Control</td>
    </tr>
    <tr>
      <td><b>Power Electronics Topology</b></td>
      <td>3-Phase SiC Inverter</td>
    </tr>
    <!-- Header 3 -->
    <tr>
      <td colspan="2" align="center"><b>Gearbox & Vehicle Load Model</b></td>
    </tr>
    <tr>
      <td><b>Transmission Type</b></td>
      <td>Single-Speed Reduction Gearbox</td>
    </tr>
    <tr>
      <td><b>Gear Ratio (i)</b></td>
      <td>9.5 : 1</td>
    </tr>
    <tr>
      <td><b>Transmission Efficiency (η)</b></td>
      <td>97%</td>
    </tr>
    <tr>
      <td><b>Max Wheel Torque</b></td>
      <td>1900 N·m</td>
    </tr>
    <tr>
      <td><b>Vehicle Dynamics Model</b></td>
      <td>Longitudinal Load Dynamics (Aero Drag + Rolling Resistance)</td>
    </tr>
    <!-- Header 4 -->
    <tr>
      <td colspan="2" align="center"><b>Battery & Drive Thermal Management (BTMS)</b></td>
    </tr>
    <tr>
      <td><b>Refrigerant Type</b></td>
      <td>R1234yf</td>
    </tr>
    <tr>
      <td><b>Operating Temperature Range</b></td>
      <td>-20°C to +55°C</td>
    </tr>
    <tr>
      <td><b>Cooling Capacity (W/O PTC)</b></td>
      <td>8.5 kW</td>
    </tr>
    <tr>
      <td><b>Heating Capacity (W/O PTC)</b></td>
      <td>9 kW</td>
    </tr>
    <tr>
      <td><b>Auxiliary Heating Capacity (W PTC)</b></td>
      <td>10 - 20 kW</td>
    </tr>
    <tr>
      <td><b>Coolant Flow Rate</b></td>
      <td>≥ 1100 l/h</td>
    </tr>
  </tbody>
</table>

</div>

## 02. Motor & Drive

The electric motor is driven by the power drive electronics and is mechanically connected to the actuator drive system. The simulation framework can be easily extended to describe alternative motion control architectures (e.g., direct-drive or multi-axis linear setups). The motor’s dynamic characteristics and electrical losses are modeled using efficiency maps, with transient temperatures determined by internal losses and thermal mass. To deliver the required torque and force, the motor is coupled with a precision gearbox operating at a constant transmission ratio, with mechanical losses modeled using a constant efficiency.
<br><br>
<img width="1915" height="1009" alt="9-1" src="https://github.com/user-attachments/assets/d6148666-3abd-47bb-82a7-26651afe2487" />
<br><br>
This Simscape Electrical motor and drive block is parameterized at the system level with an enabled thermal port to model physical heat transfer into the cooling loop. The drive unit is configured for a continuous peak torque of 200 N·m and a continuous maximum power of 100 kW with a fast dynamic response defined by a torque control time constant $T_c = 0.002\text{ s}$. System efficiency and electrical losses are captured via 2D lookup tables $P(\omega, T)$ mapped across the full speed and torque operational range. This multi-physics formulation accurately calculates real-time power dissipation losses to evaluate transient electro-thermal performance under demanding drive cycles.
<br><br>
<img width="813" height="1029" alt="image" src="https://github.com/user-attachments/assets/0c8a0152-dcca-4e3a-b0a0-5a12a7340e6b" />
<br><br>
## 03. Pumps

In this system architecture, there are two pumps (P1 and P2) operating in separate coolant circuits. P1 provides thermal conditioning for the battery pack, DCDC converter, and onboard charger, whereas P2 regulates the temperature of the electric motor and interfaces directly with the chiller loop.

<img width="589" height="216" alt="image" src="https://github.com/user-attachments/assets/fb3b6662-22fe-4b3e-a7fa-6afa6d319eac" />


| Pump | Primary Thermal Loop | Main Function |
| :--- | :--- | :--- |
| **P1** | Battery, DCDC & Charger | Precise temperature control for high-voltage battery safety and battery lifespan |
| **P2** | Electric Motor & Chiller | Heat dissipation for the electric powertrain and refrigerant-to-coolant heat exchange |

## 04. Compressor

### Compressor Subsystem

The compressor drives the flow in the refrigerant loop. Instead of using a map-based compressor model commonly found in system simulations, a higher-fidelity, geometry-based 3D-to-1D discretized compressor model developed in GT-SUITE is integrated to achieve significantly higher accuracy and physical reliability.

### 3D-to-1D Discretized Scroll Compressor Model
This model implements a detailed 3D-to-1D discretized multi-chamber approach directly derived from 3D CAD scroll geometry rather than relying on empirical performance maps. The physical compression volume between the stationary and orbiting scrolls is discretized into discrete transient pockets (Chambers 1a–4a and 1b–4b) whose volume and porting areas dynamically evolve as a function of the orbital angle. By explicitly resolving flank and radial leakage paths between adjacent chambers, the model accurately predicts internal recirculation losses, thermal interactions, and discharge valve dynamics with high fidelity while maintaining 1D computational efficiency.
<img width="856" height="500" alt="GT_Scroll" src="https://github.com/user-attachments/assets/f7d13a90-0bb6-4a4a-8dc9-9b23ac900a69" />
<br><br>
<img width="1280" height="599" alt="image" src="https://github.com/user-attachments/assets/39f0f387-08ca-4539-a389-9196e504ae7d" />


## 05. Control & Embedded Firmware Development
The control architecture is designed to enable precise motor control, dynamic load tracking, and integrated electro-thermal system management.

* **Electric Motor & Motion Control:** Executes speed and torque command generation ($T_{cmd}$) based on driver demand ($VehSpdRef$), enabling dynamic load regulation, precise motion tracking, and transient torque control for the electric drive unit.

The  control unit coordinates the battery, power electronics, electric motor, charging & battery management system (BMS) and thermal management functions through real-time supervisory control. This enables coordinated vehicle operation, operating-state management, torque and regenerative-braking control, and monitoring of key electrical and thermal constraints.

<br><br>
<img width="1918" height="795" alt="9-5" src="https://github.com/user-attachments/assets/9f4ab89b-5ed5-444d-8c9b-95f26ebda8a4" />
<br><br>

This project  includes a conceptual **Embedded C firmware architecture** for an electric vehicle **Vehicle Control Unit (VCU)**. The firmware layer connects the model-based vehicle system architecture with real-time embedded control, vehicle-state management and CAN-based communication with major vehicle subsystems.

The implementation is structured around an STM32-based VCU with **C, FreeRTOS and CAN communication**, following a modular architecture suitable for prototyping and Hardware-in-the-Loop (HIL) development.

### Embedded Firmware Scope

| Area | Implementation | Engineering Purpose |
|---|---|---|
| **Embedded C** | Modular C application layer | Real-time vehicle control and system coordination |
| **STM32 MCU** | STM32-based VCU architecture | Embedded execution platform |
| **FreeRTOS** | Task-based real-time scheduling | Deterministic execution of control and communication functions |
| **CAN Communication** | CAN message handling and interfaces | Communication with BMS, inverter/motor controller and charger |
| **Vehicle Control Logic** | State machine and supervisory control | Vehicle operating-state management |
| **Fault Management** | Fault detection and safe-state handling | System protection and diagnostic response |
| **BMS Interface** | CAN-based BMS status/command interface | Battery monitoring and control coordination |
| **Motor Controller Interface** | CAN-based motor/inverter interface | Drive, torque and regenerative-braking coordination |
| **Charger Interface** | CAN-based charger interface | Charging-state and charging-command management |
| **Diagnostics** | UART debug and status monitoring | Development-time observability and troubleshooting |

### Firmware Architecture

| Module | Primary Responsibility |
|---|---|
| `main.c` | MCU initialization, peripheral setup and RTOS startup |
| `vehicle_control.c` | Vehicle state machine, supervisory control and fault logic |
| `can_bus.c` | CAN initialization, message reception, transmission and buffering |
| `bms_can.c` | BMS CAN message encoding, decoding and status handling |
| `motor_can.c` | Motor-controller CAN message encoding, decoding and command handling |
| `charger_can.c` | Charger CAN message encoding, decoding and command handling |
| `fault_manager.c` | Fault detection, protection thresholds and safe-state handling |
| `state_machine.c` | Vehicle operating-state transitions |
| `thermal_manager.c` | Thermal-limit monitoring and supervisory thermal control |
| `diagnostics.c` | Runtime diagnostics and development logging |
| `ev_config.h` | CAN identifiers, safety thresholds and timing parameters |
| `ev_types.h` | Shared data structures, states and command definitions |

### Real-Time Task Structure

| Task | Priority | Typical Period | Function |
|---|---:|---:|---|
| `ControlTask` | High | 10 ms | Vehicle-state evaluation, supervisory control and command generation |
| `CanRxTask` | High | Event-driven | CAN message reception, decoding and status updates |
| `ThermalTask` | Medium | 50–100 ms | Thermal monitoring and protection logic |
| `DiagnosticsTask` | Low | 500 ms | Diagnostic logging and system-status reporting |

The reference VCU architecture uses separate control, CAN reception and diagnostic tasks under FreeRTOS, with the main control loop operating at a 10 ms period.

### CAN Interface Matrix

| Node | Direction | Example Data |
|---|---|---|
| **BMS** | BMS → VCU | Battery voltage, current, temperature, SOC and status |
| **BMS** | VCU → BMS | Contactor, charge and discharge requests |
| **Motor Controller / Inverter** | Motor → VCU | Speed, current, temperature, operating state |
| **Motor Controller / Inverter** | VCU → Motor | Enable, torque, direction and regenerative-braking commands |
| **Charger** | Charger → VCU | Charging voltage, current and charger state |
| **Charger** | VCU → Charger | Charge enable and target voltage/current |

The reference implementation defines separate CAN interfaces for BMS, motor controller and charger communication and uses dedicated status and command structures for each subsystem.

### Vehicle State Machine

```text
IDLE
  |
  v
READY
  |
  +-----------> DRIVE
  |
  +-----------> CHARGING
  |
  +-----------> FAULT
````

Typical state transitions are governed by:

* BMS availability and battery conditions
* High-voltage system readiness
* Motor-controller status
* Accelerator and brake inputs
* Charger connection and charging conditions
* Thermal limits
* CAN communication timeout
* Over-voltage / under-voltage conditions
* Over-temperature conditions
* System fault status

### Embedded C Firmware Implementation

The following implementation defines a simplified VCU supervisory-control function for vehicle-level control, status monitoring, and command management:

```c
#include "vehicle_control.h"
#include "bms_can.h"
#include "motor_can.h"
#include "fault_manager.h"

void VehicleControl_Step(void)
{
    BMS_Status_t bms;
    Motor_Status_t motor;
    Motor_Command_t command;

    BMS_GetStatus(&bms);
    Motor_GetStatus(&motor);

    if (FaultManager_IsActive())
    {
        command.enable = false;
        command.torque_request = 0.0f;
        command.regen_request = 0.0f;

        Motor_SendCommand(&command);
        return;
    }

    if (bms.soc > SOC_MIN &&
        bms.temperature < BATTERY_TEMP_MAX &&
        motor.ready)
    {
        command.enable = true;
        command.torque_request = Vehicle_GetTorqueRequest();
        command.regen_request = Vehicle_GetRegenRequest();
    }
    else
    {
        command.enable = false;
        command.torque_request = 0.0f;
        command.regen_request = 0.0f;
    }

    Motor_SendCommand(&command);
}
```

### CAN Communication & Message Handling

```c
void CAN_ProcessMessage(const CAN_Message_t *msg)
{
    switch (msg->id)
    {
        case CAN_ID_BMS_STATUS:
            BMS_DecodeStatus(msg);
            break;

        case CAN_ID_MOTOR_STATUS:
            Motor_DecodeStatus(msg);
            break;

        case CAN_ID_CHARGER_STATUS:
            Charger_DecodeStatus(msg);
            break;

        default:
            Diagnostics_ReportUnknownCANMessage(msg->id);
            break;
    }
}
```

### Fault Handling

The VCU monitors critical system conditions and transitions the vehicle to a safe state when predefined limits or communication conditions are violated.

| Fault Condition           | Detection                | VCU Response                      |
| ------------------------- | ------------------------ | --------------------------------- |
| Battery over-temperature  | BMS temperature feedback | Disable drive request             |
| Battery under-voltage     | BMS voltage feedback     | Disable drive request             |
| Motor over-temperature    | Motor CAN status         | Torque limitation / drive disable |
| CAN communication timeout | Message watchdog         | Enter safe state                  |
| BMS fault                 | BMS status flag          | Disable HV-related commands       |
| Inverter fault            | Motor-controller status  | Disable torque request            |
| Charging fault            | Charger status           | Stop charging request             |

### Development and Verification

The firmware architecture is intended to support incremental verification from software simulation through embedded execution.

| Verification Level | Method                                | Purpose                                                      |
| ------------------ | ------------------------------------- | ------------------------------------------------------------ |
| **Model-Level**    | MATLAB / Simulink                     | Verify control logic and system behavior                     |
| **Software-Level** | C unit testing                        | Verify individual firmware modules                           |
| **SIL**            | Software-in-the-Loop                  | Compare embedded control logic with system models            |
| **HIL**            | Hardware-in-the-Loop                  | Validate VCU behavior with simulated vehicle subsystems      |
| **Embedded Test**  | STM32 + CAN network                   | Verify real-time execution and communication                 |
| **System-Level**   | EV system model + embedded controller | Validate interaction between controls and vehicle subsystems |

### Engineering Integration

The embedded firmware layer complements the model-based EV system study by providing a pathway from system-level modeling to real-time implementation.

| Engineering Layer         | Technology                                  | Role                                          |
| ------------------------- | ------------------------------------------- | --------------------------------------------- |
| **Vehicle System Model**  | MATLAB / Simulink / Simscape / GT-SUITE     | System-level analysis and control development |
| **Control Algorithm**     | Simulink / Embedded C                       | Control logic development                     |
| **Embedded Firmware**     | C / STM32 / FreeRTOS                        | Real-time implementation                      |
| **Vehicle Communication** | CAN                                         | ECU-to-ECU communication                      |
| **Validation**            | SIL / HIL / Embedded Testing                | Verification of control behavior              |
| **Physical System**       | Motor / Inverter / Battery / Thermal System | Target electromechanical system               |

### Key Engineering Capabilities

* Embedded C development
* STM32-based ECU architecture
* Real-time control with FreeRTOS
* CAN communication and message handling
* VCU supervisory control
* BMS, inverter and charger interfaces
* Vehicle state-machine design
* Fault detection and safe-state management
* Thermal and electrical constraint monitoring
* SIL / HIL-oriented verification
* Model-based control integration
* Embedded-to-system engineering traceability
<br><br>

## 06. Electro-Thermal Co-Simulation
This GT-SUITE sub-model captures the detailed 1D thermal-fluid dynamics of the refrigerant compressor loop co-simulated directly with Simulink. The circuit models two-phase refrigerant flow through inlet and outlet piping (`PipeRound`) connected between environmental boundary conditions and the compressor unit. Rotational speed commands and boundary states are dynamically exchanged with the Simulink control model via dedicated co-simulation interface ports. A specialized initialization block (`RefrigCircInit`) establishes state convergence for the refrigerant loop to ensure stable transient simulation during vehicle operational cycles.
<br><br>
<img width="1280" height="542" alt="GT-SUITE_blocks" src="https://github.com/user-attachments/assets/6f010da7-290a-4381-9ed3-0ad721b30cfa" />
<br><br>
## 07. ECAD / MCAD / DFM Integration
Multi-domain digital thread closes the loop between E/E design, 3D mechanical packaging, and manufacturing validation before physical prototyping. By linking domain-specific models directly into the system-level simulation environment, engineering team can continuously verify electrical and thermal performance under realistic operating conditions. As a result, critical design issues are caught early in the development cycle, sign-off processes are accelerated, and the entire product baseline is seamlessly released to PLM with full traceability.
<br>
<p align="center">
 <b>An integrated ECAD-MCAD flow creates a digital thread through the design</b>
<img width="1226" height="364" alt="image" src="https://github.com/user-attachments/assets/567c381d-a811-4ea5-a1e1-e61f3a3767bc" />
</p>
<br>

An integrated ECAD-MCAD digital thread connects the electrical architecture and wiring-harness definition with the model-based system engineering environment. **Xpedition (ECAD)** defines the electrical and PCB design, **Capital** (E/E Systems Engineering) defines the electrical architecture, wiring and connectivity, **NX (MCAD)** defines the mechanical system and 3D integration, and **Valor (DFM)** validates manufacturability and supports design-for-manufacturing decisions. These engineering tools are connected to the MATLAB/Simulink/Simscape/GT-SUITE environment for system-level EV analysis, linking electrical, mechanical, manufacturing, and system models from design → system analysis → integration → verification → manufacturing → PLM release.
<br><br>
<p align="center">
 <b>XML helped connect the traditionally separated ECAD and MCAD
domains</b>
<img width="864" height="1488" alt="image" src="https://github.com/user-attachments/assets/0dbe6e13-daf7-4037-bf8d-1dd9fcb86188" />
</p>
<br><br>
This repository presents a model-based engineering study of an integrated electric vehicle system using MATLAB, Simulink, Simscape and GT-SUITE. The simulation architecture is used to investigate the interaction between electric motors, power electronics, mechanical driveline, control systems, thermal systems and vehicle-level operating scenarios. The engineering focus is on the development and analysis of Electromechanical systems and the interfaces between:

**Electrical Power → Drive & Control → Motor → Mechanical Transmission → System Load**

The original vehicle-level architecture provides the foundation for investigating the interaction between electrical, mechanical, control and thermal domains. The focus of this repository is the engineering optimization of electromechanical motion systems and their interfaces, rather than the development of a complete vehicle model. The entire product lifecycle in this project is governed by the PLM framework, demonstrating how a robust "Single Source of Truth" can be established. This system integrates technical requirements with engineering execution by establishing strict document revision controls, transitioning engineering bills of materials (EBOM) to manufacturing skids (MBOM), and enforcing disciplined change log workflows (ECR/ECO/ECN). Ultimately, every technical optimization, such as power consumption reductions or material reusability, is directly linked to target costing and ROI models, proving that robust engineering governance is a direct driver of corporate profitability. 
<br><br>
## 08. AI/ML Modeling

This section outlines the AI/ML surrogate modeling framework developed using **Artificial Neural Networks (ANN)** to predict total system electromechanical performance. Trained on multi-physics datasets spanning varied electric motor specifications, gear ratios, and driveline parameters, the model rapidly estimates system-level thermal behavior. This data-driven approach replaces computationally expensive finite-element and lump-parameter dynamic simulations with high-speed predictive modeling. The ANN framework enables real-time optimization and rapid design-space exploration across diverse powertrain configurations and operating profiles.
<br><br>
The proposed software enables a new generation of AI-assisted system design by integrating electro-mechanical based simulations with Artificial Neural Network (ANN) optimization. It significantly reduces development time while maintaining engineering credibility, making it well-suited for OEM-level decision support in electromechanical system development.
<br><br>
Here we can just publish, a novel ML-based thermodynamic modeling approach has been developed and validated against simulations. Related AI/ML developments are available in my companion repository:
[Physics-Informed AI/ML Modeling](https://github.com/MajidMehrnia/Physics-Informed-AI-ML-for-Thermodynamic-Modeling)

Detailed technical information on this AI/ML framework is provided in the following sub-sections.


# AI-Enabled Engineering Value & Optimization Platform

**Physics-Based Simulation, ANN Surrogate Modeling & Engineering Decision Support**

---

## Table of Contents

- [7.1 Overview](#71-overview)
- [7.2 Objective of the Software](#72-objective-of-the-software)
- [7.3 Software Workflow](#73-software-workflow)
- [7.4 Overall Software Architecture](#74-overall-software-architecture)
- [7.5 OEM and Engineering Input Parameters](#75-oem-and-engineering-input-parameters)
  - [Vehicle and System Requirements](#vehicle-and-system-requirements)
  - [Electric Motor Specifications](#electric-motor-specifications)
  - [Power Electronics and Drive Parameters](#power-electronics-and-drive-parameters)
  - [Thermal Management Parameters](#thermal-management-parameters)
- [7.6 Physics-Based Simulation Core](#76-physics-based-simulation-core)
- [7.7 ANN-Based AI Engine](#77-ann-based-ai-engine)
- [7.8 AI Output Evaluation and Engineering Validation](#78-ai-output-evaluation-and-engineering-validation)
- [7.9 Optimization and AI-Enabled Engineering Value](#79-optimization-and-ai-enabled-engineering-value)
  - [Optimization Functions](#optimization-functions)
- [7.10 Development Roadmap](#710-development-roadmap)


---


## 7.1 Overview

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Category</font></th>
<th><font color="white">Description</font></th>
</tr>
<tr>
<td><b>Title</b></td>
<td>AI-Integrated System Design & Optimization Platform</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Application</b></td>
<td>Electric Motors & Mechatronic Systems with Thermal Management</td>
</tr>
<tr>
<td><b>Target Users</b></td>
<td>OEMs, Tier-1 Suppliers, R&amp;D and Engineering Teams</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Development Stage</b></td>
<td>Concept / Pre-Development</td>
</tr>
<tr>
<td><b>Core Innovation</b></td>
<td>Physics-based electro-thermal simulation integrated with Artificial Neural Network (ANN) surrogate modeling</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Primary Goal</b></td>
<td>Accelerate system concept development and engineering decision-making for electric mobility applications</td>
</tr>
<tr>
<td><b>Key Function</b></td>
<td>Translate engineering requirements into optimized system architectures and design parameters</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Scope</b></td>
<td>Concept design, architecture selection, parameter sizing, performance prediction and optimization</td>
</tr>
<tr>
<td><b>Engineering Value</b></td>
<td>Higher engineering efficiency, stronger decision intelligence, improved engineering assurance, optimized cost position and development ROI</td>
</tr>
</table>

---

## 7.2 Objective of the Software

The objective is to integrate **physics-based engineering simulation, AI surrogate modeling and optimization** into a decision-support platform for early-stage system development.

The software is designed to:

* Accelerate early-stage electromechanical and electro-thermal system architecture development.
* Bridge high-fidelity physics-based simulations with fast AI-driven surrogate models.
* Reduce repetitive computational effort during design-space exploration.
* Enable automated optimization of system architectures and engineering parameters.
* Provide quantitative engineering trade-off analysis to support system-level decisions.
* Increase engineering throughput by reducing repetitive simulation and evaluation activities.
* Strengthen engineering assurance through structured validation and physics-based verification.
* Improve cost position by reducing unnecessary computational, iteration and prototype effort.
* Apply AI selectively to engineering use cases where it provides measurable technical or economic value.
* Critically evaluate AI-generated outputs before they are used to support engineering decisions.

---

## 7.3 Software Workflow

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Step</font></th>
<th><font color="white">Engineering Activity</font></th>
</tr>
<tr>
<td><b>1</b></td>
<td>OEM and engineering requirements are entered through a graphical interface.</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>2</b></td>
<td>Initial system architecture and engineering parameter ranges are defined.</td>
</tr>
<tr>
<td><b>3</b></td>
<td>Physics-based simulations generate reference data across the design space.</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>4</b></td>
<td>ANN surrogate models learn the validated relationship between engineering inputs and system performance.</td>
</tr>
<tr>
<td><b>5</b></td>
<td>Optimization algorithms explore and refine candidate architectures and parameter combinations.</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>6</b></td>
<td>AI-generated predictions are evaluated against physics-based reference data and engineering constraints.</td>
</tr>
<tr>
<td><b>7</b></td>
<td>Selected concepts are verified using high-fidelity Simulink / GT-SUITE models.</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>8</b></td>
<td>Performance, energy, thermal and cost-related trade-offs are summarized for engineering decision-making.</td>
</tr>
</table>

---

## 7.4 Overall Software Architecture

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Layer</font></th>
<th><font color="white">Function</font></th>
<th><font color="white">Technology</font></th>
</tr>
<tr>
<td><b>Engineering Input Interface</b></td>
<td>Capture vehicle, motor, electrical and thermal requirements</td>
<td>MATLAB App Designer</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Physics-Based Simulation</b></td>
<td>High-fidelity electro-thermal and thermal-fluid system modeling</td>
<td>Simulink / GT-SUITE / CFD</td>
</tr>
<tr>
<td><b>AI / ANN Engine</b></td>
<td>Fast surrogate modeling of system performance</td>
<td>ANN / Deep Learning</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Optimization Layer</b></td>
<td>Automated design-space exploration and parameter optimization</td>
<td>Genetic Algorithm / Bayesian Optimization</td>
</tr>
<tr>
<td><b>Validation Layer</b></td>
<td>Evaluate AI predictions against physics-based results</td>
<td>Simulation / Statistical Validation</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Decision Support</b></td>
<td>Engineering trade-off analysis and decision-ready outputs</td>
<td>MATLAB / Plots / Tables / Reports</td>
</tr>
</table>

---

## 7.5 OEM and Engineering Input Parameters

### Vehicle and System Requirements

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Parameter</font></th>
<th><font color="white">Description</font></th>
</tr>
<tr>
<td><b>Vehicle Data</b></td>
<td>Vehicle segment, BEV architecture and drivetrain configuration</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Electrical Architecture</b></td>
<td>DC-link voltage, battery voltage and electrical power limits</td>
</tr>
<tr>
<td><b>Environmental Conditions</b></td>
<td>Ambient temperature range, humidity and altitude</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Thermal Requirements</b></td>
<td>Cabin heating and cooling demand, battery heating and cooling requirements</td>
</tr>
<tr>
<td><b>Operating Scenarios</b></td>
<td>Drive cycles, load profiles and transient operating conditions</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>System Constraints</b></td>
<td>Packaging limits, mass constraints and energy consumption targets</td>
</tr>
<tr>
<td><b>Cost Constraints</b></td>
<td>Component cost sensitivity and system cost targets</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Regulatory</b></td>
<td>Refrigerant type, safety and compliance constraints</td>
</tr>
</table>

### Electric Motor Specifications

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Parameter</font></th>
<th><font color="white">Description</font></th>
</tr>
<tr>
<td><b>Motor Type</b></td>
<td>PMSM / IPMSM / BLDC / Induction Motor</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Rated Power</b></td>
<td>Continuous rated power [kW]</td>
</tr>
<tr>
<td><b>Peak Power</b></td>
<td>Maximum short-duration power [kW]</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Rated Voltage</b></td>
<td>Nominal motor voltage [V]</td>
</tr>
<tr>
<td><b>DC-Link Voltage</b></td>
<td>Nominal and operating DC bus voltage [V]</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Rated Current</b></td>
<td>Continuous RMS current [A]</td>
</tr>
<tr>
<td><b>Peak Current</b></td>
<td>Maximum current capability [A]</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Rated Speed</b></td>
<td>Nominal operating speed [rpm]</td>
</tr>
<tr>
<td><b>Maximum Speed</b></td>
<td>Maximum mechanical speed [rpm]</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Rated Torque</b></td>
<td>Continuous torque [Nm]</td>
</tr>
<tr>
<td><b>Peak Torque</b></td>
<td>Maximum short-duration torque [Nm]</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Pole Pairs</b></td>
<td>Number of electromagnetic pole pairs</td>
</tr>
<tr>
<td><b>Winding Resistance</b></td>
<td>Phase resistance [Ω]</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Inductance</b></td>
<td>d-axis / q-axis inductance [H]</td>
</tr>
<tr>
<td><b>Back-EMF</b></td>
<td>Back-electromotive-force characteristics</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Efficiency</b></td>
<td>Motor efficiency map / operating-point efficiency [%]</td>
</tr>
<tr>
<td><b>Torque-Speed Characteristics</b></td>
<td>Torque, speed and power operating envelope</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Loss Characteristics</b></td>
<td>Copper, iron, mechanical and stray losses</td>
</tr>
<tr>
<td><b>Thermal Characteristics</b></td>
<td>Winding, stator, rotor and housing thermal parameters</td>
</tr>
</table>

### Power Electronics and Drive Parameters

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Parameter</font></th>
<th><font color="white">Description</font></th>
</tr>
<tr>
<td><b>Inverter Topology</b></td>
<td>Three-phase inverter architecture</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Switching Devices</b></td>
<td>Si / SiC semiconductor characteristics</td>
</tr>
<tr>
<td><b>Switching Frequency</b></td>
<td>Inverter switching frequency [kHz]</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>DC Bus</b></td>
<td>Voltage range and current limits</td>
</tr>
<tr>
<td><b>Drive Strategy</b></td>
<td>FOC / vector control / torque control</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Current Limits</b></td>
<td>Continuous and peak current constraints</td>
</tr>
<tr>
<td><b>Inverter Losses</b></td>
<td>Conduction and switching losses</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Control Constraints</b></td>
<td>Voltage, current, speed and thermal operating limits</td>
</tr>
</table>

### Thermal Management Parameters

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Parameter</font></th>
<th><font color="white">Description</font></th>
</tr>
<tr>
<td><b>Coolant</b></td>
<td>Coolant type and thermophysical properties</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Coolant Flow Rate</b></td>
<td>Mass / volumetric flow rate</td>
</tr>
<tr>
<td><b>Coolant Temperature</b></td>
<td>Inlet temperature and operating range</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Heat Exchangers</b></td>
<td>Capacity, UA characteristics and pressure drop</td>
</tr>
<tr>
<td><b>Cooling Architecture</b></td>
<td>Motor, inverter, battery and cabin thermal interfaces</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Thermal Constraints</b></td>
<td>Maximum allowable component temperatures</td>
</tr>
<tr>
<td><b>Ambient Conditions</b></td>
<td>Temperature, altitude and humidity</td>
</tr>
</table>

---

## 7.6 Physics-Based Simulation Core

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Category</font></th>
<th><font color="white">Description</font></th>
</tr>
<tr>
<td><b>Modeling Approach</b></td>
<td>1D thermodynamic, thermal-fluid and electro-thermal modeling</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Main Subsystems</b></td>
<td>Electric motor, inverter, refrigerant loop, coolant loops, cabin and battery interfaces</td>
</tr>
<tr>
<td><b>Model Environment</b></td>
<td>Simulink / GT-SUITE / CFD</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Role in Software</b></td>
<td>Reference-data generation and final engineering validation</td>
</tr>
<tr>
<td><b>Execution Mode</b></td>
<td>Offline batch simulation and on-demand validation</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Engineering Role</b></td>
<td>Physics-based simulation provides the reference for evaluating AI predictions</td>
</tr>
</table>

---

## 7.7 ANN-Based AI Engine

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Category</font></th>
<th><font color="white">Description</font></th>
</tr>
<tr>
<td><b>ANN Purpose</b></td>
<td>Reduce repeated high-fidelity simulation effort during design-space exploration and optimization</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>ANN Inputs</b></td>
<td>Motor characteristics, inverter parameters, ambient conditions, flow rates, thermal parameters, architecture type and operating conditions</td>
</tr>
<tr>
<td><b>ANN Outputs</b></td>
<td>Motor efficiency, thermal losses, component temperatures, COP, heating/cooling capacity and power consumption</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Primary Benefit</b></td>
<td>Rapid performance prediction across the validated design space</td>
</tr>
<tr>
<td><b>Engineering Application</b></td>
<td>Fast architecture comparison, parameter sensitivity analysis and optimization</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Validation Basis</b></td>
<td>AI predictions evaluated against physics-based simulation results</td>
</tr>
<tr>
<td><b>Out-of-Domain Handling</b></td>
<td>Uncertain or out-of-domain cases are referred back to high-fidelity simulation</td>
</tr>
</table>

---

## 7.8 AI Output Evaluation and Engineering Validation

A central principle of the software is that **AI-generated outputs are treated as engineering decision-support information and must be critically evaluated before use.**

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Validation Dimension</font></th>
<th><font color="white">Engineering Check</font></th>
</tr>
<tr>
<td><b>Prediction Accuracy</b></td>
<td>Compare ANN predictions against physics-based simulation results</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Physical Plausibility</b></td>
<td>Verify consistency with expected engineering and thermodynamic behavior</td>
</tr>
<tr>
<td><b>Design-Space Validity</b></td>
<td>Confirm inputs remain within the validated AI model domain</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Constraint Compliance</b></td>
<td>Check electrical, thermal, mechanical, packaging and operating constraints</td>
</tr>
<tr>
<td><b>Robustness</b></td>
<td>Evaluate model behavior under parameter and operating-condition variations</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Exception Handling</b></td>
<td>Escalate uncertain or out-of-domain cases to high-fidelity simulation</td>
</tr>
<tr>
<td><b>Engineering Review</b></td>
<td>Final interpretation and design decisions remain subject to engineering review</td>
</tr>
</table>

---

## 7.9 Optimization and AI-Enabled Engineering Value

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Engineering Value</font></th>
<th><font color="white">AI Contribution</font></th>
</tr>
<tr>
<td><b>Decision Intelligence</b></td>
<td>Enable rapid quantitative comparison of system architectures, design alternatives and engineering trade-offs</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Engineering Efficiency</b></td>
<td>Reduce repetitive simulation, parameter sweeps and manual design-space evaluation</td>
</tr>
<tr>
<td><b>Engineering Assurance</b></td>
<td>Improve consistency, traceability and validation of engineering evaluations through physics-based verification</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Value Engineering &amp; Cost Position</b></td>
<td>Apply VAVE principles to optimize system architecture, engineering effort and prototype strategy while improving development cost position and potential ROI</td>
</tr>
<tr>
<td><b>AI Use-Case Fit</b></td>
<td>Identify engineering applications where surrogate modeling provides measurable value over repeated high-fidelity simulation</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>AI Output Assurance</b></td>
<td>Critically evaluate AI predictions against physics-based models, engineering constraints, validation data and the defined model domain</td>
</tr>
</table>

### Optimization Functions

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Engineering Function</font></th>
<th><font color="white">Application</font></th>
</tr>
<tr>
<td><b>Architecture Exploration</b></td>
<td>Compare alternative system architectures across multiple operating scenarios</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Parameter Optimization</b></td>
<td>Identify suitable motor, inverter and thermal-system parameters</td>
</tr>
<tr>
<td><b>Performance Analysis</b></td>
<td>Evaluate efficiency, thermal performance, power consumption and system capacity</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Sensitivity Analysis</b></td>
<td>Identify parameters with the greatest influence on system performance</td>
</tr>
<tr>
<td><b>Trade-Off Analysis</b></td>
<td>Evaluate performance, energy, thermal and cost-related trade-offs</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Design Screening</b></td>
<td>Rapidly identify candidate concepts for detailed engineering evaluation</td>
</tr>
<tr>
<td><b>High-Fidelity Verification</b></td>
<td>Re-evaluate selected candidates using physics-based simulation</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Decision Output</b></td>
<td>Generate engineering-ready plots, tables and quantitative comparison reports</td>
</tr>
</table>

---

## 7.10 Development Roadmap

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Phase</font></th>
<th><font color="white">Duration</font></th>
<th><font color="white">Primary Deliverable</font></th>
</tr>
<tr>
<td><b>Phase 1 — Physics Model</b></td>
<td>6 months</td>
<td>Parametric Simulink / GT-SUITE model and reference simulation dataset</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Phase 2 — AI Development</b></td>
<td>3 months</td>
<td>ANN training, validation and robustness assessment</td>
</tr>
<tr>
<td><b>Phase 3 — Optimization Integration</b></td>
<td>3 months</td>
<td>AI-assisted optimization and design-space exploration</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Phase 4 — Decision Support</b></td>
<td>2 months</td>
<td>Engineering GUI, reporting and OEM demonstration</td>
</tr>
</table>
<br><br>

## 09. NPI Flowchart

<p align="center">
 <img width="1267" height="703" alt="image" src="https://github.com/user-attachments/assets/735892a3-fd9d-4ce8-bfbb-678039c29123" />
</p>


## 10. Results

The complete executable models and the underlying management tool is available below:

* Access the simulation files in the [Simulation](input_data) and [Results](results) directories.

This plot shows the power consumed by the thermal management system to cool the vehicle components and cabin. The largest power consumption occurs in the refrigerant compressor when the chiller bypass valve directs coolant to the chiller to cool the batteries.
<br><br>
<img width="908" height="546" alt="image" src="https://github.com/user-attachments/assets/936f5b02-17b3-496f-bf7b-aedb204c37af" />
<br><br>
This Simscape Data Inspector plot illustrates the transient current waveform ($i_1$) for the DC-DC converter block over a long-term simulation profile of $2.5 \times 10^4\text{ s}$. The time-series response captures steady-state current draw baseline around $0.8\text{ A}$ to $1.0\text{ A}$ interrupted by periodic high-amplitude current spikes reaching up to $2.5\text{ A}$. These dynamic current transients represent cyclic peak power demands from auxiliary low-voltage loads and dynamic charging events within the power distribution network. Analyzing these current profiles is critical for evaluating component electrical stress, conductor sizing, and thermal dissipation management under representative driving scenarios.
<br><br>
<img width="1918" height="793" alt="9-6" src="https://github.com/user-attachments/assets/8eaf0f07-25ab-4d4c-a1bc-33c3a1c1ec37" />
<br><br>
This Simscape scope plot highlights the thermal transient responses of the powertrain components, plotting temperature (°C) against time (s) across driving cycles. The electric motor operates between 40°C and 50°C, reflecting rapid heating and cooling phases during high-load traction and regenerative cycles. The battery temperature maintains a stabilized thermal envelope between 29°C and 36°C, demonstrating effective multi-loop active cooling. The DC-DC converter exhibits sharper dynamic fluctuations from 13°C up to 34°C, directly driven by low-voltage auxiliary power demands and intermittent cycling.
<br><br>
<strong style="color:red;">Simulation Results from Simscape Logging</strong>


<img width="975" height="492" alt="T_vs_t_Motor_Battery" src="https://github.com/user-attachments/assets/f6526b99-67fe-4be3-97a3-2bf96466c894" />
<br><br>
<img width="1088" height="534" alt="HF-1" src="https://github.com/user-attachments/assets/87254f23-f38a-4114-9edc-318406de2367" />
<br><br>
The main thermodynamic output of the calculations is the **P–h diagram** of the heat pump cycle, as shown in the figure below. The calculated COP (based on refrigerant enthalpy difference (cycle COP)) is 5.2 at a **condensing temperature of 40 °C**, which is a reasonable value for this operating condition. It should be noted that the reported COP was calculated solely based on the refrigerant-side enthalpy differences across the compressor and condenser. The electrical power consumption of the compressor drive, condenser fan, cabin blower, and other auxiliary components was not included in the calculation. Therefore, the presented value represents the cycle (thermodynamic) COP rather than the overall system COP, and the actual system-level COP of the heat pump would be lower. For detailed information, please refer to the [results](results) folder of this project, where enthalpy, entropy and temperature values for different parts of the cycle are provided.
<br><br>
<p align="center">
<img width="731" height="523" alt="530534618-effea2f7-4077-4bf4-82ef-bf2cce446ec7" src="https://github.com/user-attachments/assets/822cede5-fdb1-4b8e-8a0d-c17b794ed56b" />
<br>
 
## Support
For any questions regarding the model place a comment in the repository.

## License
The model requires the following products:

- MATLAB® R2024a: Simulink®; Simscape™; Simscape Fluids™; Simscape Battery™; Simscape Driveline™; Simscape Electrical™; Stateflow®
- GT-SUITE

See [license](LICENSE.md) file attached to this repository

## Sources
[1] A Holistic Approach for Designing a Battery Electric Vehicle Thermal Management System, 
Steve Miller, Lorenzo Nicoletti

[2] The MathWorks. “Electric Vehicle Thermal Management - MATLAB & Simulink.”, Available 
[here](https://www.mathworks.com/help/hydro/ug/sscfluids_ev_thermal_management.html). 
 
