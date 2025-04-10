---
id: 7ge60f5z9a7qkmgh836qxdv
title: A Software Engineering Approach to the Design and Application of Digital Twins in Industrial Chemical Processes
desc: 'Literature Review & Project Proposal'
updated: 1744270997921
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

This project aims to andwer the following research questions:

*How do existing standards and frameworks (e.g., DEXPI, Modelica, OPC UA) influence the interoperability and scalability of Digital Twin systems in chemical and process engineering?*

*What role do emerging technologies, such as online learning and surrogate modeling, play in reducing the cost and complexity of developing Digital Twins for legacy industrial systems?*


*How do Digital Twins evolve traditional control systems, and what are the implications for feedback loop design in autonomous industrial processes?*


## Methodology
<!--
How will you answer your research questions?
Details of methods, experiments, case studies, simulations, or theoretical frameworks.
Tools, data sources, analysis techniques.
-->

To answer these questions, two main methods will be used:


### Implementation of the Ahuora Digital Twin in industrial case studies

The Ahuora Digital Twin Platform will be used to model a variety of industry case studies, in collaboration with New Zealand industries and other PhD case studies. As the Digital Twin Platform is still relatively new, inevitably feedback and requirements from these case studies will necessitate development of new features and refinement of workflows.

These changes in requirements form the key data points of this research, as this gives feedback into what standards, frameworks, and technologies are most influential, provide the most value, or cause the most problems when designing and deploying a Digital Twin.


### Theoretical analysis of system properties

As the software project gradually evolves over time through ongoing use and refinement, patterns and structures will be identified. These patterns in the software architecture can provide a different lens into the problem space of designing a Digital Twin. The effect of these patterns can be analysed through logical reasoning, by asking the question *"If this property was to be taken to its logical extreme, and strictly enforced across the Digital Twin, what consequences - both positive and negative - would emerge?"* This enables a deeper understanding of the trade-offs in adopting an architectural principle.

For instance, if modularity emerges as a dominant pattern, one might ask: "What would a fully modular Digital Twin look like?" This could lead to benefits such as improved testability, parallel development, and plug-and-play component integration. However, it might also introduce challenges around coordination, interface design, and performance due to increased abstraction boundaries.

Ultimately, these emergent patterns become not just a byproduct of the development process, but can be applied together to provide a generalised framework for building Digital Twins.

## Timeline

![Estimated PhD Timeline](assets/timeline.drawio.svg)


### Research Plan

<!---
Research plan
A Gantt chart or milestones (e.g. months 6–12: prototype; 12–18: case study).
Break it down to show progress made and what’s coming.
-->


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

## Challenges and Risk Management

<!---
What are the uncertainties or limitations?
How will you address or mitigate them?
Ethical considerations
-->

# Conclusion