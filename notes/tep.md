---
id: s68wvssdc9k5vwhgzqdz18x
title: Tennessee Eastman Process
desc: ''
updated: 1752707131509
created: 1752645873149
---


![Tennessee Esatman Process PID diagram. Sourced from [Andersen et al.](https://onlinelibrary.wiley.com/doi/full/10.1002/qre.2975)](assets/tep.jpg)

The Tennessee Eastman process is a simulated plant that has some good dynamics. This makes it a good test case for integrating live data with the ahuora platform.


## Key variables

[This article](https://onlinelibrary.wiley.com/doi/full/10.1002/qre.2975#qre2975-sec-0020-title) explains the basic layout of the tennessee eastman process. Section 2 is the most relevant.

There are a few different key variables:

- saftey variables, which must be kept between certain bounds for safe operation.
- economic variables - things that we can min/max to reduce costs and increase profits
- control variables - things that we can normally manipulate to balance the process.
- other process varables - things that might help us understand what is going on, but aren't directly related to saftey or profitability or control.

The TEP has pre-programmed routines for disturbances, including in temperature, flow rate, composition, sticking valves, etc. These can be activated in the simulator to see the effect the change has on the process. These are labelled IDV1-28.


## Running it in python

There is a library called [PyTEP](https://github.com/ccreinartz11/pytep?tab=readme-ov-file) fom ([this paper](https://www.sciencedirect.com/science/article/pii/S2352711022000449#b26)) that makes it possible to run the TEP in python in an interactive manner.


see also [github.com/bertkdowns/tep-control](github.com/bertkdowns/tep-control)

PyTep includes a control system for the plant, but allows you to model the control system setpoints, and enable or disable the preset faults. As you can also pause and continue the simulation, you could do a model predictive supervisory control method on it, similar to what you might do in a real plant.s



