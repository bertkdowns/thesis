---
id: z8u6vtfxw87oi5axdiqbwee
title: Optimisation Optimisation Proposal
desc: ''
updated: 1765245027172
created: 1764792564763
---

Right now the scenarios panel is quite cluttered with a bunch of random things, including:

- Optimisation
- Steady/MSS/Dynamics variables
- Enable/disable initialisation
- Enable/disable rating mode
- choosing a solver

It basically has become a dumping ground for features that don't fit cleanly in any other place.

We need to think about how to improve it. Do we even need scenarios? are there better places or implementations of these features?

## Optimisation

Right now, setting up optimisation involves choosing a bunch of options from a lot of different input fields - which property you want to minimise/maximise, what degrees of freedom there are, and so forth. How could we make it more connected to the main flowsheet? Some ideas include:

- Showing additional context on the flowsheet in optimisation view, so the units with a degree of freedom are in a different color, or the free properties are shown on the flowsheet below the unit operation
- Having a unit operation/property picker tool if you want to pick a property for a field
- possibly moving solver choice as a generic setting rather than scenario-specific, and the same for initialisation

