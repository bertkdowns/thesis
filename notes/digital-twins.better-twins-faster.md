---
id: deenrenuhciuh6zo3z7punp
title: Better Digital Twins Faster
desc: ''
updated: 1780533584732
created: 1780532873920
---


# What do we do to make better digital twins, that can be built faster than anyone else?

There are a number of things we do.

## A good user interface for building a steady state model.

This is the Ahuora Digital Twin Platform, with it's drag and drop interface.

It also is the variable replacement methodology that makes dealing with degrees of freedom easier.

## Good diagnostics when you make a mistake in your model.

This includes the diagnostics methods on each unit operation, as well as the interpretations of the diagnostics toolbox results.

May include AI features in the future if they can be proven to be valuable.

## Machine learning for stuff that is missing.


## An easy way to convert a steady state model to a dynamic or online model.

This includes:

- Ability to enable dynamics on unit operations (but still have them work for steady state)
- Ability to account for pipe flows (online mode pseudo dynamics rather than instant recycling, auto add 1d pipe unit ops in proper dynamic models)
- Ability to specify input variables in temperature and pressure etc without having DAE index problems when using petsc for time-stepping 
- Pseudo-dynamic online mode with delayed recycling
- Support for dynamic unit operations in pseudo-dynamic mode



## An easy way to connect this model into a wider system

The tagging features in the Ahuora Platform

Connecting to an Excel workbook? (may not be relevant)

Connecting to a MQTT server