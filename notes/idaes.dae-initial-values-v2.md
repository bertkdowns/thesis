---
id: h1dca8lr149smfqun1g4faf
title: Dae Initial Values V2
desc: ''
updated: 1780883641157
created: 1780632483793
---

In a steady state EOM, there are basically only variables and parameters (or alternatively, unfixed variables and fixed variables). But when we start modelling dynamic systems, there
are more special variables that we have to worry about. 

DAE stands for Differential-Algebraic Equations, and you kind of want to keep them seperate. If you start mixing them, you end up with a high-index DAE system.

The Differential part of the system is a variable defined across time `x(t)`, where we also care about its derivative across time as well (`dx/dt` or `x'(t)`). Typically, we fix the initial time point of `x(0)` (pretty much the "plus c") and everything else is calculated. However, in some cases you can fix `x'(0)` e.g fixing it to zero for a steady state assumption, in which case it will calculate the appropriate value of `x(0)` to make it steady state.

In a DAE system, the algebraic equations are defined at every time point. They don't reference equations at any other time point, only at that instantaneous time. However, the equations can include the value for the state `x` at that time point `x(t)`, or the value for the derivative `x'(t)`. This is how it links across time.

In the same way as a steady state model, the algebraic component can have fixed variables and unfixed variables. The fixed variables are fixed for each time based on some funciton, i,e `u(t)` is defined a value for every time `t`. In a dynamic context, the fixed variables may be called either "inputs" or the "forcing function", or may be split into the "control function" or "disturbance". All of these are part of the fixed variables. The unfixed variables `z(t)` are calculated from the fixed variables `u(t)`, the differential variables `x(t)` and derivative `x'()`, all at that instantaneous time. The derivative `x'()` is typically also calculated from the fixed variables `u(t)` too, which can then be used to calculate `x(t+1)` and the process can be repeated. 

| Variable type              |   Interior time points |         Initial time point |
| -------------------------- | ---------------------: | -------------------------: |
| Differential state `x(t)`  |                unfixed | fixed or constrained by IC |
| Derivative `dx/dt` or `x'(t)` |             unfixed |            usually unfixed |
| Algebraic variable `z(t)`  |                unfixed |                    unfixed |
| Algebraic parameter `u(t)` |                  fixed |                      fixed |

One problem is that the differential state `x(t)` is often hard to measure. Some things, such as tank level, can be easily measured. However some things are much harder, such as the amount of thermal energy in the tank. This makes setting the initial condition `x(0)` quite hard. 

One solution is to not specify these conditions directly, but instead specify the derivatives to be zero, `x'(0) = 0`. This starts everything at steady state. However, sometimes we may want to fix the initial tank level, which means this can't be used. 

In [idaes.dae-initial-values] I suggested adding constraints to calculate the initial energy from temperature and pressure. However, this may also make the DAE high index. Another solution is to solve a steady-state subproblem before solving the dynamic problem. 


