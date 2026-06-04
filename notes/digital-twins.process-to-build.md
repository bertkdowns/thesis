---
id: 378sgodzyllwhqkkvh2qli2
title: Process to Build a Digital Twin
desc: ''
updated: 1780539414047
created: 1780379686064
---


![Process to build a Digital Twin, broken down step by step.](assets/dt-building-process.drawio.svg)


## Gathering Sources

Piping and Instrumentation Diagrams can be used to figure out how unit operations connect together. This could potentially be done automatically, but it can be hard to know
the level of detail that you need.

Plant data is required for determining or validating the characteristics of individual unit operations. Machine learning can also be used to model the unit operations directly from the plant data.

Typically, there are some plant design specifications that are constants that also need to be specified. This could include feed conditions, or equipment properties (e.g heat exchanger area).

Depending on the compounds present in the plant, there may already be property models, if not, property data needs to be collected to regress coefficients or a data-driven model for them.

## Building Blocks

Note that these building blocks can be first-principles or data-driven, or a hybrid.

## A Steady State Model

A steady state model forms the base of the digital twin.

## Dynamics/ Optimisation

We show that we can easily convert a steady state model into a more complex model for various use cases.

## Integrating Live Systems


## Next

We need to show what it gives out - what does all this work give you? E.g pinch analysis, openhens, etc, optimisation algorithms, predictive (what if) operator decisions, model predictive control, come from here. This could extend the diagram to the right quite a lot.

# What are the main areas that still need work?

- Converting a P&ID diagram into something that can actually be processed into a steady state model in the Ahuora Platform. This is quite a challenging task.
- Unit models with ML properties (Or partly ML properties)
- Property Models that are partly calculated by Machine Learning methods (e.g auto estimation of activity coefficients based on data, etc)
- Converting from a Steady State model to a dynamic model (defining what dynamic context is required etc, switching from DOF replacement to PID control, etc.)

I've got the live data stuff pretty down now.