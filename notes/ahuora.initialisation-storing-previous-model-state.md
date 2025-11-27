---
id: i7fgoggwocwh9vpirl5y828
title: Initialisation Storing Previous Model State
desc: ''
updated: 1747260584622
created: 1747200746572
---

# The Problem

I started trying to model an Evaporator, however after adding a few unit operations it started to fail to solve. All the conditions I was adding should have been feasible though. 


Looking at the solver logs, some of the pressures and temperatures that were being set were way off the correct values. This can happen for two reasons

- it is possible that some unit operations had bad initialisation routines. We had just built a DSI and Translation block, and hadn't evaluated the initialisation routine much
- We support adding additional constraints, which may not be taken into account during initialisation. E.g we are calcualting the amount of steam to inject based on the target outlet temperature, the initial guess may not have provided enough steam resulting in too low of a temperature for downstream unit operations to work with. (you can't cool down a stream if IDAES thinks its already cold!) 

To investigate this, I recreated some of the model in idaes  ( [see initialisation_experiment_evaporator.py](https://github.com/bertkdowns/direct_steam_injection/tree/09d52046c1044b7b88d5959c77fb16d958ec5899) ) . This flowsheet included a Direct Steam Injection, then a flash valve to lower pressure, then a phase separator to take out the steam, then a heater that heated the condensate up.

The valve was failing to initialise when I fixed the outlet pressure, so instead I set a constraint globally - effectivly simplifying the initialisation routine, but making the final solve more difficult. 

```python
m.fs.flash.valve_opening.fix(1)
# Add a constraint to fix the outlet pressure of the flash, which should calculate the valve coefficient.
@m.fs.Constraint()
def flash_pressure_constraint(fs):
    return fs.flash.outlet.pressure[0] == 75_000 # 75 kPa
m.fs.flash.Cv.unfix() # IDK why this is fixed by default.
```

However, when I added a heater after the valve, this meant the heater had much worse initial values, and so its initialisation routine was much more likely to fail, or end up a long way away from the correct solution.

# A mental model

Through trial and error, I found that I could fix the heat_duty of the effect, but I could not fix the temperature of the effect - Initialisation would succeed but then finding the solution would fail. This happened even when I knew the temperature was solveable, because I had calcualted that temperature previously from the heat_duty. 

There are a couple different ways to explain this behaviour, I'm not sure which mental model is correct:

![Potential Problems with initialisation: Starting outside the solution space or moving outside the solution space](assets/initialisation_potential_problems.drawio.svg)

In the above diagrams, I'm imagining the "Feasible Region" as the set of all possible solutions for the variables, with those constraints. However, this space is limited by the bounds of the variables and the constraints. 

The left image shows an initialisation point that is outside the feasible region completely. This could happen because each sub-unit model is feasible when initialised, but the constraints between unit models are infeasible. Thus, initialisation succeeds but solving fails.

The bounds of variables can interact in weird ways too. For example, at a high pressure the enthalpy might be very large, and the temperature still quite low. However, at a low pressure that same enthalpy may be infeasible, because it would result in a temperature outside the maximum temperature bound. If scaling is off, it may be possible to get a case on the right, where the solver starts within the solution space but tries to move outside the solution space to solve the problem.

This way of looking at the problem looks similar to the way homotopy works. IDAES has a [homotopy meta-solver](https://idaes-pse.readthedocs.io/en/1.4.0/core/util/homotopy.html), and so perhaps that might help in solving when the initial values are incorrect?

The other way of looking at the problem is by considering convexity. Perhaps during solving, its getting stuck at a local optimum rather than a global one:

![Potential Problems with initialisation: Getting stuck at a local optimum](assets/initialisation_potential_problems_2.drawio.svg)


# Solution: Better Initialisation?

Regardless of the way you look at the problem, it is clear that a better initial guess means you are much more likely to be able to find a solution.

In my experience building models on the platform, I often change one thing at a time - adding a new unit operation here, changing which variable is being constrained there - rather than doing everything at once. This helps me identify what's causing an error, as I know exactly when it stops solving. Because I always only change one thing at a time, in theory the previous solve is pretty close to the correct values for the next solve. Thus, if we saved the state of the previous solve, things would likely work fine for the next solve. 

# Test case

## Hypothesis

Using the state from the last solve values is better than using initialisation. By "better" I mean it:

- Takes less IPOPT steps to solve (estimated 1/2 to 1/5 as much)
- Does not fail to solve as often as using the initialisation routines.

## Test case

I tried with a bunch of different heat duties and temperatures, with the initial values being either from the previous solve, or from the initialisation routine.

[See initialisation_experiment_evaporator.py](https://github.com/bertkdowns/direct_steam_injection/blob/acaa91df88f5f0e0f1cc14d326e65c46f36ae6ed/initialisation_experiment_evaporator.py)


Case | Solving time (s) | Iterations | Status
 --- | --- | --- | --- 
heat duty of 0 with initialisation  |  1.0205872058868408 |  54 |  Optimal
heat duty of 1000 with initialisation  |  0.95733642578125 |  41 |  Optimal
heat duty of 2000 with initialisation  |  0.9359180927276611 |  45 |  Optimal
heat duty of 4000 with initialisation  |  0.9301490783691406 |  37 |  Optimal
heat duty of 8000 with initialisation  |  0.9101030826568604 |  41 |  Optimal
heat duty of 12000 with initialisation  |  0.9309337139129639 |  49 |  Optimal
heat duty of 16000 with initialisation  |  0.997211217880249 |  43 |  Optimal
heat duty of 20000 with initialisation  |  0.8305227756500244 |  38 |  Optimal
heat duty of 30000 with initialisation  |  0.8931007385253906 |  34 |  Optimal
heat duty of 60000 with initialisation  |  1.0755863189697266 |  50 |  Optimal
heat duty of 0 from previous solve  |  0.07674407958984375 |  5 |  Optimal
heat duty of 1000 from previous solve  |  0.06808257102966309 |  3 |  Optimal
heat duty of 2000 from previous solve  |  0.12989449501037598 |  3 |  Optimal
heat duty of 4000 from previous solve  |  0.1135873794555664 |  3 |  Optimal
heat duty of 8000 from previous solve  |  0.08837270736694336 |  3 |  Optimal
heat duty of 12000 from previous solve  |  0.07968568801879883 |  3 |  Optimal
heat duty of 16000 from previous solve  |  0.08147954940795898 |  3 |  Optimal
heat duty of 20000 from previous solve  |  0.07163739204406738 |  3 |  Optimal
heat duty of 30000 from previous solve  |  0.06998658180236816 |  3 |  Optimal
heat duty of 60000 from previous solve  |  0.06609487533569336 |  3 |  Optimal
temperature of 351.15 from previous solve  |  0.07333040237426758 |  4 |  Optimal
temperature of 353.15 from previous solve  |  0.06981587409973145 |  3 |  Optimal
temperature of 355.15 from previous solve  |  0.06970381736755371 |  3 |  Optimal
temperature of 357.15 from previous solve  |  0.06816983222961426 |  3 |  Optimal
temperature of 359.15 from previous solve  |  0.07016396522521973 |  3 |  Optimal
temperature of 361.15 from previous solve  |  0.06571102142333984 |  3 |  Optimal
temperature of 363.15 from previous solve  |  0.05827641487121582 |  3 |  Optimal
temperature of 365.15 from previous solve  |  0.05720949172973633 |  3 |  Optimal
temperature of 330.15 from previous solve  |  0.056287527084350586 |  3 |  Optimal
temperature of 375.15 from previous solve  |  0.05764889717102051 |  5 |  Optimal
temperature of 351.15 with initialisation  |  1.3332476615905762 |  59 |  Infeasible
temperature of 353.15 with initialisation  |  1.3413028717041016 |  40 |  Infeasible
temperature of 355.15 with initialisation  |  1.2737398147583008 |  39 |  Infeasible
temperature of 357.15 with initialisation  |  1.333902359008789 |  43 |  Infeasible
temperature of 359.15 with initialisation  |  1.2751681804656982 |  47 |  Infeasible
temperature of 361.15 with initialisation  |  1.3430328369140625 |  51 |  Infeasible
temperature of 363.15 with initialisation  |  1.2962055206298828 |  53 |  Infeasible
temperature of 365.15 with initialisation  |  1.364783763885498 |  49 |  Infeasible
temperature of 330.15 with initialisation  |  1.23244047164917 |  42 |  Infeasible
temperature of 375.15 with initialisation  |  1.3908917903900146 |  140 |  Infeasible

The only solves that failed were the temperature solves with initialisation happening first. Solving with the same temperature values but starting with the previous solution worked fine.

Additionally, using the previous solve meant it only took 3 or 4 iterations to solve, around 10 times less than using initialisation. (Note that this is only the number of iterations to solve the final result, not including solver iterations for initialisation purposes.)

The wall clock time was around 20 times better too - this includes the time to initialise.

## Conclusion

From this, it appears that my hypothesis holds - that using the previous solve will speed things up a lot and make solving more reliable

To implement this in the platform there are a couple of options:

1. Just pass all the previous values as guesses to IDAES service. These can all be used for initialisation purposes. 
2. Store "guesses" for each property value. these can be used for initialisation, manually set by the user if they want, and automatically updated when a solve succeeds.

There aren't that many advantages of option 2 over option 1, so maybe option 1 is simpler.

However, there are other variables that we don't store in the platform - the internals of property packages, for example. So we would still have to do some level of initialisation.

An alternative method is to:

- Use idaes's [JSON serialisers](https://idaes-pse.readthedocs.io/en/1.4.0/core/util/model_serializer.html) to store the values of everything in a flowsheet (or unit operation). This would mean we don't have to initialise at all, as long as there are no structural changes (e.g a different property package or new unit operation.) This might be really good, however we need to handle those cases. Also, if we are sometimes enabling dynamics, we will need to store a seperate initialisation state for the dynamic model and the steady state model. 

I think doing both this and option 1 from above is a good idea.