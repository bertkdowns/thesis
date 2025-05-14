---
id: 0uxp8k7ie0jzychzzs57abz
title: Modelica
desc: ''
updated: 1743117525020
created: 1743117525020
---
# Thoughts on modelica

Based on initial research, I was unsure whether modelica is a good fit for the project.

Modelica has been around a while, and is standard in some spheres, but often in mechanical disciplines. Mathematically, it's pretty similar to pyomo in that it provides methods to create equations. Pyomo appears more focused to discrete solving, e.g steady state, while modelica is setup for dynamic solving by default.

[Modelica Github](https://github.com/modelica)

One potential advantage of Modelica is that there are a large number of [libraries avaliable](https://modelica.org/libraries/), ( see
[Third party libraries Github](https://github.com/modelica-3rdparty)). There are also some that seem pretty relevant or comparable to IDAES.

For example, there's the. The [compressor](https://build.openmodelica.org/Documentation/IDEAS.Fluid.HeatPumps.Compressors.ReciprocatingCompressor.html) docs seem very similar to the [IDAES compressor docs](https://idaes-pse.readthedocs.io/en/2.4.0/reference_guides/model_libraries/generic/unit_models/compressor.html) in pyomo.

[HelmholtzMedia](https://github.com/modelica-3rdparty/HelmholtzMedia) (see the [poster](https://gfzpublic.gfz-potsdam.de/rest/items/item_247376_1/component/file_247375/content))


Todo: read [Steady-state Optimization of Modelica Models and Functional Mockup Units with Pyomo
](https://www.researchgate.net/publication/388094864_Steady-state_Optimization_of_Modelica_Models_and_Functional_Mockup_Units_with_Pyomo)