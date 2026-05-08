---
id: 4k78u14mywcjzaz07viptde
title: Blending Property Packages
desc: ''
updated: 1772484172189
created: 1772483491702
---

# Blending Property Packages

![Blending two property packages together](assets/blending-property-packages.drawio.svg)

The idea behind this is:

- Linear property packages are very easy to solve. No discontinuities etc. They give a general intuition about the search space.

- More exact property packages often have discontinuities that make them hard to solve.

So we start the problem off with a linear property package, converge to an exact solution, and then re-solve with a blended property package that is a
combination of the linear and exact property package. We gradually decrease the weighting to the linear property package, and then finally only use the
physically accurate property package.

This approach might be able to generalise across a variety of different property package formulations - and even to different fidelity models of unit operations such as Heat exchangers.