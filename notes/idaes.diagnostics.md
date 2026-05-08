---
id: 9szxg244zp02ngrhxps2pbr
title: Diagnostics
desc: ''
updated: 1776382360489
created: 1776376169027
---

One of the most frustrating error messages to see when doing mathematical programming is: "Solver failed to return an optimal solution. Solution status: warning, Termination condition: infeasible".

Why? What part is infeasible? This is always so confusing to figure out.

Our initial debugging strategy was just to recheck everything, make sure out equations were okay, and try guess what could be the problem. However, this doesn't scale well with big models, so better targeting is needed.

For our constraints that we add (e.g fixing temperature or another expression) we now show the infeasibility in those constraints. That helps a lot: if an infeasibility is more than 1e-5 it probably means that the problem is somehow related to that.

E.g:

```
Property: fs.Crystallizer3_1642627.utility_in[0.0].temperature, Infeasibility: 24.694267554440955
```

In this case, it seems like the utility inlet temperature was infeasible. This was probably because it was too low: the crystalliser energy balance needed more energy but there wasn't enough in the stream: it had already cooled the outlet to zero degrees celcius (the lower bound for temperature) and it couldn't get much more energy than that, so the only option was to push the inlet temperature up. (or it could have also tried to push the feed flow rate down, but that variable is directly fixed, so it's harder to see the error there.)

While this isn't a perfect explanation of how, the "not enough energy" problem is quite common. There are a bunch of different common diagnostics issues that show up in different types of unit operations

Unit Operation | Diagnostics Issue | Description | Resolution
 -- | -- | -- | --
Valve | Expected phase change but already all vapor | Sometimes you have a valve targeting an outlet temperature, but valves can only adjust pressure. Temperature changes only during a phase change (liquid-vapor). If the fluid is already vapor, it can't drop in temperature any more. | Change the inlet fluid temperature, or target outlet pressure instead.
Heat Exchanger | Not enough energy on on one stream for the heat exchange | The amount of heat transferred is affected by the temperature difference between the hot side and cold side, and the HX coefficient and area.  If for some reason the temperature difference between the hot and cold side is larger than expected, or the flow rate on one side is lower than expected, there may not be enough heat to transfer. This is more common when you have fixed one side to a specific outlet temperature and the other side cannot provide the energy to reach that temperature. A special case of this is if a condensor is getting liquid instead of vapor in, or a evaporator is getting gas in - there is no energy as the phase change has already happened. | Only fix U and A rather than outlet temperatures, don't try as drastic a temperature change, decrease u and a until it solves, or increase inlet flow rate/temperature of the side with insufficient energy.
Heat Exchanger | Temperature balance infeasible | If we are targeting a cold side outlet temperature of 50 C, but the hot side inlet is only 40 C, it is physically impossible to get the cold side to above 40 C | change what we're targeting (switch to U and A) or ensure that the other inlet/outlet has enough temperature.
Crystalliser | No Vapor in utility inlet | If we don't have vapor in the steam utility inlet, we probably won't get enough heat as the vapor phase change is what provides the heat to the crystalliser. The crystalliser has all the general HX problems too, except we don't do temperature balances | Make sure there is vapor and sufficient energy in utility inlet
Crystalliser | Not enough work to create a flow in the vapor outlet | The crystalliser fails to solve without a flow in the vapor outlet. | Increase the heat duty until some vapor is created.
Crystalliser | Vastly different flows | also kinda true of normal HX, sometimes we set kg/s instead of kg/h for the inlet flow. This meant we didn't have enough flow | provide enough utility or a lower inlet flow
Crystalliser | Wrong Property Package | This only works with Nacl and Water | Change the property package
Desuperheater | Liquid inlet | This is supposed to desuperheat steam. Won't really work if there is no steam to desuperheat | change the upstream properties or just use a heater
Compressor | Doesn't work on liquid | Liquids are incompressible.  | Make sure there's vapor instead.
Splitter | Not enough flow to split | If you fix a certain flow rate on the outlet, and that is more than the flow rate on the inlet, it's impossible | fix a different flow rate or provide more inlet flow





 