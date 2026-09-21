# Electromechanical Systems Engineering

## Description
This repository supports a New Product Development (NPD/NPI) program focused on the design, modeling, dynamic simulation, and performance analysis of electric drive units, power electronics (inverters/DC-DC converters), mechanical transmissions (driveline/gears), and coupled electrical and thermal systems.

The project evaluates key engineering performance metrics for **high-torque-density electric motors** and **power-electronics thermal management** under real-world drive cycles and heavy-duty transient operating conditions for an electric vehicle (EV) sedan. The novelty lies in the **integrated co-simulation of electrical and control loops with mechanical system dynamics**, enabling system-level analysis of coupled electromechanical interactions. 

One of the key objectives of this development is to develop an [**AI/ML-based engineering software framework**](#07-aiml-modeling) for the design and optimization of EV thermal systems across a wide range of motor, drive and power-capacity configurations.

## Project Structure

1. [System Architecture](#01-system-architecture)
2. [Motor & Drive](#02-motor--drive)
3. [Gearbox & Mechanical Load](#03-gearbox--mechanical-load)
4. [Motion & Embedded Control](#04-motion--embedded-control)
5. [Electro-Thermal Co-Simulation](#05-electro-thermal-co-simulation)
6. [ECAD & MCAD Interfaces](#06-ecad--mcad-interfaces)
7. [AI/ML Modeling](#07-aiml-modeling)
8. [NPI Flowchart](#08-npi-flowchart)
9. [Results](#09-results)

   
## 01. System Architecture

The figure below shows the complete electromechanical system architecture co-simulated using Simulink, Simscape, and GT-SUITE. Blue signal and physical lines represent the thermodynamic and fluid networks, including refrigerant loops, coolant channels, the chiller, radiator, and evaporator circuits. Brown lines denote the electrical power distribution and control interconnections between the high-voltage battery, DC-DC converter, charger, PTC heater, and electric motor drive. This integrated Simscape environment enables precise multi-physics dynamic simulation to evaluate transient thermal responses and energy efficiency across demanding vehicle drive cycles.
<br><br>
<img width="1280" height="596" alt="Sim_diagram" src="https://github.com/user-attachments/assets/46be4208-0ef6-48cd-b8c6-e85d227e8c28" />
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
## 03. Gearbox & Mechanical Load

This Simscape Driveline Simple Gear block parameterizes the mechanical reduction ratio between the electric motor and the driven axle. The transmission is configured with a continuous reduction gear ratio of $N_F/N_B = 9$ and same-direction output shaft rotation to amplify motor torque delivered to the driveline. Mechanical meshing losses are modeled using a constant efficiency formulation fixed at $97\%$ ($\eta = 0.97$) with a follower power threshold of $0.001\text{ W}$. This high-efficiency mechanical reduction unit enables accurate power transfer calculation and dynamic driveline load evaluation across vehicle operating cycles.
<br><br>
<img width="822" height="735" alt="9-2" src="https://github.com/user-attachments/assets/f738b58f-ef44-48c9-b39d-2a0d573a8668" />
<br><br>
<img width="821" height="466" alt="Driveline" src="https://github.com/user-attachments/assets/8cfc1bcb-6746-4658-8663-d031a9174066" />
<br><br>
## 04. Motion & Embedded Control
The control architecture is designed in **Simulink** to enable precise motor control, dynamic load tracking, and integrated electro-thermal management:

* **Electric Motor & Motion Control:** Executes speed and torque command generation ($T_{cmd}$) based on driver demand ($VehSpdRef$), enabling dynamic load regulation, precise motion tracking, and transient torque control for the electric drive unit.
* **Thermal Protection & Component Actuation:** Generates closed-loop control signals ($cmd$) for coolant pumps (motor and inverter loops), the refrigerant compressor, and the condenser fan to ensure active thermal protection during high-torque transients.
* **Multi-Loop Thermal Regulation:** Controls radiator and chiller bypass valves, dynamically switching between series and parallel cooling modes based on real-time component temperatures ($T_{motor}$, $T_{coolant\_inverter\_out}$).
* **Cabin Climate Management:** Integrates HVAC blower and PTC heater actuation to satisfy climate setpoints ($T_{setpoint}$) without compromising powertrain thermal safety.
<br><br>
<img width="1918" height="795" alt="9-5" src="https://github.com/user-attachments/assets/9f4ab89b-5ed5-444d-8c9b-95f26ebda8a4" />
<br><br>

## 05. Electro-Thermal Co-Simulation
This GT-SUITE sub-model captures the detailed 1D thermal-fluid dynamics of the refrigerant compressor loop co-simulated directly with Simulink. The circuit models two-phase refrigerant flow through inlet and outlet piping (`PipeRound`) connected between environmental boundary conditions and the compressor unit. Rotational speed commands and boundary states are dynamically exchanged with the Simulink control model via dedicated co-simulation interface ports. A specialized initialization block (`RefrigCircInit`) establishes state convergence for the refrigerant loop to ensure stable transient simulation during vehicle operational cycles.
<br><br>
<img width="1280" height="542" alt="GT-SUITE_blocks" src="https://github.com/user-attachments/assets/6f010da7-290a-4381-9ed3-0ad721b30cfa" />
<br><br>
## 06. ECAD & MCAD Interfaces
<br>
<p align="center">
 <b>An integrated ECAD-MCAD flow creates a digital thread through the design</b>
<img width="1226" height="364" alt="image" src="https://github.com/user-attachments/assets/567c381d-a811-4ea5-a1e1-e61f3a3767bc" />
</p>
<br>
An integrated ECAD-MCAD digital thread connects the electrical architecture and wiring-harness definition with the model-based system engineering environment. Electrical design data, interfaces and connectivity are therefore linked to the MATLAB/Simulink/Simscape/GT-SUITE models used to analyze the EV system. This enables traceability from electrical requirements → E/E architecture → power and control interfaces → electromechanical system models → mechanical integration → verification and PLM release.
<br><br>
<p align="center">
 <b>XML helped connect the traditionally separated ECAD and MCAD
domains</b>
<img width="864" height="1488" alt="image" src="https://github.com/user-attachments/assets/0dbe6e13-daf7-4037-bf8d-1dd9fcb86188" />
</p>
<br><br>
This repository presents a model-based engineering study of an integrated electric vehicle system using MATLAB, Simulink, Simscape and GT-SUITE. The simulation architecture is used to investigate the interaction between electric motors, power electronics, mechanical driveline, control systems, thermal systems and vehicle-level operating scenarios. The engineering focus is on the development and analysis of **electromechanical motion systems** and the interfaces between:

**Electrical Power → Drive & Control → Motor → Mechanical Transmission → System Load**

The original vehicle-level architecture provides the foundation for investigating the interaction between electrical, mechanical, control and thermal domains. The focus of this repository is the engineering optimization of electromechanical motion systems and their interfaces, rather than the development of a complete vehicle model. The entire product lifecycle in this project is governed by the PLM framework, demonstrating how a robust "Single Source of Truth" can be established. This system integrates technical requirements with engineering execution by establishing strict document revision controls, transitioning engineering bills of materials (EBOM) to manufacturing skids (MBOM), and enforcing disciplined change log workflows (ECR/ECO/ECN). Ultimately, every technical optimization, such as power consumption reductions or material reusability, is directly linked to target costing and ROI models, proving that robust engineering governance is a direct driver of corporate profitability. 
<br><br>
## 07. AI/ML Modeling

This section outlines the AI/ML surrogate modeling framework developed using **Artificial Neural Networks (ANN)** to predict total system electromechanical performance. Trained on multi-physics datasets spanning varied electric motor specifications, gear ratios, and driveline parameters, the model rapidly estimates system-level thermal behavior. This data-driven approach replaces computationally expensive finite-element and lump-parameter dynamic simulations with high-speed predictive modeling. The ANN framework enables real-time optimization and rapid design-space exploration across diverse powertrain configurations and operating profiles.
<br><br>
The proposed software enables a new generation of AI-assisted system design by integrating electro-mechanical based simulations with Artificial Neural Network (ANN) optimization. It significantly reduces development time while maintaining engineering credibility, making it well-suited for OEM-level decision support in electromechanical system development.
<br><br>
Here we can just publish, a novel ML-based thermodynamic modeling approach has been developed and validated against simulations. Related AI/ML developments are available in my companion repository:
[Physics-Informed AI/ML Modeling](https://github.com/MajidMehrnia/Physics-Informed-AI-ML-for-Thermodynamic-Modeling)

Detailed technical information on this AI/ML framework is provided in the following sub-sections.

# **AI-Integrated System Design & Optimization Software**

## **7.1. Overview**

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
<td>Electro-thermal simulation integrated with Artificial Neural Network (ANN) surrogate modeling</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Primary Goal</b></td>
<td>Accelerate system concept design and engineering decision-making for electric mobility applications</td>
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
<td>Higher engineering productivity, faster decision-making, improved work quality and reduced development costs</td>
</tr>
</table>

---

## **7.2. Objective of the Software**

The objective is to integrate **physics-based engineering simulation, AI surrogate modeling and optimization** into a decision-support platform for early-stage system development.

The software is designed to:

* Accelerate early-stage electromechanical and electro-thermal system architecture development.
* Bridge high-fidelity physics-based simulations with fast AI-driven surrogate models.
* Reduce repetitive computational effort during design-space exploration.
* Enable automated optimization of system architectures and engineering parameters.
* Provide quantitative engineering trade-off analysis to support OEM and R&D decision-making.
* Improve engineering productivity by reducing repetitive simulation and evaluation activities.
* Improve work quality through structured validation, traceability and engineering constraints.
* Reduce development and simulation costs by minimizing unnecessary high-fidelity iterations.
* Apply AI selectively to engineering use cases where it provides measurable productivity, quality or cost benefits.
* Critically evaluate AI-generated outputs before they are used for engineering decisions.

### **AI Value Creation**

| AI Application               | Engineering Value                                                                            |
| ---------------------------- | -------------------------------------------------------------------------------------------- |
| **Productivity Improvement** | Reduce repetitive simulation, parameter sweeps and manual design-space evaluation            |
| **Decision-Making**          | Enable rapid quantitative comparison of architectures and design alternatives                |
| **Work Quality**             | Improve consistency, traceability and validation of engineering evaluations                  |
| **Cost Reduction**           | Reduce computational effort, engineering iteration time and unnecessary prototype evaluation |

---

## **7.3. Software Workflow**

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

## **7.4. Overall Software Architecture**

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

## **7.5. OEM & Engineering Input Parameters**

### **Vehicle & System Requirements**

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

### **Electric Motor Specifications**

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

### **Power Electronics & Drive Parameters**

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

### **Thermal Management Parameters**

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

## **7.6. Physics-Based Simulation Core**

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

## **7.7. ANN-Based AI Engine — Surrogate Modeling**

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

## **7.8. AI Output Evaluation & Engineering Validation**

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

## **7.9. Optimization & Engineering Decision Support**

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">Engineering Function</font></th>
<th><font color="white">AI / Software Contribution</font></th>
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

### **AI-Driven Engineering Value**

<table>
<tr bgcolor="#1F4E78">
<th><font color="white">AMETEK AI Requirement</font></th>
<th><font color="white">Implementation in the Platform</font></th>
</tr>
<tr>
<td><b>Improve Productivity</b></td>
<td>Reduce repetitive high-fidelity simulations, manual parameter sweeps and engineering evaluation effort</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Improve Decision-Making</b></td>
<td>Provide rapid, quantitative comparison of architectures, parameters and system trade-offs</td>
</tr>
<tr>
<td><b>Improve Work Quality</b></td>
<td>Apply structured validation, physical plausibility checks, constraint verification and engineering review</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Reduce Costs</b></td>
<td>Reduce computational effort, engineering iteration time and unnecessary prototype or physical evaluation</td>
</tr>
<tr>
<td><b>Identify AI Use Cases</b></td>
<td>Apply AI where surrogate modeling provides measurable value over repeated high-fidelity simulation</td>
</tr>
<tr bgcolor="#F3F8FC">
<td><b>Critically Evaluate AI Outputs</b></td>
<td>Validate predictions against physics-based models, engineering constraints and the validated model domain</td>
</tr>
</table>

---

## **7.10. Development Roadmap**

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

---

## **Engineering Principle**

> **AI accelerates engineering decisions; physics-based models and engineering judgment remain the validation authority.**

This approach positions AI as an **engineering productivity and decision-support capability**, rather than a replacement for physics-based engineering analysis or engineering judgment.



## 08. NPI Flowchart

<p align="center">
 <img width="1267" height="703" alt="image" src="https://github.com/user-attachments/assets/735892a3-fd9d-4ce8-bfbb-678039c29123" />
</p>


## 09. Results

The complete executable models and the underlying management tool is available below:

* Access the simulation files in the [Simulation](input_data) and [Results](results) directories.

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

## Project status
In development

## Sources
[1] A Holistic Approach for Designing a Battery Electric Vehicle Thermal Management System, 
Steve Miller, Lorenzo Nicoletti

[2] The MathWorks. “Electric Vehicle Thermal Management - MATLAB & Simulink.”, Available 
[here](https://www.mathworks.com/help/hydro/ug/sscfluids_ev_thermal_management.html). 
 
