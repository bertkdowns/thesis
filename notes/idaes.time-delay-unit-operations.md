---
id: fo508ysd2t812cs3rt3yiej
title: Time Delay Unit Operations
desc: ''
updated: 1777353769762
created: 1777352163800
---

Adding time delay is an alternative way of creating dynamic models.

an example of time delay in pyomo is [here](https://www.osti.gov/servlets/purl/1890623) in Listing 6. This could be implemented, however, the amount of time delay is not a variable here (i.e it's fixed.) Still could be helpful to implement though, but you gotta figure out a good way to specify the initial values.

Traditionally, to address time delay you would just use a 1d unit operation in pyomo. However, it would be interesting to see if you can model time delay directly, by referencing previous time steps. Though this would create a very dense problem with all timesteps coupled very closely together through a lot of constraints: instead of just depending on the previous timestep, you depend on all previous timesteps (almost like the attention mechanism in LLMs) 1d is more like a state space model. Potentially there is some kind of hybrid that makes things simpler.