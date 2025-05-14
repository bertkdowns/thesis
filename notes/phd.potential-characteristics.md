---
id: 1ziqbhf6cqyt0b6urtfd42n
title: Potential Characteristics
desc: ''
updated: 1744322007041
created: 1744262162102
---

# Potential Characteristics of a Digital Twin Software

Software design has a lot of different characteristics, and choosing characteristics that we want our software to have will define other attributes that it might have as well. A good example of this is erlang - becasue it limited itself to message passing and no memory sharing, it could be distributed and scalable.


**What fundamental laws/principles should we define to ease the building of digital twins?**

Some potential characteristics include:


### Stateless

Stateless model or stateless connections to the DT? enables replication/parallel solving?


### Always mathematically representable

Can we always convert it into a mathematical problem that can be solved in IPOPT? is that desirable? can we always solve the whole thing as one problem? do we keep the DOF as always 0?

### Standardised external function calling interface

Is supporting Functional Mockup units or something timilar going to be a core attribute that enables cross-reusability?


### Idempotent

Similar to stateless, do we make DTs have an event log and each event can only happen once or something? could that increase reliability?

### Always accessible by the same set of tools

Similar to how spreadsheets "everything is a cell/number" and linux "everything is a file", can we have "everything is a simulationobject, with components" (entity-component view similar to unity).

How can we make these tools work with different modes of data, including images, text, live data, historical data, etc?

### enforced edge translation/data schema

Do we make a plugin system, and everything must work with that plugin system?

Do we enforce one api to work with data, and everything uses that?

How do we handle having so many different types of tools that we want to work with?

Do we enforce erlang-style message passing/communication? can that be a way to make multiple digital twins talk to each other?

### Modelica?

Do we just always support modelica - it's pretty powerful! what does it not do? What do we add ? (maybe optimisation, and history)


### Multi-level modelling as a first class citizen?

Do we make it possible to hot-swap a effectiveness HX with a 0d heat exchanger, 1d heat exchanger, 2d HX, etc, without having to reformulate everything? will that break everything we've done or will the model still solve?

How do we handle these different types?

What about energy, electricity, piping diagrams, and other things? do we include 3d models? what's the point of doing that?


### How do we make it testable?

We test the code - why don't we have tests for solvability/feasibility? Is test driven development appropriate for building digital twins?
or checking performance as you build? what would that even look like?

### Versioning

How do we version through time? What if the factory changes and we have new sensors, new equipment, etc? What if we decide we want to make a more accurate model, when is the historical data similar enough or when is it really modelling a different system as things have been retrofitted since then? 

How do we version in development? can people work together to build a factory in realtime/git like collaboration? 

What does Git look like for digital twins? can we put the world in git?

### Isolation

Do we specify the structure of the model and the data/parameters of the model differently, or are they inherently the same thing?

Do we store this data together or seperate?

### Layering?

Similar to multi-level modelling, can we layer a more complex model on top of a simpler model, or layer changes so a different layer can represent the factory at a different point in time?

"Individual laywers tend to be about things that change at similar rates. Things that change at different rates diverge.
Differential rates of change encourage layers to emerge." - https://s3.amazonaws.com/systemsandpapers/papers/bigballofmud.pdf

### Surrogates?

Is it better to just create a black box/surrogate model for any given problem, rather than using first principles?

### Dynamics and Steady state simultaneously

How do we do both in a way they don't get in the way with each other? what about other forms of analysis? What ones the "real" factory? Does live data make it more complicated?


### Virtualisation

Should we support OPC UA and other things like that, and emulate sensor readings so control systems can talk to the DT and think its a real plant? Is that helpful, and if so what for? maybe training, seeing how the real plant would respond in sped-up time

