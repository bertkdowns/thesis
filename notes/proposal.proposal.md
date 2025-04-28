---
id: 7ge60f5z9a7qkmgh836qxdv
title: A Software Engineering Approach to the Design and Application of Digital Twins in Industrial Chemical Processes
desc: 'Literature Review & Project Proposal'
updated: 1745808674606
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



### Current Progress

The initial part of the PhD has involved research and development of the core steady-state functionality required for the Ahuora Digital Twin platform to become usable in industry. In conjunction with others at Ahuora, additional property packages were implemented in the platform to support milk and humid air, two critical components to model New Zealand industries.

In March 2025, the Ahuora Digital Twin Platform was released in to industry in a beta phase. A training day was conducted with 30 stakeholders from a variety of sectors representing their various companies. This was generally well recieved, and is now being followed by discussions on how to implement the Ahuora Platform into their workflows. Additional features and requirements have also been identified throughout this process.

Since then, the platform has been expanded with additional functionality for dynamic modelling, which will pave the way for the Ahuora Digital Twin Platform to be used for prediction and supervisory control.

A literature review has also been conducted to survey the current technologies and comprises the next chapter of this document. This has identified the need for a standardised workflow to construct digital twins, and the tools in process engineering that a Digital Twin platform can utilise.

In parallel, research has been conducted into radial basis operator networks [@kurz2024radial], and these techniques have been implemented in the Algebraic Modelling language Pyomo. This may enable creating surrogate models of dynamical systems, and including them in a Digital Twin. 


### Future Work

Through discussions from the Ahuora Platform day, and Project Ahuora's other industry connections, it is anticipated that a case study can be started by late 2025 on modelling a continuous process. This will provide feedback which will be used to improve the scope and architecture of the Ahuora Digital Twin Platform.  Additional case studies will be used further in the development of the ADTP, from other industrial partners and from the scenarios existing PhD students are already studying as part of Project Ahuora. 

Development of the ADTP and implementation in case studies will be conducted simultaneously, following the principles of iterative design [@beck2001manifesto]. The case sudies will naturally influence the requirements and design of the system, and these changes will be recorded and documented. 

These notes and discussions on requirements changes will become a primary data source for future studies. They will be used to qualitatively discuss:

- The benefits and drawbacks of the Ahuora Digital Twin Platform from a systems design perspective
- How the Ahuora Digital Twin Platform provides structure for a Process Digital Twin
- How the Ahuora Digital Twin Platform can be used to build a Process Digital Twin

These topics are expected to be the major component of my thesis.

In order for a framework such as the Ahuora Digital Twin Platform to be truly useful in industry, a business model or long-term support plan is required. Towards the end of my PhD, I anticipate that more time will be taken up creating a business model to ensure that my research is able to provide the most value to the industry. As an open source product, this is also novel in the field of Process Digital Twins. 

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
What are the ## Current Progress
<!---
What you've done: experiments, models, code, papers, reviews, etc.
Can include early results or pilot studies.
-->

ultiple case studies will be avaliable in the near future. If some case studies are unable to be completed, it is likely that a replacement case study will be avaliable, however, this could cause some delays. In absence of real-world case studies, work could still take place through collaboration with other PhD student's existing case studies, and emulation of factory control systems. 

There could be technological concerns with the use of the Ahuora Digital Twin platform, if development of the required features is taking longer than expected. This can be mitigated through incremental development and managing scope. External tools could also be used if the Ahuora Digital Twin Platform is not yet at the level required to build some specific feature needed for the Digital Twin. Similar techniques can be used to manage other time constraints.

Depending on the case study, confidentiality may be required with regard to company data. This will be handled in accordance with the University policy's procedures and guidelines. Likewise, any ethical issues will be dealt with in accordance with University policy. 




\newpage
