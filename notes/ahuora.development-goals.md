---
id: ic5nyf8x64zgtf3k9kw273u
title: Development Goals
desc: ''
updated: 1747782928929
created: 1743117525011
---

# Surrogate Modelling

Support OMLT import export
 - train a simple heat exchanger surrogate model - perhapse using a resnet like in Bryan Li's paper - https://www.sciencedirect.com/science/article/pii/S0098135425002157?via%3Dihub
 - Export the model as onnx
 - learn how to use OMLT to import a machine learning model into the platform
 - Make it work with the Machine Learning implementation that we have, as an alternative to uploading a pysmo model.

Surrogate model a property package, using OMLT or pysmo. Ben's got some results for that already. Then try put it in the platform.

# 1D unit operations

- Add a pipe to the platform. That's a simple case, and we don't need to show any of the complexity on the frontend, just the length and number of segments is sufficient. That should allow us to model first-order-plus-time-delay systems.
- Add a heat exchanger to the platform that is also 1d. Same as the pipe, don't bother about modelling the internals of it yet.
- Figure out a way to visualise the internals of the 1d unit operations

# Reactions

- Create a reaction property package and implement it, along with a reactor, in the platform

# Dynamics

- Test to see if the integration block allows for model predictive control or PID tuning.

# Multiple Time Steps

- View the results of all the time steps somehow.

# Live Data

- Get PyTEP (Tennessee eastman Process simulator) running, and make it a live data source. Connect this up to a model of the system in the Ahuora Platform.

# Refactoring

- use References in pyomo/idaes to make it so that everything can be ordered in the same way (Time, if present, then everything else...)
This might involve more custom unit operations, but it should mean that we no longer have to have fancy logic to handle if a block is indexed by time or not. If theres an alternative way that's also good, maybe it could be abstracted some other way too.


# Control

Find a good control problem to do.

# Design time/retrofit analysis

Model that factory that's nearby.

# Ahuora Development Goals

Property Packages
- ~~Generalise to support mass streams, energy streams, without compounds~~
- Start looking at reaction property packages

Optimisation
- ~~How to write an objective/objective block (similar to a control block?)~~

Dynamics
- ~~Make a ui prototype to support dynamics~~
- ~~Plan how to store all the dynamic data~~
- ~~How to switch to/from dynamic mode as seamlessly as possible~~

Multi-Steady-state
- Better visualisation of results
- ~~Store in the backend (seperate to history?)~~
- Modify history to be specifically for live streaming?

Surrogate Modelling
- ~~Use Shean's UI and develop it in the platform~~
- ~~Find a way to handle data processing:~~
    - ~~Storage in the platform~~
    - ~~training a model~~
    - ~~running a surrogate model~~
- ~~Should surrogate models be seperate to a flowsheet? can you import them from other flowsheets? (We now have a way to export them)~~

UI Tweaks
- ~~Refactor the frontend (minimize stuff)~~
- undo/redo
- ~~search bar - create a custom library~~
- auto close/ auto open stuff

P-Graph
- Multi-solving
- Storing/displaying results from different pgraph solves
