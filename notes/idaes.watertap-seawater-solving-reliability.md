---
id: yecf3xylh4lfnech49qcorh
title: Watertap Seawater Solving Reliability
desc: ''
updated: 1772138377565
created: 1772136503990
---


# A case study on failed initialisation points



We^[Kai, Bert] were trying to solve a heater using watertap, with our standard property package extensions so that you don't have to fix the state variables. In this case, we had fixed flow_mol when the state variable was flow_mass_phase. The inlet Temperature and Pressure were also fully specified.

We found that if we set the heat duty of the heater too high, it would fail to solve. However, if we constrained the outlet temperature, it could solve at higher heat duties.

So we graphed the relationship between heat duty and outlet temperature, to get a graph that looked like this:


![Graph of relationship between heat duty and outlet temperature](assets/heat-duty-watertap-graph.drawio.svg)

Interestingly, after a certain heat duty the temperature goes down - this would be because the temperature enthalpy correlation they are using is a quadratic equation.

In the region that we were solving at, the heat duty was not that high though, so why was the model failing to solve?

The answer is apparent when you consider the effect of flow on heat duty. IF we initialise with a much smaller flow, it takes a lot less heat to heat up the water. this has an effect of squeezing the curve:

![Showing the effect of flow on the relationship between heat duty and outlet temperature](assets/heat-duty-watertap-flow.drawio.svg)

This could result in the temperature that is calculated being on the downwards slope of the t-h curve, rather than the upwards slope.  If the flow rate is low enough, it could even be zero or negative, resulting in an infeasible solution. And it's hard for IPOPT to get from that side of the curve over to the other side of the curve, because that flips the relationship between temperature and enthalpy: On the right side of the curve it tries to increase temperature to decrease heat duty, when it should be decreasing temperature to decrease heat duty.


# Some of the solutions:

- Get a better guess of flow mol: Increasing the guessed flow rate to be much higher means it initialises on the right side of the curve. However, you might have the opposite problem with e.g heat exchangers or other unit ops.
- Change the correlation to something that doesn't go down: A cubic wouldn't have the same problem as it always goes up.
- Start with a simpler correlation and then gradually switch to a more complex model: This is my "phasing between model complexity levels" idea in [[phd.2026-february-update]]. 
- Start by solving with a heat duty of zero: This means the model solves from the "left" of the graph rightwards, meaning it is always on the side with the correct relationship between temperature and enthalpy.




