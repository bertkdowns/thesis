---
id: 5nanq7k2v18kt5y3byvw0pa
title: Geothermal
desc: ''
updated: 1753762207840
created: 1753326165517
---


![Geothermal flowsheet](assets/geothermal.jpg)


Image from [Bryan's paper](https://www.sciencedirect.com/science/article/pii/S0098135425002157)


The data source for geothermal has a bunch of different readings:

- AH - Air Humidity (0-100%) AKA Relative Humidity
- AT - Air Temperature (°C)
- BCOCF - Brine-Condensate Outlet Combined flow 
- CIT - Condenser Inlet Temperature
- COT - Condenser Outlet Temperature
- CP - Condenser Pressure (Outlet) bar
- KW - Parasitic load (kW)
- MW - Turbine power 
- NCGOT - Non-condensable gas outlet temperature ( steam Outlet of vaporiser, decent approximation of inlet of preheater)
- PHBOT - Preheater brine outlet temperature
- PHMFIT - Preheater motive fluid inlet temperature
- PHMFOT - Preheater motive fluid outlet temperature
- SD - Pump speed
- SIF - Steam inlet flow
- SIT - Steam inlet temperature
- TOT - Turbine outlet temperature
- VMFL - vaporizer motive fluid level
- VMFOT - vaporizer motive fluid outlet temperature
- VP - vaporizer pressure


This gives us enough to model and estimate most of the process. The only weird thing is, we don't have exact data on the fluid that is used, so there isn't really a property package. I'm going to see how far I can get without information about the fluid, but I think having data on the fluid would greatly augment my ability to model this.

Since there's so much data, we can do a lot with machine learning to model the relationships between the variables. E.g if you know the inlet temperatures and flow rates of the heat exchanger, you should be able to define the outlet temperatures. Of course, the flow rate won't change if there's no dynamics.

See also: [Github Repository](github.com/bertkdowns/geothermal)