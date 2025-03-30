---
id: 76xc8fam1ote5jgclx082dx
title: Litreview
desc: 
updated: 1743378503507
created: 1743126179488
bibliography: assets/refs.bib
---
# Literature Review

This literature review analyses the most relevant literature on the development methodology used to build Digital Twins for chemical industries.

The design process of a number of Digital Twin Systems is compared to show the common elements and challenges in building a digital twin system.

Then, some existing and emerging technologies are reviewed that may be helpful in creating a process for building digital twins.

## An Overview of Digital Twins

Digital Twins are a broad concept that has been used in many disciplines. It was introduced by Michael Grieves as a way to utilize the wealth of data avaliable from modern digital systems in a way that helps us replicate, visualise, and analyse the physical system without the constraints of the physical realm [[@grieves2014digital]]. 

The term "Digital Twin" is used in a variety of industries. In construction and smart cities, DTs of buildings are used for planning, construction and operation, and to keep track of changes over time - a natural extension to traditional floor plans. In Healthcare, "Digital Twins" are used for personalised medicine prescription. In Manufacturing, Energy Generation, and Aerospace, DTs are used for performance modelling and fault analysis. In Robotics and Automotive industries, DTs are used for real-time decision making, simulation, and training [[@applications_singh_2022]]. 

# Digital Twins from a Control Perspective

The broadest definition of a Digital Twin is *a set of digital data that mirrors a physical object or system.* Different authors have interpreted this differently, and the usage of the word differs across industries. Michael Grieves categorized Digital Twin Development in phases, where the simplest forms of Digital Twins only modeled the form of a product, and more advanced DT systems modelled behaviour, could integrate with other systems, and could use the data to make predictions and act autonomously [[@grieves2023digital]]. In other literature, it is strongly associated with the concept of bi-directional communication between a physical and virtual model [[@agrawaldtconcepttopractice]]. 
<!--- citation needed -->


In the field of Chemical and Process Engineering, Digital Twins are most commonly associated with high fidelity modelling, bi-directional communication, and the resulting adaptivity [[@energy_yu_2022]]. This article will focus on these types of DT systems, rather than DTs used for design or offline analysis.


The bi-directional communication of an autonomous Digital Twin can be better viewed from the perspective of control systems. The fundamental characteristics of such a Digital Twin include:

![Feedback loop between Digital Twin and Physical System, when viewed from a Control Theory Perspective](assets/dt_feedback_loop.drawio.svg)


- **Physical System & Context Awareness**: This type of Digital Twin represents one physical system - i.e., the specific physical object must be identifiable [[@minerva_digital_2020]]. It also may represent the environment of the physical system. 
- **Real-time Updates through Data Collection and Online Learning**: Digital Twins are updated as often as necessary to reflect changes in the physical system. It also stores and updates some "state" that defines key attributes of the physical system. 
- **Digital Modelling and Simulation**: Digital Twins incorporate models—mathematical, machine learning, or hybrid—that represent the system's behavior and can simulate future states under different conditions.
- **Control and Actuation**: Digital Twins can communicate with the physical system through control mechanisms.

Together, these form a control or feedback loop, where the Digital Twin is used to predict the future state of the system, and controlling actions are tested on the Digital Twin before being implemented on the physical asset.


![Evolution of Control Systems](assets/dt_history_control.drawio.svg)


In this way, a digital twin can be viewed as a natural progression of existing control systems.
<!--- 
Write a cited paragraph about how these control systems link together. 
You're gonna have to research what they are first.

-->


### Examples of Digital Twin Technology

- **Chemical Factory Model**: A mathematical model of a factory represents its operations, including equipment degradation, and simulates changes to optimally control processes.
- **Traffic System (Google Maps)**: Models road networks and uses real-time traffic data to predict journey times and suggest optimal routes, effectively controlling traffic flow.
- **Mechanical Robot**: Simulates the robot's response to control inputs, selects the best inputs, and sends them to the robot for execution, combining real-time updates, simulation, and control.


