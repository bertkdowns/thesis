---
id: 378sgodzyllwhqkkvh2qli2
title: Process to Build a Digital Twin
desc: ''
updated: 1780457153539
created: 1780379686064
---


![Process to build a Digital Twin, broken down step by step.](assets/dt-building-process.drawio.svg)


## Gathering Sources


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