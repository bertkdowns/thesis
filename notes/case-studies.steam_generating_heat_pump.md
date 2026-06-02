---
id: 2ic55zb4xauos9xnvjtny5l
title: Steam_generating_heat_pump
desc: ''
updated: 1780370904089
created: 1780368695399
---

> For more information, see [https://github.com/bertkdowns/sghp-twin](https://github.com/bertkdowns/sghp-twin)

# Overview

The University of Waikato is building a small scale steam generating heat pump, to get experience in the process of getting and building one and to pave the way for industry to do the same in the future.

As part of this, we have got a PLC that controls the steam generating heat pump. I have set up the PLC to be able to control the heat pump, but we didn't have the heat pump on hand as it was still being built. An excelent way to test the PLC control system and interface then, is to virtualise the heat pump.

This uses a Digital Twin to model the process. The PLC decides what valve opening fraction, pump speed, etc to set, and this is then sent over MQTT to the computer running the Digital Twin. The digital twin calculates what the pressures and temperatures etc would be in response, and then sends this data back to the PLC. The PLC can then make controlling actions in response to this.

## Working with PLCs

PLCS make building PID controllers pretty easy, and linking up lots of different inputs etc. There isn't too much of a learning curve to figure it out and start building interfaces etc. It helps that pretty much everything's a global variable.

However working with ladder is painful for complex logic. I think PLCs are definitely best for input mapping - getting the physical inputs to a semantic tag and range that makes sense (e.g voltage to temperature conversion, etc). It's best to then get them to talk over MQTT to offload more complex logic. It's nice that they support a range of protocols (at least unilogic's one does) though if they didn't I'd probably standardise on MQTT anyway and then have some sort of bridge service to convert to/from.

## A true Digital Twin

On the platform we can set tags for all the variables that are calculated from the platform. These can be sent to the PLC with the VIRTUAL
prefix when we want to virtualise the process.

The PLC can send back signals for the controlling actions it takes (new amount of heat duty, pump speed, mechanical work), from the PID control algorithms. These can then be used as inputs to the simulation (we can automatically detect which things are inputs to the simulation and have tags using the tag mappings).

This actually works pretty well. One problem is when the system is in an unstable state, the platform tries to simulate to steady state. E.g if you have a recycle loop and your heater is on too high, in the real world it would gradually get hotter and hotter. Our platform is simulating a steady state model, and so in steady state temperature is infinite (it would just spiral higher and higher forever) and infeasible. 

This could be remedied in a couple of ways:
- Having some sort of buffer, e.g generic heat loss that pulls variables back down to some maximum
- Specifying different properties (e.g if you specify temperatures and pressures you don't get this problem. However, then you can't use the platform as a "virtual plant emulator")
- Doing one step of a dynamic simulation instead of a steady state simulation (then holdup can actually model it). This will involve adding some holdup/tanks between unit ops, or at least somewhere in the unstable recycles. It'll also take some work to make the platform robust enough to do this.
- Manually do dynamic simulation: Break the recycle loop at some point, and use the previous value (or a percentage of the previous values) output as the input for the next solve. Solve each at steady state.

Breaking the recycle loop at some point works really well for this. For one, it simplifies the model a lot. Two, it better represents what happens in the real world, as changes take time to propogate through the system. Three, it's much simpler than doing a dynamic simulation, as we are still solving a steady state model each time. 

We could even go a step further and add additional points to introduce process lag between unit operations. One caveat is that this doesn't work if you are controlling something further down in the process by DOF replacement. However it works well if you have no DOF replacement in your model, or in this case the DOF replacement could be switched out with a PID controller (TODO: investigate this further.)

The hard part to figure out is how quickly should values be recycled through? This is pretty much a question of defining transfer functions. The advantage of this type of approach is that we could specify time delay in the transfer functions too (by having an array/buffer to store intermediate values), something that can't be approximated well without a 1d buffer (e.g a pipe) in a normal dynamics/DAE simulation. 

Using pyomo and IDAES in this way is more similar to how things would work with modelica and maybe CAPE-OPEN, rather than normal idaes models. However the advantage is you can use the same models for these things and for your steady state model, and use this type of simulation if you want fast simulation online, or switch to a DAE model if you want more robust dynamic optimisation. It would be good to look at how we can switch between the different modes easily - particuarly for this case, variable replacement may work really good for automating converting to/from PID controllers. 


## Some of the problems this has identified:

- Zero flow: This means that everything fails to solve, because if you try do do 1 kW of mechanical work on zero flow you get to infinite pressure. Could probably add a "zero flow" warning and some slack variables to properties that are affected by flow so that it stops caring about them when flow is zero.
- Heat exchangers failing: In theory, U and A heat exchangers should always solve. However, it seems sometimes they fail and calculate that we don't have enough energy to reach an outlet temperature. This could be when we go through the vapor-liquid equilibrium breaking the delta_temperature LMTD or whatever calculation method?
- Valves are often showing that there is a problem between inlet and outlet temperature. However, it is often very small differences between inlet and outlet temperature, so it usually seems to be a numerical precision area as enthalpy is the state var. We should ignore those very small deviations as it clutters up the logs.

- Maybe? Ipopt is starting from start, setting warm_start = true might help once it's solved once (could be overshooting a bit.)

The "recycle whatever you get" strategy doesn't work when you get an infeasible solve as then it recycles and fixes garbage variables. So you should only recycle when you get "optimal solution found".


Next steps: Clean up logs to notify of zero flow, ignore valve issues. 

I would like to look at the slack variable idea to make it more reliable. I would like to try the hierarchical modelling as an alternative fallback strategy if the main model doesn't solve, but that will be a lot of work. 

I also need to set sensible defaults for the p&ID variables and the other outputs so that the model doesn't immediately fail when I turn on the PLC. (right now the pumps are off so the model immediately breaks.)