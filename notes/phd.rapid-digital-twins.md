---
id: p2b63onupzh26xhdv40pz5h
title: "Towards Lifecycle-complete Rapid Digital Twins: Case study of a Butane Steam-Generating Heat Pump for Design and Virtual Commissioning in the Ahuora Digital Twin Platform"
desc: ''
updated: 1781498540763
created: 1781040524556
bibliography:
  - assets/gl-refs.bib
---

# Abstract

One of the core ideas behind Digital Twins is that a model is complete enough to be analysed in many different ways. However, most literature discussing building Digital Twins only uses the resulting model for a single purpose. 
This paper introduces and standardises a workflow to rapidly build Process Digital Twins that can be used across the entire life cycle of the equipment, from design, to commissioning, to operation. 
We explore the creation of a Butane Steam-Generating Heat Pump Digital Twin in the Ahuora Digital Twin Platform, demonstrating how the platform enables rapid construction of Digital Twin models. The model is used for equipment sizing, testing how the heat pump will perform in a variety of conditions using a multi-steady-state analysis. This model is then adapted with minimal changes to be used for virtual commissioning, using hardware-in-the-loop testing to validate the control systems of the PLC.
The results show that the Ahuora Digital Twin Platform can significantly reduce duplication of effort through features that allow design-time models to be easily adapted to also work in commissioning tasks and beyond. We discuss how the workflow presented here can practicably be employed on other cases in the future. 

# Introduction

Digital Twins have become a very common research area over the last decade, including in the process engineering sector [@tao2024advancements]. Literature covers how Digital Twins provide value across the life cycle of the product, including the design phase, construction and commissioning, operation and maintenance, and reconfiguration and retrofit. 

In theory, the idea of a Digital Twin is that one system representation, the "Digital Twin," can be used to help solve a variety of problems. 
Reusing a model for multiple applications is a powerful concept, as the cost of developing the model can be amortized across the value added by each use.
This benefit of digital twins is not always made clear in research, as most papers on Digital Twins focus on one specific application  [@ors2020conceptual]. Reusing the model is often harder than expected, as the model or modelling software may only have been designed with one purpose in mind. Different applications often require a slightly different "view" of the Digital Twin.

This paper introduces a procedure for real-time simulation using models built in the Ahuora Digital Twin Platform. 
The Ahuora Digital Twin Platform is a process simulation platform built by the University of Waikato, based on the IDAES equation-oriented modelling framework [@beattie2024idaes]. 
The Ahuora Platform provides a simple drag-and-drop user interface to lower the barrier to entry to working with equation oriented simulators, helping to avoid common problems users may have with constructing a model. Currently, it supports steady-state modelling, multi-steady state analysis, and has experimental dynamic simulation functionality. 
In keeping with the prinicple of model reuse, additional features have been developed to use the same models for pinch analysis and heat exchanger analyisis, and costing. However, no real-time functionalities exist. 
In developing this method, we seek to minimise the amount of work required to create a real-time simulation model from an existing steady-state process flowsheet in the Ahuora Digital Twin Platform. 

The procedure we propose is as follows:
- Use the Ahuora Platform's Variable Replacement technique to reformulate the model to calculate what you are trying to predict in real time;
- Simulate first-order dynamics in a system by replacing equality constraints between unit operations with exponential smoothing filters;
- Tag model inputs and outputs in a manner consistent with existing data collection systems;
- Use our Ahuora-Live integration tool to connect the model to an existing SCADA or PLC system. This is a simple, reusable MQTT Client to recieve inputs fron the industrial SCADA/PLC system, solve the model, maintain the current state, and return results back to the SCADA/PLC system or process historian.

To validate the effectiveness of this procedure, we implement it using a model of a Butane Steam-Generating Heat Pump, for hardware-in-the-loop testing of the PLC's control system and data logging functionality. 
This allows us to build and test the PLC's systems while the physical heat pump is still under construction. We show that the Digital Twin shows similar enough behaviour to the real system to test the PLC's operation and use the PLC's PID controllers to control the Digital Twin to steady state conditions. 
We then evaluate the procedure we have proposed to identify how effectively it reduces the work required to convert an existing steady-state model in the Ahuora Platform into a real-time simulation integrated with external industrial systems. 
We also discuss limitations of approach we have proposed, and how it could be improved further to be simpler and more generally applicable.


## Literature Review


The most common style of DT that can be used across the product lifecycle is one that is based on process simulation technologies, such as modelica [@yin2026system].

# Method

