---
id: 7ge60f5z9a7qkmgh836qxdv
title: A Software Engineering Approach to the Design and Application of Digital Twins in Industrial Chemical Processes
desc: 'Literature Review & Project Proposal'
updated: 1745462908820
created: 1743126355038
bibliography:
  - assets/refs.bib
---

<!---
% Segue from lit review, talking about what gaps and potential new research ideas there are.


% talk about the need for a generalised method of building digital twins, and how this could be achieved.


% From the literature, start to identify some of the key characteristics that will aid in the development of a generalised method of building digital twins.


% Discuss research questions, methodology for development - pull from the proposal i've already written.

- Wrap up by explaining how you can tell if the project is successful.

-->

# Proposal

## Research Aims and Questions

<!---
The core questions or hypotheses you're trying to answer.
Often phrased clearly and precisely, e.g.:
"To investigate whether..."
"How can X be improved by Y?"
-->

This project aims to answer the following research questions:

*How do Digital Twins evolve traditional modelling and control systems in the Chemical and Process sector?* This involves understanding how existing frameworks such as DEXPI, Modelica, or OPC-UA can be augmented or replaced by Digital Twin systems. 


*How do emerging Machine Learning techniques affect the cost and complexity of developing Digital Twins for legacy industrial systems?* Data-driven techniques have been rigourously analysed for effectiveness in modelling and generalisation. There is less research into how they are systematically built and deployed, particularly with regard to dynamic systems. This involves understanding how ML training and usage could be standardised to work in Process Digital Twins. 


## Methodology
<!--
How will you answer your research questions?
Details of methods, experiments, case studies, simulations, or theoretical frameworks.
Tools, data sources, analysis techniques.
-->

To answer these questions, two main methods will be used:


**1. Implementation of the Ahuora Digital Twin in industrial case studies**

The Ahuora Digital Twin Platform will be used to model a variety of industry case studies, in collaboration with New Zealand industries and other PhD case studies. As the Digital Twin Platform is still relatively new, inevitably feedback and requirements from these case studies will necessitate development of new features and refinement of workflows.

These changes in requirements form the key data points of this research. They provide feedback into how standards, frameworks, and technologies affect the development pace, quality, functionality, and reliability of a Digital Twin.

Through application of software engineering practices, the architecture of the platform will gradually adjust to better the problem domain. This principle of incremental change to "grow" an architecture is widely documented in the software industry [@foote1997big].


**2. Theoretical analysis of system properties**

As the software project gradually evolves over time through ongoing use and refinement, patterns and structures will be identified. These patterns in the software architecture can provide a different lens into the problem space of designing a Digital Twin. The effect of these patterns can be analysed through logical reasoning, by asking the question *"If this property was to be taken to its logical extreme, and strictly enforced across the Digital Twin, what consequences - both positive and negative - would emerge?"* This enables a deeper understanding of the trade-offs in adopting an architectural principle.

For instance, if a technique for modularity emerges as a dominant pattern, one might ask: "What would a fully modular Digital Twin look like?" 
This could lead to benefits such as improved testability, parallel development, and plug-and-play component integration. However, it might also introduce challenges around coordination, interface design, and performance due to increased abstraction boundaries.

Ultimately, these emergent patterns become not just a byproduct of the development process, but can be applied together to provide a generalised framework for building Digital Twins.

<!---
In the appendix, include some potential characteristics, and some potential case studies
Potential characteristics avaliable in phd.potential_characteristics.md
-->

## Timeline

![Estimated PhD Timeline](assets/timeline.drawio.svg)


### Research Plan

<!---
Research plan
A Gantt chart or milestones (e.g. months 6–12: prototype; 12–18: case study).
Break it down to show progress made and what’s coming.
-->

The initial part of the PhD has involved research and development of the core steady-state functionality required for the Ahuora Digital Twin platform to become usable in industry. This included the March 2025 Ahuora day, where the Ahuora Digital Twin Platform was released to industry in a beta phase, an a training day was conducted. Since then, the platform has been expanded with additional functionality for dynamic modelling, which will pave the way for the Ahuora Digital Twin Platform to be used for prediction and supervisory control.

Through discussions from the Ahuora Platform day, and Project Ahuora's other industry connections, it is anticipated that a case study can be started by late 2025 on modelling a continuous process. This will provide feedback which will be used to improve the scope and architecture of the Ahuora Digital Twin Platform.  Additional case studies will be used further in the development of the ADTP, from other industrial partners and from the scenarios existing PhD students are already studying as part of Project Ahuora. 

Development of the ADTP and implementation in case studies will be conducted simultaneously, following the principles of iterative design [@beck2001manifesto]. Notes and discussions on requirements changes and development will become the primary resource used for a qualitative discussion on the benefits and drawbacks of the Ahuora Digital Twin Platform from a systems design perspective. The case studies themselves can be analysed to provide a standardised model for a Digital Twin, as well as a standardised methodology to build a Digital Twin via the Ahuora Digital Twin Platform.





### Current Progress
<!---
What you've done: experiments, models, code, papers, reviews, etc.
Can include early results or pilot studies.
-->

## Expected Contributions

<!--
What will your research contribute to knowledge, practice, or theory?
How will it make a difference?
-->

![Anticipated PhD Outcomes](assets/outcomes.drawio.svg)

It is expected that this PhD will contribute both to academic theory on how to build Digital Twins, and the way Digital Twins are used in New Zealand Industries. 

This PhD will include substantial work on the Ahuora Digital Twin Platform, which it is intended will be avaliable as a partially commercial, partially open source product for the forseeable future. This will provide a medium to transfrom many techniques currently only used in academia into general industry practices.

Existing literature already outlines a number of different frameworks to view a Digital Twin. However, through participation in multiple case studies, these existing models can be reviewed and updated, and specialised to the domain of Chemical and Process Engineering. 

Through the case studies, a generalised methodology to build a DT of a chemical process can be constructed. This methodology will be shown in the context of the Ahuora Digital Twin Platform. It will be both a novel piece of research into DT application, and a guide to industry on how to create a DT system.

Finally, it is anticipated that this process will also involve creation of new techniques for Machine Learning and/or Mathematical Modelling. As these techniques form the core of the DT, it is likely that tangential research into those fields will be required to enable the use of emerging techniques in Digital Twins. For example, the current work on operator networks fits into this category.



## Challenges and Risk Management

<!---
What are the uncertainties or limitations?
How will you address or mitigate them?
Ethical considerations
-->

The most important requirement for this project is the accessibility of case studies; depending on economic, confidentiality, or other business priorities, real-world case studies may or may not be avaliable. Fortunately, discussions have already started and it is likely that multiple case studies will be avaliable in the near future. If some case studies are unable to be completed, it is likely that a replacement case study will be avaliable, however, this could cause some delays. In absence of real-world case studies, work could still take place through collaboration with other PhD student's existing case studies, and emulation of factory control systems. 

There could be technological concerns with the use of the Ahuora Digital Twin platform, if development of the required features is taking longer than expected. This can be mitigated through incremental development and managing scope. External tools could also be used if the Ahuora Digital Twin Platform is not yet at the level required to build some specific feature needed for the Digital Twin. Similar techniques can be used to manage other time constraints.

Depending on the case study, confidentiality may be required with regard to company data. This will be handled in accordance with the University policy's procedures and guidelines. Likewise, any ethical issues will be dealt with in accordance with University policy. 




# Conclusion