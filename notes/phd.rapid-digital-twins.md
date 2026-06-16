---
id: p2b63onupzh26xhdv40pz5h
title: "Towards Lifecycle-complete Rapid Digital Twins: Case study of a Butane Steam-Generating Heat Pump for Design and Virtual Commissioning in the Ahuora Digital Twin Platform"
desc: ''
updated: 1781645948809
created: 1781040524556
bibliography: assets/gl-refs.bib
csl: assets/custom-style.csl
---

# Abstract

One of the core ideas behind Digital Twins is that a model is complete enough to be analysed in many different ways. However, most literature discussing building Digital Twins only uses the resulting model for a single purpose. 
This paper introduces and standardises a workflow to rapidly build Process Digital Twins that can be used across the entire life cycle of the equipment, from design, to commissioning, to operation. 
We explore the creation of a Butane Steam-Generating Heat Pump Digital Twin in the Ahuora Digital Twin Platform, demonstrating how the platform enables rapid construction of Digital Twin models. The model is used for equipment sizing, testing how the heat pump will perform in a variety of conditions using a multi-steady-state analysis. This model is then adapted with minimal changes to be used for virtual commissioning, using hardware-in-the-loop testing to validate the control systems of the PLC.
The results show that the Ahuora Digital Twin Platform can significantly reduce duplication of effort through features that allow design-time models to be easily adapted to also work in commissioning tasks and beyond. We discuss how the workflow presented here can practicably be employed on other cases in the future. 

# Introduction


This paper introduces a procedure for real-time simulation using models built in the Ahuora Digital Twin Platform. 
The Ahuora Digital Twin Platform is a process simulation platform built by the University of Waikato, based on the IDAES equation-oriented modelling framework [@beattie2024idaes]. 
The Ahuora Platform provides a simple drag-and-drop user interface to lower the barrier to entry to working with equation oriented simulators, helping to avoid common problems users may have with constructing a model. Currently, it supports steady-state modelling, multi-steady state analysis, and has experimental dynamic simulation functionality. 
In keeping with the principle of model reuse, additional features have been developed to use the same models for pinch analysis and heat exchanger analysis, and costing. However, no real-time functionalities exist. 
In developing this method, we seek to minimise the amount of work required to create a real-time simulation model from an existing steady-state process flowsheet in the Ahuora Digital Twin Platform. 

The procedure we propose is as follows:
- Use the Ahuora Platform's Variable Replacement technique to reformulate the model to calculate what you are trying to predict in real time;
- Simulate first-order dynamics in a system by replacing equality constraints between unit operations with exponential smoothing filters;
- Tag model inputs and outputs in a manner consistent with existing data collection systems;
- Use our Ahuora-Live integration tool to connect the model to an existing SCADA or PLC system. This is a simple, reusable MQTT Client to receive inputs from the industrial SCADA/PLC system, solve the model, maintain the current state, and return results back to the SCADA/PLC system or process historian.

To validate the effectiveness of this procedure, we implement it using a model of a Butane Steam-Generating Heat Pump, for hardware-in-the-loop testing of the PLC's control system and data logging functionality. 
This allows us to build and test the PLC's systems while the physical heat pump is still under construction. We show that the Digital Twin shows similar enough behaviour to the real system to test the PLC's operation and use the PLC's PID controllers to control the Digital Twin to steady state conditions. 
We then evaluate the procedure we have proposed to identify how effectively it reduces the work required to convert an existing steady-state model in the Ahuora Platform into a real-time simulation integrated with external industrial systems. 
We also discuss limitations of approach we have proposed, and how it could be improved further to be simpler and more generally applicable.


## Background

Digital Twins have become a very common research area over the last decade, including in the process engineering sector [@tao2024advancements]. Literature covers how Digital Twins provide value across the life cycle of the product, including the design phase, construction and commissioning, operation and maintenance, and reconfiguration and retrofit.

In theory, the grand vision of a Digital Twin is that one system representation, the "Digital Twin," can be used to help solve a variety of problems by combining up-to-date data with a currently existing model[@Wright2020ModelVsTwin]. 
In practice, most digital twins in literature only focus on one specific application [@ors2020conceptual], likely because this best fits the narrative flow of an academic article. 
However, reusing a model for multiple applications allows the cost of developing the model to be amortized across the value added by each use.
In order to do so for different applications, tools need to be developed to extract a model from the system representation that is appropriate for each purpose [@birk2022automatic].

