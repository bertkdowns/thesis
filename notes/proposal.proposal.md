---
id: 7ge60f5z9a7qkmgh836qxdv
title: Proposal
desc: 'Project Proposal'
updated: 1745973562024
created: 1743126355038
bibliography:
  - assets/refs.bib
---


# Proposal

## Research Aims and Questions

<!---
The core questions or hypotheses you're trying to answer.
Often phrased clearly and precisely, e.g.:
"To investigate whether..."
"How can X be improved by Y?"
-->

My project aims to answer the following research question:

*How can a standardised framework minimise the work required to create new Process Digital Twins?* 

In software engineering, *Essential Complexity* , or domain logic, is the "complexity inherent in solving the problem that you are trying to solve" [@farley2021modern]. *Accidental Complexity*, or application logic, refers to the "problems we are forced to solve as a side effect of doing something useful with computers". This includes infrastructure, scalibility, security, and data persistence. 

There is a lot of complexity in building a digital twin, including:

- Creating a model of the process 
- Adapting the models to work in an online context, where they can be continuously retrained
- Collecting, aggregating, visualising, and storing heterogeneous data in a cohesive fashion
- Leveraging the digital twin for design optimization, operational decision support, and supervisory control.

My hypothesis is that much of this is accidental complexity; the domain logic is in the model itself. Thus, a standardised Digital Twin framework can implement the application logic, and eliminate the majority of accidental complexity. This will enable those building Process digital twins to focus on their model of the system, the essential complexity of the digital twin.

Machine learning techniques are frequently used to adapt a design-time behavioural model into an online context. There is less research into how they are systematically built and deployed, particularly with regard to dynamic systems. This can be standardised to reduce the accidental complexity in implementing machine learning methods.  

Process digital twins often build on existing frameworks for describing process structure (e.g DEXPI), describing process behaviour (e.g Modelica), and data collection (e.g OPC-UA). Linking these is a major source of accidental complexity. Building a DT framework that encompasses these standards will minimise the work required to create new Process Digital Twins.

Because digital twins include bi-directional communication with the physical sytem, they can be viewed as an advanced control system. Thus, supervisory control systems will provide a good template for how these various technologies can be linked and function together. 


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

I have developed the core steady-state functionality required for the Ahuora Digital Twin platform to become usable in industry. Collaborating with others at Ahuora, I implemented additional property packages in the platform to support milk and humid air, two critical components for modeling New Zealand industries.

In March 2025, I released the Ahuora Digital Twin Platform to industry in a beta phase. I conducted a training day with 30 stakeholders from various sectors representing their companies. The training was well received, and I am now engaging in discussions on how to integrate the Ahuora Platform into their workflows. Throughout this process, I identified additional features and requirements.

Since then, I have expanded the platform with additional functionality for dynamic modeling, paving the way for the Ahuora Digital Twin Platform to be used for prediction and supervisory control.

I also conducted a literature review to survey current technologies, which forms the next chapter of this document. This review highlighted the need for a standardized workflow to construct digital twins and the tools in process engineering that a Digital Twin platform can utilize.

In parallel, I researched radial basis operator networks [@kurz2024radial] and implemented these techniques in the Algebraic Modeling language Pyomo. This work may enable the creation of surrogate models of dynamical systems and their inclusion in a Digital Twin.

### Future Work

Through discussions from the Ahuora Platform day and Project Ahuora's other industry connections, I anticipate starting a case study by late 2025 on modeling a continuous process. This case study will provide feedback to improve the scope and architecture of the Ahuora Digital Twin Platform. I will also use additional case studies from other industrial partners and scenarios that existing PhD students are already studying as part of Project Ahuora.

Development of the ADTP and implementation in case studies will be conducted simultaneously, following the principles of iterative design [@beck2001manifesto]. The case studies will naturally influence the requirements and design of the system, and these changes will be recorded and documented. 

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

Finally, I anticipate that this process will also involve creation of new techniques for Machine Learning and/or Mathematical Modelling. As these techniques form the core of the DT, it is likely that tangential research into those fields will be required to enable the use of emerging techniques in Digital Twins. For example, the current work on operator networks fits into this category.



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
