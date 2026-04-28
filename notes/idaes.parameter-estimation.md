---
id: dwfz6j3fsvgggqjdf05h4tz
title: Parameter Estimation
desc: ''
updated: 1777336913240
created: 1777328063346
---

Written based on my parameter estimation test [here](https://github.com/bertkdowns/geothermal/blob/a758e41a1214c4765906f961f5a921794b2504c9/parameter-estimation/parameter_estimation.ipynb#L4).

# Initial Thoughts on pyomo's Paramest

The [Paramest](https://pyomo.readthedocs.io/en/6.8.0/contributed_packages/parmest/driver.html) library is very powerful, and definitely makes parameter estimation easier than setting it up yourself, though it is a bit clunky to use. Rather than just having functions where you have to specify the required arguments, there are certain suffixes and expressions that you need to have in your model for it to work.

Some things that the documentation didn't quite make clear, that it is helpful to know:

- If you are specifying your own cost functions, you have to define them as `m.FirstStageCost`, `m.SecondStageCost`, and `m.Total_Cost_Objective` for the objective (actually I think the objective can be named whatever).
- you also need to define the suffixes `m.unknown_parameters` and `m.experiment_outputs`.
   - `m.unknown_parameters` is sometimes reffered to as `theta θ`  and defines the things that you want the parameter estimation to estimate. The estimated best values for these are printed by the estimator when it finishes running `theta_est`.
   - `m.experiment_outputs` are actually the inputs to paramest (presumably data you got out of an experiment you did earlier). This is a mapping of variables in the model to values they should be "fixed" too. In reality, these variables should not be fixed (if any of them are fixed it will cause dof errors or infeasibilities) but parameter estimation will find the value of `m.unknown_parameters` (theta) that best fit the expected values.

Paramest doesn't do any sort of scaling, so if you have drastically different scales for your measured variables (e.g some of your "experiment outputs" are temperature , some are pressure) you will have problems. you could add your own objective function to fix that. I personally think it would be good to make some wrapper Experiment classes or methods that have to define the unknown parameters and experiment outputs, and also handle scaling etc too.