Ferle et al. [@Ferle2026ContinuedVCUse] discuss an area where this can be used, in adjusting models built for Virtual Commissioning during the product engineering phase to also deliver benefits in the operational phase. 
Their approach follows the insight of Birk et al. [@birk2022automatic], discussing how the validated models they already had for virtual commissioning could be used for applications such as training human operators, collision avoidance of mechanical systems, and testing new control software. 
They concluded that real-time applications typically require more development, because data aquisition and synchronisation and alignment all needs to be accounted for, but also significant benefit in efficiency, output or quality.
As they started with a virtual commissioning model, their use cases were skewed towards those that are easist to adapt a virtual commissioning model to. 

While the idea of applying a model to a different case is similar to what is discussed in Ferle et al.'s paper, this work starts from a steady-state design-time simulation model. Existing commercial simulation technologies, such as gPROMS, Simulink, or Aspen, include some ways of repurposing models. 
Aspen HYSYS and gPROMS include workflows to convert steady-state models to dynamic process simulation models [@AspenHYSYSDynamics][@SiemensgPROMS], a feature which is currently in development in the Ahuora Digital Twin Platform. 
Simulink is built from the ground up for dynamics, and due to its integration with MATLAB, is best positioned for integration with PLCs and external systems [@MathWorksVirtualCommissioning]. 

Yin et al. discuss a use case of this, where a system-level dynamic model built in Simulink/Modelica is used for pinch analysis of a Power-to-Gas system in design-time, and then for model predictive control during plant operation [@yin2026system]. 
To adjust the model for real-time operations, MATLAB's system identification toolbox is used to estimate system transfer functions and allow a simpler model reformulation. 
An objective for optimal control is also added, and resolved in regular intervals.
In a similar way, our method will need to provide tools to reformulate the model to a more reliably solvable formulation, change the objectives of the model, and re-solve at regular intervals with the latest real-time data. 

# Method

This section explains the procedure one would apply to adjust a steady state model built in the Ahuora Digital Twin Platform for use in a real-time application.

## Reformulating the model

This methodology assumes one already has an existing existing steady-state flowsheet in the Ahuora Digital Twin Platform. 
This model may have been set up for some design-time use case, such as Pinch Analysis, equipment sizing, or equipment rating. 

The model in the Ahuora Platform will be formulated to calculate the properties required for the design-time use case. 
To adjust the model to be suitable for a real-time application, the Ahuora Platform's Variable Replacement functionality can be used to reformulate the model for the target real-time simulation task. 
In the virtual commissioning context, this means calculating sensor-like quantities, such as temperatures, pressures, and flow rates, from actuator or controller outputs.

The Ahuora Platform ensures that the base model always has zero degrees of freedom, by ensuring that any extra variable specified must also free an existing variable. Reformulating the model from a design case to a control or virtual commissioning model will not change the number of things that need to be specified, unless the model itself is fundamentally changed [@luyben1996design]. 

Some cases may require the model to be added. For example, additional model fidelity may be required, or additional properties may need to be calculated that were not on the existing model. (One example of this we saw is calculating the mechanical work of a pump from the VSD speed.) This can be added to the Ahuora model using conventional simulation tools and custom expressions and variables, and should be done in conjunction with the model reformulation. 

When equality constraints are removed, the model is also checked for structural degeneracy. Degeneracy can occur when an upstream unit operation is specified by a condition downstream of the selected breakpoint. 
A useful diagnostic is to examine whether a variable in the Ahuora Digital Twin Platform has been replaced "across" the breakpoint. 
If this is the case, the user may need to adjust their formulation to specify different variables in the equation oriented model, or change which intermediate stream dynamics is added to[@leeidaes].

## Supporting Dynamics

A software tool has been created to emulate dynamic response across subsequent solves of a steady state model built using the Ahuora Platform.

It requires the user to select streams between unit operations where dynamic response is to be represented. The tool removes the equality constraints that require outlet and inlet states to be identical. This decouples the unit operations in the equation oriented model. The downstream inlet properties are then specified directly, and the model is solved at the fixed inlet conditions. 

