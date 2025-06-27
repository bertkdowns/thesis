---
id: cgxv3blivd3f4tj5afr1jkk
title: Creating a Helmholtz Property Package
desc: ''
updated: 1750976405420
created: 1750975506960
---

... or more  specifically, supporting a new compound in IDAES's helmholtz property package.

Each helmholtz property is configured with 5 different files, however, the [json file](https://github.com/IDAES/idaes-ext/blob/f60f28be990755749906567ac73fb1e0a188fa42/src/general_helmholtz/param_data/co2.json) is the most important. In there, you have to specify what equations you are using and what the coefficients for the equations are. 

The other files are all generated, except for the [.py file](https://github.com/IDAES/idaes-ext/blob/f60f28be990755749906567ac73fb1e0a188fa42/src/general_helmholtz/param_data/co2.py) which contains the script to generate the `.nl` files, and the `[compound_name]_parameters.py` file.


The full documentation for how to set up all the properties is avaliable [in the idaes-ext repository](https://github.com/IDAES/idaes-ext/blob/f60f28be990755749906567ac73fb1e0a188fa42/src/general_helmholtz/doc/documentation.pdf), and it's definitely worth checking out. If you want to add a new compound, you only really need to read chapter 5 - parameter files.

Taking a closer look at the .json file, you'll need to set up:

- The Helmholtz Equation of State
- The residual
- any extra properties, e.g viscosity.

The formulations of those are documented in the documentation (there are multiple different formulas for the equation of state etc, so you have to check what equation type it is) and they are implemented in [idaes-pse](https://github.com/IDAES/idaes-pse/blob/6a588c799fbaf00cb1b32b814fcee6ad111bc4c1/idaes/models/properties/general_helmholtz/expressions/phi_ideal_type01.py)

I've done this for butane, which should hopefully be merged [here](https://github.com/IDAES/idaes-pse/pull/1636), and looking through that might provide some extra context.