- Begin with an existing steady-state flowsheet in the Ahuora Digital Twin Platform.
- Use the Ahuora Platform's variable replacement functionality to reformulate the model for the target real-time simulation task. In the virtual commissioning context, this means calculating sensor-like quantities, such as temperatures, pressures, and flow rates, from actuator or controller outputs.
- Assign tag names to model variables and calculated properties using conventions that are consistent with the external control or data acquisition system.
- Export the reconfigured model from the Ahuora Platform for execution outside the platform environment.
- Connect the exported model to the external system using the Ahuora-Live integration tool. This tool acts as an MQTT client that receives input values from the SCADA or PLC system, updates the model state, solves the model, and publishes calculated outputs back to the SCADA, PLC, or process historian.
- To introduce approximate dynamics into a steady-state model, select streams between unit operations and remove the equality constraints that require outlet and inlet states to be identical. The downstream inlet properties are then fixed directly.
- Solve the model at the fixed inlet conditions.
- At the next timestep, update the fixed inlet values using the corresponding calculated outlet values. These values may be copied directly to represent a fast response, or updated using an exponential smoothing filter to represent first-order dynamics.
- The smoothing factor can be related to the desired time constant using:

$$\alpha = 1 - e^{-\frac{\Delta t}{\tau}}$$

- A buffer of previous values may also be used to approximate transport delay through the system.
- When equality constraints are removed, check the model for structural degeneracy. Degeneracy can occur when an upstream unit operation is specified by a condition downstream of the selected breakpoint. A useful diagnostic is to examine whether a variable has been replaced "across" the breakpoint.
- IDAES and the Ahuora Platform include tools that can assist in identifying structurally problematic points, after which the model can be manually reformulated.
- After integration with the external system, use logged MQTT messages or another data ingestion pipeline to inspect the interaction between the model and the controller over time.
- For virtual commissioning, use the real controller where possible, but map simulated measurements into standard memory addresses or communication tags rather than physical analogue input addresses.
- Where the physical control system does not use MQTT directly, an appropriate protocol translation layer may be required.








# System Description and Model









<!-- E -->

![Steam Generating Heat Pump Process Flow Diagram](assets/sghp-pfd.png)

The steam-generating heat pump considered in this case study uses n-butane (R600a) as the refrigerant and is designed to produce up to $65\ \mathrm{kg}\,\mathrm{h}^{-1}$ of steam at 1-3 bar, in addition to hot water. 
Steam is produced by heating water at elevated temperature and pressure before flashing it to generate steam. 
The system is being constructed by the Ahuora Centre for Smart Energy Systems at the University of Waikato, in collaboration with Excel Refrigeration (New Zealand), the Eastern Switzerland University of Applied Sciences, and Bitzer (Germany). 
It is the first system of this type in New Zealand, and the broader objective of the project is to increase confidence in this technology across New Zealand industry.


A model of the steam-generating heat pump, including the source-water heating loop and hot-water loop, was developed in the Ahuora Digital Twin Platform. 
The model used predefined first-principles representations of valves, heat exchangers, compressors, mixers, and splitters from the Ahuora Platform unit operation library, reducing the time required to construct the flowsheet. 
N-butane and water were modelled using Helmholtz energy formulations [@wagner2002iapws] [@bucker2006reference], both of which are supported by the Ahuora Platform property libraries. 
Pipe pressure losses were represented using valves with fixed coefficients and fixed positions, allowing the pumps to determine the flow rate through the system.

![Steam Generating Heat Pump Built in the Ahuora Platform](assets/sghp-ahuora-platform.png)

# Design-time Analysis Case

<!-- Explain how variable replacement makes it easy to switch the model from an initial design to a rating mode case, possibly take some content from the variable replacement paper. -->



# Hardware-in-the-loop Virtual Commissioning

<!-- Explain how the model can be used with external systems easily -->

![View of the Heat Pump on the PLC monitor. All temperatures and pressures are being calculated from a model and sent over MQTT to the PLC, simulating what you would see if the PLC was connected to a real plant.](assets/heat-pump-plc-screen.png)

The PLC software for controlling the heat pump was developed before construction of the physical heat pump had been completed. 
This created a need to test whether the PLC had been configured correctly before it could be connected to the plant. 
The control software uses temperature, pressure, and flow measurements in its PID algorithms, and manipulates valve positions and pump speeds. 
In this work, the heat pump model was used to virtualise the process, allowing the model to calculate the temperature, pressure, and flow measurements expected in response to the PLC output signals.


The University of Waikato selected a Unitronics PLC for the heat pump. The physical PLC was used during virtual commissioning, but simulated measurements were mapped to standard memory addresses rather than analogue input memory addresses. MQTT subscribers were configured in the PLC to read values from a set of MQTT topics, while PLC outputs were also published as MQTT topics. MQTT was selected because it is a common industrial communication standard and allows the interaction between the PLC and the model to be observed by other SCADA systems or data historians.


