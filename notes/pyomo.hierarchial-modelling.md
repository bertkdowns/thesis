---
id: t596jttoggoomwdkmhktemv
title: Hierarchial Modelling
desc: ''
updated: 1778277502986
created: 1778276705805
---

There are different levels of fideity we may want to model a plant at, depending on the time scale or equipment. For example, we might want to model a heat exchanger as a 1d discretised operation to get an idea of what the temperature profile is like and if it could be better aligned. We might then model the heat pump it's a part of, and may not need that detail in the heat pump model. Then we might model the heat pump as part of a utility system to generate hot water/steam. Then we might model the plant as a whole, with the utility system and everything else, and may just be interested in how much of each utility we are using. Finally, the plant might just be a small piece of a model of a regional electricity network.

All these systems and scales are linked in the real world, but typically we model each level completely separately. An ideal digital twin would combine these levels so we could see interaction on each scale. How could we do this effectively?

Idea: for each scale, we have a high-fidelity model and a low fidelity model (approximate with constants, or a linear model or surrogate model). The high fidelity models can have sub-models that also have high-fidelity and low-fidelity versions. You can choose what level of fidelity you want to drill down to when simulating something, or just solve the sub-models on their own.