The user is also required to specify a transfer function to represent the gradual effect of upstream changes propagating through downstream unit operations. 
This can be specified for each property that is passed between streams (pressure, enthalpy, composition, and flow rate).
The easiest way to do so for first-order responses is by using an exponential smoothing filter. The smoothing factor can be related to the desired first-order time constant using:

$$\alpha = 1 - e^{-\frac{\Delta t}{\tau}}$$

A buffer of previous values may also be used to approximate transport delay through the system, to allow a First order plus time-delay response.

At the next timestep, the inlet values are updated based on the previously calculated outlet values and the transfer function, and the model is resolved. This is repeated for every timestep.


## Connecting to live data

Virtually every process will have a naming system to define all the equipment and equipment properties that can be accessed [@meier2026control]. This is usually defined in the Piping & Instrumentation Diagrams. This provides a simple way to map data across a variety of systems. 

A feature has been developed in the Ahuora Platform to allow the user to assign an alphanumeric tag to individual properties in a flowsheet. 
The user should assign tag names and units to model variables and calculated properties using conventions that are consistent with and meaningful to the external control or data acquisition systems.

Once this is done, the reconfigured model and tags should be exported from the Ahuora Platform. These are combined with the user's configuration for dynamic response, and can be loaded into the Ahuora-Live integration tool. This software system acts as an MQTT client that receives input values from the SCADA or PLC system, updates the model state, solves the model, and publishes calculated outputs back via MQTT. The SCADA, PLC, or process historian can then subscribe to the appropriately tagged MQTT topics to recieve the results of the simulation.

MQTT is supported by a very broad range of industrial systems, either directly or via an edge gateway or add-on [@BabayigitMQTT]. A brief summary of some of these is included in the appendix. 
By appropriately configuring the external industrial system to publish and subscribe to the required inputs and outputs, it should be possible to connect the Ahuora-Live model to virtually any industrial system.
Where the physical control system does not use MQTT directly, an appropriate protocol translation layer may be required.

IDAES and the Ahuora Platform include tools that can assist in identifying structurally problematic points, after which the model can be manually reformulated.

# Case Study: Butane Steam-Generating Heat Pump


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

<!--

What limitations are there of this approach and this case study?