For this case study, a plugin was developed to associate each model variable or calculated property with a tag name. 
These tag names were selected to follow the same conventions as the PLC. The Ahuora Platform variable replacement feature was then used to reformulate the heat pump model for control simulation, such that temperatures and pressures were calculated from the PLC output variables.

After reconfiguration, the model was downloaded from the platform and executed by a server connected to the MQTT broker. 
The server monitored the MQTT topics associated with the model tags, solved the model when updated values were received, and published the calculated properties back to the broker. 
Because the tag mappings are stored with the Ahuora model, the integration software is not specific to the heat pump case study and can be reused with other tagged models and PLC systems that use matching MQTT tag names.

![Giving properties such as temperature and pressure a custom tag](assets/tagging-properties.png)

## Adding Dynamics

<!-- Explain how the recycle system enables converting the steady state model to a dynamic model with minimal effort -->

The heat pump model contains recycle loops, which can make the corresponding steady-state model unstable if too many transfer variables are specified. 
Transfer variables implicitly define the state of the stream, and may include pressure drop, mechanical work, or heat addition. 
However, recycle loops can also allow no state variables to be specified directly, leaving the model without an appropriate reference point.

In recycle loops, transfer variables must balance across the full system. 
For example, if the compressor mechanical work is too high, the downstream valve may be unable to reduce the pressure sufficiently to meet the required inlet pressure. 
The mathematical solver may then increase the compressor outlet pressure, which also increases the compressor inlet pressure, producing an unstable feedback effect.

In the physical heat pump, this behaviour is managed by PID control loops that prevent pressures from exceeding acceptable limits. 
A purely steady-state model cannot represent this control response directly. In this case study, approximate dynamics were therefore introduced using the procedure described in the Method section.

This formulation allowed the heat pump model to be tuned to reflect the expected response time of the system with minimal changes to the existing steady-state model.

### Limitations

In many models, removing equality constraints in this way does not create additional structural issues. 
However, structural degeneracy can occur when the property of a unit operation upstream of the breakpoint is specified by a condition downstream of the breakpoint. 
This issue is generally identifiable and can be addressed by reformulating the relevant portion of the model. A useful heuristic is that replacing a property "across" a breakpoint is likely to introduce degeneracy. 
IDAES and Ahuora also include tools that can assist in identifying these points for manual reformulation.

## Running the model

![View of the Heat Pump on the PLC monitor. All temperatures and pressures are being calculated from a model and sent over MQTT to the PLC, simulating what you would see if the PLC was connected to a real plant.](assets/heat-pump-plc-screen.png)

After the model was connected to the PLC, the PLC interacted with it in the same manner as it would with the physical heat pump. 
The temperatures and pressures displayed by the PLC were calculated by the model, allowing the control system to be tested before the physical system was available. 
This enabled verification of the PLC configuration and identification of faults in the PLC logic.

A Telegraf data ingestion pipeline was configured to store MQTT messages in a time-series database for visualisation of historical trends. This made it possible to examine how the PID control loops interacted with the heat pump model to stabilise the process and bring it toward equilibrium over time.

![InfluxDB Screen showing how PID is used to stabilise the inputs over time.](assets/heat-pump-pid-stabilisation.png)

The virtual commissioning environment also provided a training environment for future plant operators, allowing them to observe the control system and process response without risk of damaging physical equipment.

One limitation of this approach is that mathematical process models may become unstable or fail to solve under zero-flow conditions, because temperatures and pressures can become undefined. As a result, the model was not fully suitable for representing start-up and shutdown procedures without additional smoothing or minimum-flow assumptions.


# Discussion

<!-- Argue that the variable replacement and the recycling system outlined here can easily be applied to other models. -->
The approach used in this butane heat pump case study can be applied to other process models built in the Ahuora Platform. 
Variable replacement is a generic technique already used across models in the Ahuora Platform, and it enables a single model to be adapted to different analysis scenarios. 
Similarly, removing stream constraints between unit operations and manually recycling values at each timestep provides a simple method for introducing approximate dynamics without requiring specialised dynamic model configuration.

<!-- Argue that the mqtt workflow etc will pretty much work for whatever case you have. -->
At a fundamental level, real-time model execution requires setting the appropriate variable values, solving the model, and publishing the calculated results. 
The MQTT layer provides a useful abstraction for shaping and exchanging these data. 
Although not all industrial systems are configured to use MQTT directly, protocol translation layers are commonly available to convert between MQTT and other industrial communication protocols, making the approach compatible with a wide range of data collection and control systems.


# Conclusions


# Bibliography
