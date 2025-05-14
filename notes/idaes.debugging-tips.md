---
id: a0jgvj1rsffix8gbht7t967
title: Debugging Tips
desc: ''
updated: 1743117824630
created: 1743117524982
---

# First Step: verify degrees of freedom

```
from idaes.core.util.model_statistics import degrees_of_freedom
degrees_of_freedom(m)
```

# Try the model diagnostics toolbox 

```
from idaes.core.util import DiagnosticsToolbox
dt = DiagnosticsToolbox(m)
dt.report_structural_issues()
```

The idaes model diagnostics toolbox is pretty good, but requires a bit more understanding on how everything works. You can probably figure it out as you go though.

[Diagnostics Toolbox](https://idaes-examples.readthedocs.io/en/2.3.0/docs/diagnostics/diagnostics_toolbox_doc.html) 

# Specific types of solver results

## Too few degrees of freedom

That literally means it's over defined, impossible to solve. You gotta remove some constraints.

If there's too many degrees of freedom, the model will probably solve, but you have to remember that it'll have one solution in a space of possible solutions. 

## Initialisation failed

Initialisation happens before the actual solve. Initialisation generally proceeds from inlet-to-outlet, sequentially through the unit operations. If an inlet condition is not known, e.g you don't know the inlet enth_mol, idaes will provide an initial guess of that variable. If you have a better initial guess, you should provide that, as it provides better initialisation values for everything downstream.

We're also trying to add extra constraints to allow you to specify more chemical properties. That involves adding additional initialissation techniques for those other constraints too.

## `problem may be infeasible`

This could be because you have provided actual infeasible chemical properties. Double check that chemical is at a reasonable state at that temperature/enthalpy/pressure. Double check that there's enough mcp in that heat flow stream that the heat exchanger can reasonably heat/cool the other stream to that temperature, etc

It also could be because of discontinuities in the solution. E.g when you're solving with a fixed temperature and it's trying to calculate the enth_mol, as in [[idaes.ph-formulation]]. 

It also could be the true value of a variable is outside of the bounds - check for this, the variable will be right on the limit - either it's maximum or minimum value.

Even if your degrees of freedom is zero, double check that the right things are defined. E.g if you have a heater, and have defined inlet temperature, pressure, and enth_mol, even though the whole unit op might have DOF=0, the state block is overdefined - T can be calculated from P and h. 

## `failed to converge`

This could be because scaling is wrong, or you've got super small gradients that you're working with. The solver might be taking tons of steps in the direction without getting there in time.