Where could you go next? e.g more complex dynamics, system identification like the matlab system identificaiton toolbox (we don't have real data to estimate from)

-->

The Butane Heat Pump case study shows the validity of this approach to adjust a steady-state flowsheet built in the Ahuora Platform to a real-time use case.
It is reasonable to assume that a similar procedure could be used for a different model, as nothing in the method is explicitly tied to the Butane Heat Pump use case.  
However, to verify this, future work should implement this methodology in other conditions to verify if the approach is generic enough to work in many different scenarios.

There are a few other limitations to this case study.
This paper discusses applying the steady state model to a real-time hardware-in-the-loop commissioning use case, and the way the problem needs to be reformulated would be different for a soft sensor or real-time optimisation or control problem.
It also does not investigate the complexities of missing or incorrect real-time data. Literature suggests that additional layers for model calibration and data validation may be required for this [@birk2022automatic].

Future work could also add automatic tools for system identification, such as identifying which transfer function best models the dynamics of the system. 
This is already a well established field [@tan2019industrial], but tools that integrate well with the Ahuora Digital Twin Platform would simplify the process.


# Conclusion


By applying the methods outlined in this paper, users of the Ahuora Digital Twin Platform will be able to reuse existing steady-state models for a variety of real-time applications. 
This enhances the value these models provide and unlock greater understanding of the process, bringing new meaning to the concept of a "Digital Twin" in process engineering.



# Appendix 

## MQTT Use across Process Systems
| Vendor / system                                             |                                                                                                     MQTT support | Notes                                                                                                                                                                                                                                                                                                                             |
| ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Unitronics UniStream / UniLogic**                         |                                                                                         Native PLC configuration | UniLogic supports MQTT broker connections plus publications/subscriptions [@unitronics_unilogic_mqtt].                                                                                                                                                                                                                            |
| **Siemens SIMATIC S7-1200 / S7-1500**                       |                                                                                                      PLC library | Siemens’ LMQTT library implements MQTT client function blocks for S7-1200/1500, including publish/subscribe and TLS [@siemens_lmqtt_s71200_s71500].                                                                                                                                                                              |
| **Schneider Electric Modicon / EcoStruxure Machine Expert** |                                                                                              PLC library/example | Schneider documents an MQTT Handling library/example for controller applications in EcoStruxure Machine Expert [@schneider_mqtt_handling].                                                                                                                                                                                         |
| **ABB AC500 / CP600**                                       |                                                                       PLC/HMI support via ABB libraries/examples | ABB documents AC500 MQTT libraries and examples; FAQ notes AC500 V2 PM556+ and AC500 V3 PM5032+ support, and active CP600 panels support MQTT [@abb_ac500_mqtt].                                                                                                                                                                 |
| **Beckhoff TwinCAT 3**                                      |                                                                                             PLC library/function | TwinCAT TF6701 “IoT Communication” provides MQTT send/receive directly from the controller, with TLS support [@beckhoff_tf6701].                                                                                                                                                                                                  |
| **CODESYS-based PLCs**                                      |                                                                                             Portable IEC library | CODESYS MQTT Client / IIoT Libraries link a CODESYS controller to a broker for publish/subscribe by topic. This matters because many OEM PLCs are CODESYS-based [@codesys_mqtt_client].                                                                                                                                          |
| **Phoenix Contact PLCnext**                                 |                                                                                           Firmware/app ecosystem | PLCnext firmware 2024.0 LTS and newer includes MQTT client capability to communicate with any MQTT broker; PLCnext Store apps are another route [@phoenix_plcnext_mqtt].                                                                                                                                                          |
| **Opto 22 groov EPIC / groov RIO**                          |                                                                                      Native edge/controller MQTT | groov EPIC/RIO include MQTT for IIoT with string and Sparkplug payloads; docs also describe groov Manage, Node-RED, and Ignition Edge routes [@opto22_groov_mqtt].                                                                                                                                                               |
| **Rockwell Automation / Allen-Bradley ecosystem**           |                                                         Usually via FactoryTalk Optix, edge software, or gateway | Rockwell shows MQTT with FactoryTalk Optix and Studio 5000/CompactLogix; this is more integration-oriented than native Logix-controller MQTT [@rockwell_factorytalk_optix_mqtt].                                                                                                                                                 |
| **Inductive Automation Ignition + Cirrus Link**             |                                                                                      SCADA/MQTT platform modules | Ignition IIoT uses MQTT with Ignition; Cirrus Link modules provide MQTT Engine/Transmission/Distributor and Sparkplug support [@inductive_ignition_iiot_mqtt; @cirruslink_mqtt_modules].                                                                                                                                          |
| **AVEVA System Platform / Communication Drivers / DataHub** |                                                                              SCADA driver/broker/adapter support | AVEVA has a standalone MQTT Communication Driver; DataHub can aggregate/standardize MQTT and supports JSON and Sparkplug B [@aveva_mqtt_driver; @aveva_datahub_mqtt_broker].                                                                                                                                                      |
| **PTC Kepware KEPServerEX**                                 |                                                                                                      MQTT driver | Kepware’s MQTT Client Driver exchanges data between MQTT devices/brokers and client applications/OPC tags [@ptc_kepware_mqtt_client].                                                                                                                                                                                            |
| **Emerson DeltaV SaaS SCADA**                               |                                                                                       Cloud SCADA data streaming | Emerson documents DeltaV SaaS SCADA data streaming via MQTT Sparkplug B or JSON, using Emerson broker-as-a-service or a customer broker [@emerson_deltav_saas_mqtt].                                                                                                                                                             |
| **Honeywell Experion / ControlEdge**                        | Mixed: SCADA/RTU protocol ecosystem; MQTT appears in training/docs, less clearly as a general native MQTT driver | Honeywell Experion SCADA is positioned as open SCADA with many RTU/PLC drivers; Honeywell ControlEdge RTU/PLC training references MQTT protocol knowledge and MQTT-based HART-IP connectivity. I would verify exact MQTT driver/module availability with Honeywell for a specific Experion/ControlEdge release [@honeywell_experion_scada_pin; @honeywell_controledge_training_mqtt]. |


# Bibliography
