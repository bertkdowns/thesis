---
id: 76xc8fam1ote5jgclx082dx
title: Litreview
desc: ''
updated: 1743126912094
created: 1743126179488
bibliography: assets/refs.bib
---
# Literature Review

This literature review analyses the most relevant literature on the development methodology used to build Digital Twins for chemical industries.

The design process of a number of Digital Twin Systems is compared to show the common elements and challenges in building a digital twin system.

Then, some existing and emerging technologies are reviewed that may be helpful in creating a process for building digital twins.

## An Overview of Digital Twins

Digital Twins are a broad concept that has been used in many disciplines. This article shall use a general definition: A Digital Twin is a set of digital data that mirrors a physical object or system.

The fundamental characteristics of a digital twin system are that:

- A digital twin represents one physical system - i.e., the specific physical object must be identifiable [[@minerva_digital_2020]].
- It stores some "state" that defines key attributes of the physical system.
- It is updated to reflect changes in the physical system.

There are a number of characteristics that are strongly associated with digital twin technology. In different contexts, these properties may be assumed to be part of a Digital Twin, and in consequence, different articles may be describing quite different pieces of technology under the same label of "Digital Twin."

These include:

- **Real-time state updates**: Digital Twins are often updated in real-time, or near real-time, to reflect changes in the physical system.
- **Modelling capabilities**: Rather than just storing the state of the system, many Digital Twins also store the constraints and equations that govern the system. This could be implemented with mathematical models or machine learning methods.
- **Real-time model updates**: A specific case of real-time state updates, where the constraints and equations that govern the system are updated in real-time. This borders on the field of self-adaptive systems.
- **Simulation capabilities**: The ability to use a model to predict what would happen to the state of the system under different conditions.
- **Bi-directional communication**: Some Digital Twin systems also include control systems, and thus are able to influence the state of the physical system.
- **Context**: The Digital Twin may also store context of the environment the physical system is in.
- **Agentic ability**: The Digital Twin may be designed to work in a multi-agent system, interacting with other digital twins or acting on behalf of the physical system.

### Examples

- A Digital Twin of an IoT weather station may receive real-time updates from the weather station. It can then respond to queries for information in the same manner the weather station would, reducing load on the servers and data link of the IoT device. This would be a simple digital twin system, with real-time state updates, a model of the weather station's response to queries, and rudimentary agentic ability.

- A Chemical Engineer may create a mathematical first-principles model of a factory, designed to represent the factory's current state of operations, including degradation of equipment and current feedstock. This could also be considered a rudimentary digital twin, which does not have real-time updates but accurately models the system and can be used to simulate changes to the system.

- Google Maps can be considered a digital twin of the traffic system. It contains a model of the road network and uses traffic data as context in real-time to model and predict journey times. It can use this to suggest the best route to take, a form of agentic ability.

- A Digital Twin of a mechanical robot may be able to model and simulate the robot's response to different control inputs, and then pass the best control inputs to the robot to execute. This would be a digital twin with real-time state updates, simulation capabilities, and bi-directional communication.

## Adjacent Technologies

These technologies may not strictly define a digital twin, but they run parallel to some of the goals of digital twins and are often used together.

### Data Collection & History

In most cases, Digital Twin systems are real-time and thus require data to be collected from the physical system. Additionally, storing and using historical data can be useful for learning interactions within the system and its environment, which can be used to improve the Digital Twin model's ability to simulate and predict responses.

### Mathematical Modelling

This involves using some kind of mathematical or algebraic formula to represent the interactions within the system or between the system and its environment. For example, thermodynamic equations can model the behavior of a chemical reactor. Mathematical models can enrich the data collected from the physical system by calculating properties that may not be directly measurable.

### Sensor/Data Fusion

Sensor fusion combines data from multiple sensors to produce a more accurate representation of the system. This can reduce noise in the data or provide more information than any single sensor can provide. For example, virtual reality headsets use multiple cameras, accelerometers, and gyroscopes to track the user's head position and orientation.

### Control

A digital twin can be used to control the physical system by simulating the effects of different control inputs and then choosing the best input to apply. This is often the domain of Model Predictive Control.

### 3D Modelling and Simulation

3D models can simulate physical systems, such as Newtonian physics, fluid dynamics, heat transfer, or flow. While some definitions of Digital Twins focus on 3D models as the required "physical representation," digital twins only need the data necessary to represent the key characteristics of the physical system.

### Online Learning

In Machine Learning, Online Learning refers to updating a model in real-time as new data comes in, without human input or retraining from scratch. This can encode changes in the state or behavior of the physical system, such as degradation of a battery.

### Multi-Agent Systems

Digital Twins may be designed to work in multi-agent systems, interacting with other digital twins or acting on behalf of the physical system.

## Usage in Chemical and Process Engineering

Digital Twins are used in chemical and process engineering for various purposes, such as optimizing operations, predicting system behavior, and improving safety. Success metrics may include accuracy, efficiency, and adaptability.

## Digital Twin Development Methodologies

### Case Studies

- **Case Study 1**: ...
- **Case Study 2**: ...
- **Case Study 3**: ...

### Discussion

- Common elements of the design process.
- Challenges in building a digital twin system, such as compatibility, complexity, security, and privacy.

## Digital Twin Development Tools in Chemical Engineering

### Equation-Oriented Modelling Tools

| Tool                  | Features                                                                 |
|-----------------------|-------------------------------------------------------------------------|
| Modelica              | Extensive libraries, integration with platforms like Modelon.          |
| Julia ModelingToolkit | Modern, good integration with solvers.                                 |
| Python Pyomo          | Flexible, supports various modeling and optimization tasks.            |

### Digital Twin Platforms

| Platform      | Features                                                                 |
|---------------|-------------------------------------------------------------------------|
| Modelon       | Integration with Modelica.                                             |
| Collimator    | Built on Python and JAX.                                               |
| JuliaSim      | Advanced simulation capabilities.                                      |
| SimScape      | MATLAB-based simulation environment.                                   |
| AspenTech     | Industry-specific solutions.                                           |
| Honeywell     | Industrial automation and control.                                     |
| Siemens       | Comprehensive digital twin solutions.                                  |
| AVEVA         | Focused on process and engineering industries.                         |
| Aspen Hysys   | Process simulation and optimization.                                   |
| Scilab-XCOS   | Open-source modeling and simulation.                                   |

## Emerging Technologies

### Surrogate Modelling

Surrogate models approximate complex systems to reduce computational costs while maintaining accuracy.

### Operator Networks

Operator networks are used to model and simulate complex systems with high efficiency.