## Adjacent Technologies

There are a number of ongoing areas of research that are relevant to Digital Twins. 

<!--- Summarize them after you've written about them and found a flow -->


### Data Collection & History

In most cases, Digital Twin systems are real-time and thus require data to be collected from the physical system. Additionally, storing and using historical data can be useful for learning interactions within the system and its environment, which can be used to improve the Digital Twin model's ability to simulate and predict responses.

### Mathematical Modelling

This involves using some kind of mathematical or algebraic formula to represent the interactions within the system or between the system and its environment. For example, thermodynamic equations can model the behavior of a chemical reactor. Mathematical models can enrich the data collected from the physical system by calculating properties that may not be directly measurable.

### Sensor/Data Fusion

Sensor fusion combines data from multiple sensors to produce a more accurate representation of the system. This can reduce noise in the data or provide more information than any single sensor can provide. For example, virtual reality headsets use multiple cameras, accelerometers, and gyroscopes to track the user's head position and orientation.

### Control

A digital twin can be viewed as a form of Model Predictive Control, as it is used to control the physical system by simulating the effects of different controlling actions and choosing the best action to apply.

<!---
look at https://www.do-mpc.com/en/latest/theory_mhe.html and also find some papers.
-->

### 3D Modelling and Simulation

3D models can simulate physical systems, such as Newtonian physics, fluid dynamics, heat transfer, or flow. While some definitions of Digital Twins focus on 3D models as the required "physical representation," digital twins only need the data necessary to represent the key characteristics of the physical system.

### Online Learning

In Machine Learning, Online Learning refers to updating a model in real-time as new data comes in, without human input or retraining from scratch. This can encode changes in the state or behavior of the physical system, such as degradation of a battery.

### Multi-Agent Systems

Digital Twins may be designed to work in multi-agent systems, interacting with other digital twins or acting on behalf of the physical system.

### Virtualisation

Virtualisation is similar in concept to a Digital Twin. It is the ability to create a piece of digital software that can replicate how a physical system would behave. It is comonly used in computers and networking, as well as in IoT: A virtual computer can run the same code as a physical computer, and a virtual sensor can respond in the same way that a physical sensor can [[@khan_virtualisation_wireless]]. 

Virtualised systems may not have a single, identifiable real-world counterpart in the same way as Digital Twins. However, the ability to accurately simulate a physical system is essential to Digital Twins, so many concepts and tools can be applied from this field. It can be considered analogous to modelling.


### Digital Twins for Visualisation


### Digital Twins for Maintenance and Optimisation




## Usage in Chemical and Process Engineering



## Gaps

<!--- Put this where it should be, not sure yet-->


A survey by Kritzinger et al. [[@KRITZINGER20181016]] found that many articles that discussed Digital Twins in manufacturing only discussed Digital Models, which do not communicate with a real plant, and Digital Shadows, which recieve data from a real system but do not communicate back. Literature that truly analysed Digital Twin systems is much more scarce, and most literature found were on the topic was reviews and conceptual models. This shows that the field is still relatively new, and designing a Digital Twin is still a complex challenge.

## Digital Twin Development Methodologies

### Case Studies

- **Case Study 1**: [[@bottani2017cyber]]
- **Case Study 2**: ...
- **Case Study 3**: ...

### Discussion

- Common elements of the design process.
- Challenges in building a digital twin system, such as compatibility, complexity, security, and privacy.

## Digital Twin Development Tools in Chemical Engineering

### Standardisation Efforts

[[@duan2020development]]

[[@kugler2021method]]

### Equation-Oriented Modelling Tools

| Tool                  | Features                                                      |
| --------------------- | ------------------------------------------------------------- |
| Modelica              | Extensive libraries, integration with platforms like Modelon. |
| Julia ModelingToolkit | Modern, good integration with solvers.                        |
| Python Pyomo          | Flexible, supports various modeling and optimization tasks.   |
[[@applications_singh_2022]]
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
