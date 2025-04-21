---
id: 76xc8fam1ote5jgclx082dx
title: Litreview
desc: 
updated: 1744933248711
created: 1743126179488
bibliography: assets/refs.bib
---
# Literature Review

This literature review analyses the most relevant literature on the development methodology used to build Digital Twins for chemical industries.

The design process of a number of Digital Twin Systems is compared to show the common elements and challenges in building a digital twin system.

Then, some existing and emerging technologies are reviewed that may be helpful in creating a process for building digital twins.

## An Overview of Digital Twins

Digital Twins are a broad concept that has been used in many disciplines. It was introduced by Michael Grieves as a way to utilize the wealth of data avaliable from modern digital systems in a way that helps us replicate, visualise, and analyse the physical system without the constraints of the physical realm [@grieves2014digital]. 

The term "Digital Twin" is used in a variety of industries. In construction and smart cities, DTs of buildings are used for planning, construction and operation, and to keep track of changes over time - a natural extension to traditional floor plans. In Healthcare, "Digital Twins" are used for personalised medicine prescription. In Manufacturing, Energy Generation, and Aerospace, DTs are used for performance modelling and fault analysis. In Robotics and Automotive industries, DTs are used for real-time decision making, simulation, and training [@applications_singh_2022]. 



The broadest definition of a Digital Twin is *a set of digital data that mirrors a physical object or system.* Different authors have interpreted this differently, and the usage of the word differs across industries. Michael Grieves categorized Digital Twin Development in phases, where the simplest forms of Digital Twins only modeled the form of a product, and more advanced DT systems modelled behaviour, could integrate with other systems, and could use the data to make predictions and act autonomously [@grieves2023digital]. In other literature, it is strongly associated with the concept of bi-directional communication between a physical and virtual model [@agrawaldtconcepttopractice][@KRITZINGER20181016]. 

![Digital Twin System Classifications (See also [@WALMSLEY2024100139], [@energy_yu_2022] and [@applications_singh_2022] )](assets/dt_focus.drawio.svg)



In the field of Chemical and Process Engineering, Digital Twins are most commonly associated with high fidelity behavioural modelling, and bi-directional communication, [@energy_yu_2022]. A whitepaper from ArcWeb differentiates between "Project Digital Twins" used for offline analysis in design and construction, and "Performance Digital Twins" used for operations and maintenance. 
A DT system could be made or adapted for either, but bi-directional communication is a characteristic of Performance Digital Twins by nature of requiring a physical system running at the same time. This article will focus on this kind of Digital Twin system, though the attributes of Performance Digital Twins and Project Digital Twins strongly intersect. 


## Digital Twins from a Control Perspective

The bi-directional communication of an autonomous Digital Twin can be better viewed from the perspective of control systems. The fundamental characteristics of such a Digital Twin include:

![Feedback loop between Digital Twin and Physical System, when viewed from a Control Theory Perspective](assets/dt_feedback_loop.drawio.svg)


- **Physical System & Context Awareness**: This type of Digital Twin represents one physical system - i.e., the specific physical object must be identifiable [@minerva_digital_2020]. It also may represent the environment of the physical system. 
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

## A Brief Survey of Digital Twin Development Methodologies

A survey by Kritzinger et al. [@KRITZINGER20181016] found that many articles that discussed Digital Twins in manufacturing only discussed Digital Models, which do not communicate with a real plant, and Digital Shadows, which recieve data from a real system but do not communicate back. Literature that truly analysed Digital Twin systems is much more scarce, and most literature found were on the topic was reviews and conceptual models. The field is still relatively new, and designing a Digital Twin is still a complex challenge.

To gain an understanding of how Digital Twins are currently being built in Literature, this section discusses a number of recent articles published with the keywords "Digital Twin Case Study". 
The articles were chosen to focus on Manufacturing and Process Engineering fields, on Digital Twins used during operation for real-time monitoring, optimisation, and control.
Particular attention is given to those with bi-directional communication between the Digital Twin and Physical asset.

