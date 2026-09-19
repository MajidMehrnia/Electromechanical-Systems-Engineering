
# Motion Control & Electromechanical Systems Engineering
### Model-Based Development | Motor & Drive Systems | Controls | Power Transmission | System Simulation

## Description
This repository focuses on the modeling, dynamic simulation, and thermal analysis of **electric drive units, power electronics (inverters/DC-DC converters), mechanical transmissions (driveline/gears), and liquid cooling loops**. This project extracts and evaluates key engineering performance metrics relevant to precision motion control, high-torque density electric motors, and power electronics thermal protection under real-world drive cycles and heavy duty transients.

The figure below illustrates the virtual vehicle developed using Simscape and its add-on products.  

![Sim_diagram](https://github.com/user-attachments/assets/9ac5de1f-6cb7-4017-9ff3-9ec4309d36f7)


These signals enable real-time interaction between the compressor model and the system-level.

The original vehicle-level architecture provides the foundation for investigating the interaction between electrical, mechanical, control and thermal domains. The focus of this repository is the engineering optimization of electromechanical motion systems and their interfaces, rather than the development of a complete vehicle model. The entire product lifecycle in this project is governed by the PLM framework, demonstrating how a robust "Single Source of Truth" can be established. This system integrates technical requirements with engineering execution by establishing strict document revision controls, transitioning engineering bills of materials (EBOM) to manufacturing skids (MBOM), and enforcing disciplined change log workflows (ECR/ECO/ECN). Ultimately, every technical optimization, such as power consumption reductions or material reusability, is directly linked to target costing and ROI models, proving that robust engineering governance is a direct driver of corporate profitability. 

The complete executable models and the underlying management tool is available below:

* Access the simulation files in the [Simulation](input_data) and [Results](results) directories.

<p align="center">
 <img width="1267" height="703" alt="image" src="https://github.com/user-attachments/assets/735892a3-fd9d-4ce8-bfbb-678039c29123" />
</p>

## Overview

This repository presents a model-based engineering study of an integrated electric vehicle system using **MATLAB, Simulink, Simscape and GT-SUITE**. The simulation architecture is used to investigate the interaction between **electric motors, power electronics, mechanical driveline, control systems, thermal systems and vehicle-level operating scenarios**. The engineering focus is on the development and analysis of **electromechanical motion systems** and the interfaces between:

**Electrical Power → Drive & Control → Motor → Mechanical Transmission → System Load**

---

## Engineering Focus

The model is structured around the engineering disciplines relevant to modern motion-control and electromechanical products:

- Motor and drive-system behavior
- Electromechanical power conversion
- Motor speed and torque response
- Mechanical power transmission
- Driveline dynamics
- Control-system architecture
- Closed-loop system behavior
- Power electronics interfaces
- Electrical and thermal system interaction
- System-level simulation and performance analysis
- Model-based engineering and virtual validation


##  System Design  

This repository is based on a thermal management model originally developed in Simulink [1].

In addition to the Simulink implementation, the refrigerant-based thermal management system is also modeled independently in GT-SUITE. This enables a detailed system-level representation of the vapor compression cycle, including the compressor, condenser, expansion device, and evaporator, with high-fidelity thermodynamic and component performance modeling.

The combined use of **Simulink** (control-oriented modeling) and **GT-SUITE** (1D multi-physics system simulation) provides a comprehensive multi-fidelity framework for thermal system design.

To enhance model fidelity and accelerate the design process, a novel **ML**-based thermodynamic modeling approach has been developed and validated against **CFD** simulations. Related CFD and AI/ML developments are available in my companion repository: 
[Physics-Informed AI/ML for Thermodynamic Modeling](https://github.com/MajidMehrnia/Physics-Informed-AI-ML-for-Thermodynamic-Modeling)


### Main Thermodynamic Output
The main thermodynamic output of the calculations is the **P–h diagram** of the heat pump cycle, as shown in the figure below. The calculated COP (based on refrigerant enthalpy difference (cycle COP)) is 5.2 at a **condensing temperature of 40 °C**, which is a reasonable value for this operating condition. It should be noted that the reported COP was calculated solely based on the refrigerant-side enthalpy differences across the compressor and condenser. The electrical power consumption of the compressor drive, condenser fan, cabin blower, and other auxiliary components was not included in the calculation. Therefore, the presented value represents the cycle (thermodynamic) COP rather than the overall system COP, and the actual system-level COP of the heat pump would be lower. For detailed information, please refer to the [results](results) folder of this project, where enthalpy, entropy and temperature values for different parts of the cycle are provided.

<p align="center">
<img width="731" height="523" alt="530534618-effea2f7-4077-4bf4-82ef-bf2cce446ec7" src="https://github.com/user-attachments/assets/822cede5-fdb1-4b8e-8a0d-c17b794ed56b" />


## Post-processing

<strong style="color:red;">Simulation Results from Simscape Logging</strong>

<img width="959" height="577" alt="BraytonCycleGasTurbineExample_05" src="https://github.com/user-attachments/assets/317acdc4-4f66-4287-827e-1f1a54f96aa2" />


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
 
