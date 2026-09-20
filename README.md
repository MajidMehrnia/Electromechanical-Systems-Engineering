# Motion Control & Electromechanical Systems Engineering

## Description
This repository supports a new product development (NPD) program and focuses on the design, modeling, dynamic simulation, and performance analysis of **electric drive units, power electronics (inverters/DC-DC converters), mechanical transmissions (driveline/gears), and thermal and electric loops**. This project extracts and evaluates key engineering performance metrics relevant to precision motion control, high-torque-density electric motors, and power electronics thermal management under real-world drive cycles and heavy-duty transients for an Electric Vehicle (EV) sedan.

## Project Structure

1. [System Architecture](#01-system-architecture)
2. [Motor & Drive](#02-motor--drive)
3. [Gearbox & Mechanical Load](#03-gearbox--mechanical-load)
4. [Motion & Embedded Control](#04-motion--embedded-control)
5. [Electro-Thermal Co-Simulation](#05-electro-thermal-co-simulation)
6. [ECAD & MCAD Interfaces](#06-ecad--mcad-interfaces)
7. [AI/ML Modeling](#07-aiml-modeling)
8. [Results](#08-results)

   
## 01. System Architecture

The figure below illustrates the system developed using Simulink and its add-on products.  
<img width="1280" height="596" alt="Sim_diagram" src="https://github.com/user-attachments/assets/46be4208-0ef6-48cd-b8c6-e85d227e8c28" />

## 02. Motor & Drive
The electric motor is driven by the power drive electronics and is mechanically connected to the actuator drive system. The simulation framework can be easily extended to describe alternative motion control architectures (e.g., direct-drive or multi-axis linear setups). The motor’s dynamic characteristics and electrical losses are modeled using efficiency maps, with transient temperatures determined by internal losses and thermal mass. To deliver the required torque and force, the motor is coupled with a precision gearbox operating at a constant transmission ratio, with mechanical losses modeled using a constant efficiency.

<img width="1915" height="1009" alt="9-1" src="https://github.com/user-attachments/assets/d6148666-3abd-47bb-82a7-26651afe2487" />

<img width="822" height="718" alt="9-3" src="https://github.com/user-attachments/assets/4258bbc4-0878-4c89-b5d2-305b62115a73" />

<img width="823" height="712" alt="9-4" src="https://github.com/user-attachments/assets/d9590dcb-8e78-456d-b23c-bf49c38252fe" />

## 03. Gearbox & Mechanical Load

<img width="1321" height="766" alt="Driveline" src="https://github.com/user-attachments/assets/8cfc1bcb-6746-4658-8663-d031a9174066" />

<img width="822" height="735" alt="9-2" src="https://github.com/user-attachments/assets/4f8dac9c-34e5-44fd-b418-bc41db60292b" />


## 04. Motion & Embedded Control

<img width="1918" height="795" alt="9-5" src="https://github.com/user-attachments/assets/9f4ab89b-5ed5-444d-8c9b-95f26ebda8a4" />

## 05. Electro-Thermal Co-Simulation


## 06. ECAD & MCAD Interfaces

<p align="center">
 <b>An integrated ECAD-MCAD flow creates a digital thread through the design</b>
<img width="1226" height="364" alt="image" src="https://github.com/user-attachments/assets/567c381d-a811-4ea5-a1e1-e61f3a3767bc" />
</p>
An integrated ECAD-MCAD digital thread connects the electrical architecture and wiring-harness definition with the model-based system engineering environment. Electrical design data, interfaces and connectivity are therefore linked to the MATLAB/Simulink/Simscape/GT-SUITE models used to analyze the EV system. This enables traceability from electrical requirements → E/E architecture → power and control interfaces → electromechanical system models → mechanical integration → verification and PLM release.

<p align="center">
 <b>XML helped connect the traditionally separated ECAD and MCAD
domains</b>
<img width="864" height="1488" alt="image" src="https://github.com/user-attachments/assets/0dbe6e13-daf7-4037-bf8d-1dd9fcb86188" />
</p>

This repository presents a model-based engineering study of an integrated electric vehicle system using MATLAB, Simulink, Simscape and GT-SUITE. The simulation architecture is used to investigate the interaction between electric motors, power electronics, mechanical driveline, control systems, thermal systems and vehicle-level operating scenarios. The engineering focus is on the development and analysis of **electromechanical motion systems** and the interfaces between:

**Electrical Power → Drive & Control → Motor → Mechanical Transmission → System Load**

The original vehicle-level architecture provides the foundation for investigating the interaction between electrical, mechanical, control and thermal domains. The focus of this repository is the engineering optimization of electromechanical motion systems and their interfaces, rather than the development of a complete vehicle model. The entire product lifecycle in this project is governed by the PLM framework, demonstrating how a robust "Single Source of Truth" can be established. This system integrates technical requirements with engineering execution by establishing strict document revision controls, transitioning engineering bills of materials (EBOM) to manufacturing skids (MBOM), and enforcing disciplined change log workflows (ECR/ECO/ECN). Ultimately, every technical optimization, such as power consumption reductions or material reusability, is directly linked to target costing and ROI models, proving that robust engineering governance is a direct driver of corporate profitability. 

## 07. AI/ML Modeling

To enhance model fidelity and accelerate the design process, a novel **ML**-based thermodynamic modeling approach has been developed and validated against **CFD** simulations. Related CFD and AI/ML developments are available in my companion repository: 
[Physics-Informed AI/ML Modeling](https://github.com/MajidMehrnia/Physics-Informed-AI-ML-for-Thermodynamic-Modeling)

## 08. Results

The complete executable models and the underlying management tool is available below:

* Access the simulation files in the [Simulation](input_data) and [Results](results) directories.

<img width="1918" height="793" alt="9-6" src="https://github.com/user-attachments/assets/8eaf0f07-25ab-4d4c-a1bc-33c3a1c1ec37" />

  
<strong style="color:red;">Simulation Results from Simscape Logging</strong>

<img width="975" height="492" alt="T_vs_t_Motor_Battery" src="https://github.com/user-attachments/assets/f6526b99-67fe-4be3-97a3-2bf96466c894" />

<img width="1088" height="534" alt="HF-1" src="https://github.com/user-attachments/assets/87254f23-f38a-4114-9edc-318406de2367" />


The main thermodynamic output of the calculations is the **P–h diagram** of the heat pump cycle, as shown in the figure below. The calculated COP (based on refrigerant enthalpy difference (cycle COP)) is 5.2 at a **condensing temperature of 40 °C**, which is a reasonable value for this operating condition. It should be noted that the reported COP was calculated solely based on the refrigerant-side enthalpy differences across the compressor and condenser. The electrical power consumption of the compressor drive, condenser fan, cabin blower, and other auxiliary components was not included in the calculation. Therefore, the presented value represents the cycle (thermodynamic) COP rather than the overall system COP, and the actual system-level COP of the heat pump would be lower. For detailed information, please refer to the [results](results) folder of this project, where enthalpy, entropy and temperature values for different parts of the cycle are provided.

<p align="center">
<img width="731" height="523" alt="530534618-effea2f7-4077-4bf4-82ef-bf2cce446ec7" src="https://github.com/user-attachments/assets/822cede5-fdb1-4b8e-8a0d-c17b794ed56b" />

## NPI Flowchart

<p align="center">
 <img width="1267" height="703" alt="image" src="https://github.com/user-attachments/assets/735892a3-fd9d-4ce8-bfbb-678039c29123" />
</p>

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
 