Each paper discussed tackles the problem of building a digital twin in a slightly different way. This section aims to answer the questions "How does software and technology assist the creation of Digital Twins?" and "What challenges do current techniques face that increase the cost of building a Digital Twin of a chemical process?"  by comparing the tools, frameworks, software, and standards used in each study.


### Fiber Processing Plant

Azangoo et Al. [@azango_dt_pid_generation] demonstrated a methodology to generate a Digital Twin from Process & Instrumentation Diagrams (P&ID) using text recognition and image processing technologies, and a user interface to allow an engineer to link up anything that wasn't automatically recognised. 

The raw file was converted to the DEXPI file format [@dexpi_summary] ^[Examples of DEXPI files and the specification can be found at [https://gitlab.com/dexpi](https://gitlab.com/dexpi)], and this was converted into an intermediate Graph Model designed to make importing and mapping operations easier. 
This model was then imported into the BALAS simulation software [@vttbalas], which was used for steady-state simulation. In theory, an integration could be easily written for other simulation software as well. 

This is a good technique for building a Digital Twin where no digital models are already present, particularly for systems designed before the rise of Industry 4.0 techniques. It dramatically reduces the amount of manual work required to recreate the plant. Even for newer systems, using data from a DEXPI file as a starting point could reduce the cost of building a Digital Twin. However, this study focuses on building the model; interconnection with the plant's data control system (DCS) was not included.

### Brewery

The application of Digital Twins to Food Processing Industries was discussed by Kolouris et al. [@KOULOURIS2021317] using a brewery as a case study. Because of the complex chemical composition, modelling chemical and thermophysical characteristics using material and energy balances is much less feasible. 
However, as a brewery is a batched process, the major source of inefficiency is in scheduling. The DT focused on defining the characteristics of the plant, and the brewing recipe, so that schedules could be automatically generated. Based on live data from the plant, as soon as delays are encountered, the future schedule could be automatically recalculated from the constraints present in the Digital Twin. Uncertianty could also be modelled using Monte-Carlo techniques.

### Tissue Paper Mill

A whitepaper by VTT on Process modelling and Simulation [@caro_vtt_process_modelling_whitepaper] outlines how an existing paper mill was retrofitted to cut water consuption. A reference model was able to provide a baseline of performance, and this could be validated using sensor data. This enabled greater visibility into the cause for problems, and helped narrow down the cause of water consumption so that improvements could be made. Design changes could be tested on the virtual systems before applying them to the physica systems.


### Thermoforming Machine

Turan et al. discuss a case study where a DT is created using Finite Element Simulation of a thermoforming process [@digital_turan_2022]. The DT collects data in real time from the physical twin, such as temperatures, material thickness, compressed air pressure, and deflection. It optimises those parameters that can be changed, such as the heating power load and the process timing.  These are then sent to the physical twin via the Programmable Logic Controller (PLC). This was able to decrease the failure rate, and thus the material consumption. 

Finite Element Modelling was performed from first principles in this study, which may not be scalable up to larger systems without the aid of other tools. However, the data collection pipeline discussed using node-RED as a tool for manipulating the various incoming data streams into a required format. This technique could be generalized to other DT applications.

### Heat Production Plant

Martinez et al. discuss tracking systems [@martinez_tracking_simulation], which are used to update a First Principles model based on real-time feedback. A design-time model can be adapted by using optimisation techniques to adjust the model parameters to minimize the difference between model results and physical plant results. This can then be performed continuously as new data is collected, while at the same time the model can be used to predict future performance. OPC UA was used as the communication mechanism, because it enables both real-time and historical data access functionalities [@opc_spec].

Adjusting a model to minimise the difference between model results and physical plant results would be a common requirement of DT sytems.
These techniques could systematized to reduce the cost of developing a DT model. 


<!--
### Thermal Incubator
https://arxiv.org/pdf/2102.10390

A example of a small scale DT system, which focuses on how to build the DT. The interesting part is defining the difference between a DT and a cyber physical system - in a DT there are two cyber physical systems, the physical CPS and the virtual CPS.
Also helpful is the fact that it says DT are also called self-adaptive systems, autonomic computing, Industry 4.0, models@runtime, and supervisory controllers.

-->

### Discussion

At the core of each of these case studies is a model of the behaviour of a chemical process. However, this model is combined with different data to adapt it to each different use case. The Brewery added scheduling data to predict optimal schedules, the heat production plant added real-time data to fine-tune parameters, and the paper mill added historical data and potential design changes to validate new design ideas.

Working with such a diverse range of data and other tools can be a challenge, which explains why most digital twin systems are custom-built to the specific use case. However, the core model is relatively similar in each case. If the core model can be reused with a variety of adjacent techologies, it will increase the Digital Twin's value and reduce development cost of each subsequent feature. 

## Adjacent Technologies

There are a number of ongoing areas of research mentioned or adjacent to the tools used  in the previous case studies. This section summarises the state-of-the-art in each of the more frequently mentioned tools, and discusses how they are applied in building a Digital Twin, or how their application could assist in building Digital Twins of Chemical Processes.

<!--- Summarize them after you've written about them and found a flow -->


### Data Collection & History

In most cases, Digital Twin systems are real-time and thus require data to be collected from the physical system. Additionally, storing and using historical data can be useful for learning interactions within the system and its environment, which can be used to improve the Digital Twin model's ability to simulate and predict responses.

### Mathematical Modelling

This involves using some kind of mathematical or algebraic formula to represent the interactions within the system or between the system and its environment. For example, thermodynamic equations can model the behavior of a chemical reactor. Mathematical models can enrich the data collected from the physical system by calculating properties that may not be directly measurable.

There are two main paradigms of note: Algebraic Modelling and numerical computing.

Table: Comparison of Algebraic Modelling Languages and Numerical Computing Libraries

Feature / Aspect | Algebraic Modeling Languages (AMLs) | Numerical Computing Libraries
--- | --- | ---
Purpose | High-level formulation of optimization problems | Low-level numerical computation and algorithm design
Examples | Pyomo, JuMP, CVXPY, PuLP | JAX, NumPy, CasADi, TensorFlow, SciPy
Problem Representation | Declarative: equations/constraints defined like math | Imperative: custom code defines the computation logic
Ease of Use | Very user-friendly for optimization problems | Requires deeper programming and mathematical understanding
Differentiation | Mostly external or automatic via solvers | Built-in or symbolic (e.g., JAX, CasADi support autodiff)
Solver Integration | Built-in support for various optimization solvers | Must integrate solvers manually (if used for optimization)
Suitable Problem Types | LP, MILP, NLP, convex, robust optimization | General numerical problems, ODEs, PDEs, ML, simulation
Dynamics & Control Support | Not a core functionality, possible with pyomo.dae and gekko | Strong support (especially CasADi, JAX with custom dynamics)
Scalability & Performance | Depends on solver | Highly scalable with JIT, GPU, vectorization
Paradigm | High-level, mathematical modeling | Low-level, computational modeling

*Algebraic modelling libraries*  (AMLs) enable using a very high level problem specification. Variables and equations to constrain your variables are declared symbolically. Objectives can also be added to create optimization problems. The AML decomposes the system of equations and sends it to to a mathematical optimization solver. The system can be solved for any given set of known parameters. This adds a lot of flexibility as it is trivial to change which variables are unknown and which are known in the declarative model [@fragniere2002optimization].

Various AMLs have been built, both commercial and non-commercial. Historically, they have been distint languages, such as AIMMS, AMPL, and GAMS. However, more  recently, open source AMLs have been built on top of existing general-purpose programming languages, such as Pyomo and JuMP, based on Python and Julia respectively. This adds more flexibility to define and build models programmatically [@jusevivcius2021experimental], and apply a variety of model transformations [@hart2017pyomo].  

AMLs are helpful in chemical modelling because it is easily to directly recreate known physical equations, such a a thermodynamic equation of state, in an AML. They also work well with nonlinearities, which is required in almost all chemical systems. Some examples of AMLs used in this way include IDAES-PSE and GEKKO. IDAES-PSE is built on Pyomo, and declares a variety of reusable chemical unit models using mass and energy balances. It uses the flexibility of python and pyomo to make these customizable and composable into complex flowsheets [@lee2021idaes]. GEKKO is a seperate python-based AML that creates AMPL models internally, but has a similar library to IDAES for chemical operations [@beal2018gekko].

Because AMLs are particularly suited towards optimization, they are often used in design-time steady-state simulation in chemical engineering. However, they can also support dynamic simulation, through Differential-Algebraic Equations, which can be converted into algebraic finite-difference formulations [@nicholson2018pyomo]. Pyomo-DAE supports this, which can be used for dynamical optimization such as Model Predictive Control [@PARKER2023103113].

*Numerical Computing Libraries* are lower level, and provide support for numerical transformations of data, including parallel/array computing, automatic differentiation ^[Automatic differentiation is also somtimes called algorithmic differentiation.], ODE solving, and optimization. These can be used to do much the same thing as an AML, and because they are lower level they can be optimised for a specfic use case. CasADi is an example of this. It is a symbolic framework, storing mathematical expressions similar to an AML, and it implements automatic differentiation, which is in turn used for non-linear optimisation and model predictive control [@andersson2019casadi]. It can be used for optimisation the same as traditional AMLs, especially with it's Opti toolbox, but it can also be used for initial-value problems and ODEs through an integrator, and for solving via an rootfinder. Jax is relatively similar to CasADi, with plugins for optimisation and solving, but also a strong focus on machine learning applications [@rader2024optimistix].

These libraries are better suited to solving specific problems in an performant manner, because of the lower level control that is offered compared to a conventional AML. Less has been written about using these libraries for chemical modelling, but CasADi has been shown to be useful for dynamic optimization of OpenModelica Models [@SHITAHUN2013446] and has been used for this in JModelica.org [@magnusson2015dynamic].

Either AMLs or Numerical Computing methods could be used to model physical behavior in a Digital Twin. AMLs provide a very expressive interface that could work for a large number of cases, however to solve specific problems in a performant manner, the fine-grained control of a numerical computing library may be beneficial. In these cases, specific interfaces have been written to integrate with higher level domain-specific tools such as Modelica. 
The most relevant use case for Process Digital Twins is Model Predictive Control, which will be discussed further in depth.


### Sensor/Data Fusion

Sensor fusion combines data from multiple sensors to produce a more accurate representation of the system. This can reduce noise in the data or provide more information than any single sensor can provide. For example, virtual reality headsets use multiple cameras, accelerometers, and gyroscopes to track the user's head position and orientation.

In a Digital Twin, sensor fusion can be viewed as a means to get accurate data from a process to give to the DT Model. The process measurements avaliable from the sensors may not match the input expected from a design-time model: sensor fusion can be the bridge between the two. Alternatively, the DT itself can be viewed as an advanced form of sensor fusion. Thus the architecture of the two technologies may be similar.

A simple example of sensor fusion is the Kalman filter, which combines past measurement and prediction data to get a more accurate value than either alone. It can be designed to adapt to the variance in the data, and with some adjustment can handle non-linearities quite well too [@welch1995introduction]. The kalman filter resembles a predictor-corrector algorithm, which is quite similar to the way a digital twin would be used.

![Kalman filter predictor-corrector cycle, alternating between predicting the current state ahead in time, and updating the estimate by a measurement at the next time step. Reproduced from [@welch1995introduction]](assets/predictor_corrector.drawio.svg)

Kalman filters provide a powerful example of how to update predictions based on new data, and has been used for sensor fusion in chemical processes. However sensor fusion usually involves incorporating data from distinct sensors. Lines et al. [@lines2020sensor] used multiple forms of optical spectroscopy to better estimate chemical composition of spent nuclear fuel; this was used to control the process to a set composition ratio. The model uses to estimate composition was based on partial least squares, trained on known samples. Partial least squares protects against overfitting, which is important with techniques such as spectroscopy that generate a lot of data. 

These techniques can be combined with adaptive technologies such as the kalman filter to avoid degradation in online operation due to fouling and changes in process conditions [@chen2015soft]. Another common technique for adaptive sensor fusion involes using mean and variance update methodology [@wang2019monitoring], where the mean and variance is updated incrementially.





### Control

A digital twin can be viewed as a form of Model Predictive Control, as it is used to control the physical system by simulating the effects of different controlling actions and choosing the best action to apply.

<!---
look at https://www.do-mpc.com/en/latest/theory_mhe.html and also find some papers.
-->

### 3D Modelling and Simulation

3D models can simulate physical systems, such as Newtonian physics, fluid dynamics, heat transfer, or flow. While some definitions of Digital Twins focus on 3D models as the required "physical representation," digital twins only need the data necessary to represent the key characteristics of the physical system.


### Surrogate Modelling

Surrogate models approximate complex systems to reduce computational costs while maintaining accuracy.

Operator networks are used to model and simulate complex systems with high efficiency.

### Online Learning

In Machine Learning, Online Learning refers to updating a model in real-time as new data comes in, without human input or retraining from scratch. This can encode changes in the state or behavior of the physical system, such as degradation of a battery.

### Multi-Agent Systems

Digital Twins may be designed to work in multi-agent systems, interacting with other digital twins or acting on behalf of the physical system.

### Virtualisation / Emulation

Virtualisation is similar in concept to a Digital Twin. It is the ability to create a piece of digital software that can replicate how a physical system would behave. It is comonly used in computers and networking, as well as in IoT: A virtual computer can run the same code as a physical computer, and a virtual sensor can respond in the same way that a physical sensor can [@khan_virtualisation_wireless]. 

Virtualised systems may not have a single, identifiable real-world counterpart in the same way as Digital Twins. However, the ability to accurately simulate a physical system is essential to Digital Twins, so many concepts and tools can be applied from this field.

For example, a Digital Twin could emulate a physical sensor, PLC system, or SCADA system, sending data to the same visualisation and control tools that the factory uses to monitor the physical system . This would enable operators to view the simulations' response to changes in the same way as they would view the real process's responses [@sixlayer_redelinghuys_2020], and would enable the digital twin to communicate with outside systems through already-standard protocols.



## Digital Twin Development Tools in Chemical Engineering

### Standardisation Efforts


#### Standards for Describing Structure

| Standard | Type | Tools |
| --- | ---- | --- |
| Digital Twins Definition Language (DTDL) [@azure_dtdl]  | IoT, NGSI-LD , Data Aggregation     |  Azure Digital Twins [@nath2021building]   |
| Smart Data Models (SDM) [@smart_data_models] [@fiware_digital_twins] | IoT, NGSI-LD, Manufacturing, Data Aggregation, Smart Cities| FIWAREBox [@fiwarebox], Sara [@sara_purpleblob] |
| JSON-LD, RDF, OWL, SHACL|  | IndustryFusion Digital Twin [@industryfusion_digitaltwin] |
|  DEXPI [@dexpi_summary]  | P&ID, 2D, XML, ISO 15926 | Autodesk, Aveva,
Bentley,  Intergraph, Siemens , Cadmatic [@cadmatic_dexpi], Simantics Apros 6, Balas [@simantics_dexpi]|


DTDL, SDM, and DEXPI are all related to the Resource Description Framework (RDF) format, a method of describing the structure of and exchanging graph data [@candan2001resource]. ISO 15926 standardised an ontology for describing different resources in the process industry, including unit operations and equipment characteristics, both across space and time [@15926].
JSON-LD and NGSI-LD are specific formats to communicate RDF data. However, different specifications built on top of JSON-LD and NGSI-LD may not be compatible, if they start with different ontologies.

Each of these services have a slightly different focus. DTDL focuses more on collecting data from sensors, while DEXPI focuses more on describing the relationship between operations. 
A DT that is able to read or communicate via these formats will have increased interoperability with other systems, potentially speeding up development time through greater reuse of existing models and tools.



#### Standards for Modelling Behaviour

| Standard | Type | Tools |
| --- | ---- | --- |
| Modelica [@modelica_specification] | Equation Oriented, First Principles, Dynamic | OpenModelica, IDEAS [@ideas_modelica] , Modelon, Dymola |
| Pytorch & other ML Platforms | Data Driven Modelling  | Various |
| Functional Mock-up Interface | Co-simulation | Simulink, OpenModelica, Wolfram SystemModeler |
| CAPE-OPEN | Co-simulation | ASPEN, DWSIM, gPROMS, COCO |
|  PYOMO   |  Equation-oriented, First Principles, Optimisation    | IDAES-PSE    |


Modelica is a relatively established standard for modelling physical systems. It is a custom object-oriented textual description format, that is supported by a variety of applications. The relationships between systems can be specified mathematically or externally, to model system dynamics across time. Because of it's standardisation and a variety of open source libraries, including for thermodynamic properties [@multiphase_mixture_media] and various process unit operations [@ideas_modelica]. 

Alternatively, data-driven approaches can be used to model chemical processes, without relying on any first principles model at all [@SAVAGE202073].  This can have problems with generalising outside normal operating conditions, but techniques have been adapted to minimise this issue [@SEVERINSEN20241].
Data Driven models have the benifit of being easy to create, and work well for complex systems where traditional first-principles simulation doesn't work, as long as their accuracy and generalizability is sufficient for the requirements of the application. Any machine learning platform, such as PyTorch, could feasibly be used for this.
However, Data-Driven tools can also be used to augment traditional first-principles simulation [@ceccon2022omlt], so each technique can be used as best suits a given application.

The Functional Mock-up interface is a specificaton and data format that provides a standardised interface for defining a dynamic model [@blochwitz2011functional]. A Functional Mock-up Unit (FMU) can include custom C code internally to represent the behaviour, and a standardised XML interface for linking up with other components. This enables model exchange, and co-simulation using models built in seperate tools. This technology can be used to make digital twins compatible with other systems [@laine2024digital], or to make a digital twin using multiple different simulation platforms. 

CAPE-OPEN is a similar interface for co-simulation between multiple simulation tools [@belaud2003missions], but does not provide a standardised model exchange format. Nonetheless, it is supported by a variety of simulation platforms.

Pyomo is a algebraic modelling language in python. IDAES-PSE is a process simulation tool that is built on top of it. Pyomo uses optimisation methods to solve a flowsheet, rather than traditional sequential solution algorithms, which makes it expressive enough to solve a wide variety of problems. The IDAES Library includes a number of different unit operations and thermodynamic property packages. Because the model is specified in Python code, interoperability with existing systems is much harder, though some research has shown that it is possible to interface Pyomo with Modelica models through Functional Mockup Units [@gohl2024steady].




<!--
TODO: Go through a list of tools and make the table, including
references.
then describe what you mean by each type, and how it is relevant.

Type can include:
Equation Oriented
Steady State
Dynamic
Commercial/Open Source
Process/Unit Operation/Site/Energy
Scheduling/Thermodynamic
Design/Operation
-->


#### Standards for Communication

| Standard | Type | Tools |
| --- | ---- | --- |
| OPC UA    |      |     |


<!--
Type can include:
Data Collection
Control
Data History
Commercial/Open Source
Process/Unit Operation/Site/Energy
Scheduling/Thermodynamic
Design/Operation
-->




<!---
We could break this down into:


Standards and Protocols
For describing behaviour
For describing Process interconnection, layout, etc
For Communication with a physical system  (data collection)
For control
E.g Modelica spec, DEXPI, Chemical Markup Language, TMML, IDEAS library
FMI, OPC UA, ISO 15926, CAPE-OPEN

Look at the bottom part of:
https://chatgpt.com/share/67eb2789-a450-8005-a003-1c80dabe2a58



Modelling  & Simulation tools
- Equation oriented modelling tools?
- DT Platforms?
E.g simantics, idaes, modelica, Balas, etc

-->


[@duan2020development]

[@kugler2021method]

### Equation-Oriented Modelling Tools

| Tool                  | Features                                                      |
| --------------------- | ------------------------------------------------------------- |
| Modelica              | Extensive libraries, integration with platforms like Modelon. |
| Julia ModelingToolkit | Modern, good integration with solvers.                        |
| Python Pyomo          | Flexible, supports various modeling and optimization tasks.   |


[@applications_singh_2022]


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


