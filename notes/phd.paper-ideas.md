---
id: 48cec95ju5yug27kevetfw5
title: Paper Ideas
desc: ''
updated: 1757547389628
created: 1757375179650
---

# Degrees of Freedom Modelling

Present a novel way of squaring a mathematical problem, by choosing state vars and using replacement. 

Discuss how this makes initialisation easier, as initialisation can use the guesses of the original state vars every time.

Present an algorithm for figuring out if one degree of freedom can be replaced by another degree of freedom or if they are independent. (TODO) E.g why can I replace pressure with enthalpy, but not flow with enthalpy?

How does this work for dynamics? How does this work for 1d?

## Related things to look into

- how does the Dumalge-Mendelson overconstrained and underconstrained set get computed?
- What does the rank of our system of equations mean and should we control it?
- Is there a good way to visualise all the constraints in a model or the over/underconstrained set? 


# Physics informed Surrogate Modelling

PINNS often need specifically crafted loss functions to help with training. Instead, could we combine a set of data with an IDAES first principles model to create a model that both fits the real data well, and also generalises sensibly to work outside the training range. A variety of methods could be used for this, one method is:

- Use paramest on first principles model with the data
- Generate surrogate data (outside of source range)
- Train on  the surrogate data, then fine tune with the original data.

There might be other ways. What about using the IDAES model to calculate derivatives, and then using those derivatives in the conventional PINNS approach?

# Python version of IPOPT

Rewrite IPOPT from C to python/mojo. This should make the solver much more accessible to other people that aren't familiar with writing CPP code.


