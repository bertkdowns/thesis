---
id: hqjirh6xz6wie4nhsjimgoi
title: Heat Exchangers
desc: ''
updated: 1749683709865
created: 1749683709865
---


# How do we calculate the Heat Transfer coefficient?

The overall heat transfer coefficient (U), is actually calcuted from:

The heat transfer of the fluids on both sides of the heat exchanger, plus the heat transfer resistance of the pipe.

the heat transfer of the fluids on either side of the pip is calculated from a bunch of other stuff, including:

- the nusselt number for the fluids
- the prontly number (or something like that) - for force fluid convention



there is a python library called `ht` that can calculate all this stuff. TODO: look into that!

We could potentially implement all of that in idaes - alternatively, perhaps a parameter estimation technique could be useful instead.


