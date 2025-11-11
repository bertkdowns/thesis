---
id: b62gmeo8hftecfg146pqb0p
title: Digital Twins
desc: ''
updated: 1762899502069
created: 1762895964500
---

# What is a digital twin?

One time I went to a PhD confirmation and the student spent a lot of time talking about their model, what a digital twin was, and how they were going to build a digital twin. Then when it came time for the examiners to ask questions, one of the examiners said, "Digital twins have been around a lot these last couple of years and I don't really get it. What is a Digital Twin?" We all found it very ironic as the student had just been trying to explain what a digital twin was through their entire PhD Confirmation. It seemed pretty obvious to us all what a digital twin was. However, as I keep thinking about it, there is an element of relevancy. Digital Twins as a concept suffers from a bit of "It can be whatever you want". 

There's papers that classify digital twins and digital shadows and modelling etc, which are kind of helpful. People talk about digital twins for fault detection, observability, prediction, testing nonexistent systems, model predictive control, and a hundred other things. I guess in some ways that is what a digital twin is: a toolkit and set of digital models/equations/documents/media that lets you do whatever you like. In theory, the number of ways you can interact with it should be as diverse as the number of ways you can interact with a physical object. For a chemical process, it should let you measure the size of the footprint, put chemicals in and see what happens, check if its' going to be reliable, stress test, have a (virtual) operating cost and capital cost, install it into a site, view the fouling build up, etc. However, doing all of that is a lot, so we just do a bit at a time. But really that's only a piece of a digital twin. Perhaps there is som truth in the idea that a digital twin can be "whatever you want."


# How to build a digital twin for Process Engineering

In [Isaac Severinsen's thesis](https://hdl.handle.net/2292/70915), appendix D, there is a Digital Twin Recipe. This provides a good starting point for modelling a chemical system (at least in steady state). Some of his points include:

- Explicitly specify the system boundaries as inputs and outputs (sensor readings or calculations)
- Model the system using data driven modelling, surrogate modelling, or both
- Extract, interpolate, upsample and then downsample your continuous variables
- Use first principles models to verify your predictions, and try multiple different ways to predict properties to compare accuracy. 
- You can regress parameters of first principles models if you don't have everything specified, or sometimes you can approximate or set a constant values
- you can combine results from different approaches (e.g first principles vs data driven) to get a better estimated true value. You need to decide how to weight them - data driven is better if there is more data in that area.
- You need to find a good way to encapsulate and distribute the model (perhaps Functional Mockup units?)

