---
id: gyie50z9duxou24o6ntwm1t
title: Conference 2025
desc: ''
updated: 1757482089551
created: 1757375197943
---

# Process Family Design

Create a Modular hx and reactor, absorber, stripper, design etc. This is better than a custom design per factory for economies of scale.

Another similar thing is optimisation based conceptual design - using superstructures to figure out which reaction pathway is the best to achieve your desired outcome. There can be a lot more cost savings there.



# Uncertainty Modelling: PyRos

[PDF showing an example of PyROS](https://www.osti.gov/servlets/purl/2328607)


# Design of Experiments

<!-- Josh Morgan EEMPA -->

pilot scale plants using bayesian methods is one way to do design of experiments.


# CCSI2 Unit operations

These include Absorbers, regerators, contactors, packing. We should look into including more of these in IDAES.

# CCSI2 FOQUS Software





# CMR Wastewater Treatment - Prommis Flowsheet



# Technology Readiness Levels

[See Wikipedia](https://en.wikipedia.org/wiki/Technology_readiness_level)

I think Ahuora is currently (2025) at level 4 or 5. 

# General workflow

Model development -> refinement -> pilot tests


# ProMMis

With Prommis/Critical Minerals, you need to be able to model very low concentrations

# ProMMis Flowsheet Processor UI

# Reactoro Grey Box - Water Tap



# Digital Twins





With minerals, the feedstock changes over time, this is one of the big things that digital twins can help with modelling what will happen downstream.

Mass spec doesn't work for control because it takes ages

NAWI is working with WaterTap to create a protocol for digital twin implementation <!-- Adam Atia is the person to contact about this-->

Another area of research: how flexible can a plant be - extracting different minerals at different times? How flexible is it to adapt a different digital twin to work on a different mineral/layout?

Digital twins are good for responding to changes in the environment - e.g an adaptive control system that responds to variable electficity prices

## Startup and Shutdown

Often people Figure out what the optimal solution is with IDAES and then use something else to model startup, shutdown, etc. Modelica is good for control purposes and simulation. But in theory we could use idaes too. 

## Moving Horizon Estimation

## Soft Sensors

## Some DT Systems to look at

Pilot plants are good for digital twin studies because it's easy to play around with them without having problems. Potentially I could create my own pilot plant so that I can do things myself. E.g a hot water loop with heat exchange to a thermal load (https://chatgpt.com/c/68c0cd02-06b0-832e-bbe3-667063e1f316)

### University of Kentucky Pilot Plant

<!-- Marcus Holly, Josh Warner (CEO of Rycera) -->

The flowsheet is accessible at [github](https://github.com/prommis/prommis/blob/main/src/prommis/examples/uky_example.ipynb). (another version with more docs is [here](https://prommis.readthedocs.io/en/0.8.0/_modules/prommis/uky/uky_flowsheet.html))

They have a setup with a couple raspberry pi's gathering data so they can see all the properties of their pilot plant in real-time. However, they are mostly using a steady-state model and haven't looked into making it dynamic yet. The dynamics is mostly affected by the concentrations of the various minerals in the tanks. 

Getting the flowsheet to work in a digital twin for dynamics could be a good thing for me to do, but it might be quite a big task.

Potentially, we could use parameter estimation techniques to estimate the concentrations of the minerals based on the dynamic responses. Alternatively we could estimate other parameters based on the DOFs mentioned in their docs.

<!-- https://www.anl.gov/profile/richard-brian-vilim might have another potential case study in digital twins of plants, he focuses more on control and nuclear stuff though. -->


### WaterTap Reverse Osmosis

<!-- Diego Rosso was working on this, but can talk to adam atia as well -->

WaterTap has a pilot plant for reverse osmosis that they're working with, replacing the SUMO (SuperModel platform) for modelling with a waterTap model. Their stack includes real-time updates with a python script to send data back and forth to the HMI. I could ask about using that as a case study.

They also have some reverse osmosis flowsheets that I could look at already.

# UCLA remotely monitored water treatment

UCLA has projects with a remotely monitored water treatment system in africa (I think this is also reverse osmosis). They get real-time data from the system, which could also be used for digital twin modelling.





# Software Development Methodologies

Summary from Keith Beatie

The goal of software development is to provide confidence.

This means that software must be:
- Functional, for: 
   - Users
   - Stakeholders
   - Developers
- Usable, via
   - Good developer APIs, or good GUIs
   - Good documentation
- Stable, via
   - Testing (Unit testing, Integration testing, Regression Testing)
   - Static Analysis
   - Regular Releases (Date driven works well for idaes, if you miss one then "catch the next train")
- Secure:
   - Can be used with protected data and IP 
   - Good code access and review helps (more eyes on the code) 

## On Open Source

IDAES uses BSD, which is very permissive as they want as many people to use it as possible

In open source software it can be hard to get feedback and input - you aren't aware if people are using your software or not.  

The IDAES roadmap of features is driven by engagement from users.

## On Writing Software

Writing software is like cooking: cooking for yourself vs cooking for a dinner party vs being a caterer.

When you're cooking for yourself, you can get away with something super simple that gets the job done, e.g scrambled eggs on toast. In code, you might just write a quick script and hardcode in your paths to your source files, etc. You just write for whatever your current python environment is, and don't worry about much else.

When you're cooking for a dinner party, that's like writing software for your team. You know all these people so you can get away with some things (perhaps your team already uses a certain set of tools, e.g they're all on linux so you don't care about a windows version). If they get stuck they can just ask you so it's not a big deal.

When you're being a caterer, you have to cook for a bunch of strangers you know nothing about. This is like writing software for people you've never met. It's gotta work on whatever environment, they might never talk to you so you have to have good documentation etc, and it should be understandable without too much other context.

### Relevance

At each scale these things are different. Writing code that is run once vs 100 times vs 10000 times is always quite different. In our unit model system, we have had different ways of doing it from 5 unit operations to 50, to 500, to 5000.

- 5 unit operations: we can code up logic for each unit op
- 50 unit operations: we have a bunch of config files and some different adapters we can use
- 500 unit operations: we have to simplify the config file so it's all part of a single idaes Unit Model definition,
- 5000 unit operations; we might need an an "app store" or packaging functionality to choose which ones are allowed.


# Some Next steps for Ahuora

## More analysis tools

We have implemented some of the IDAES functionality for analysis and diagnostics, but there's a lot more we can do for that.

## More unit ops - reactions, prommis/water tap

Having more unit operations means that we can model a wider variety of flowsheets.

## Better initialisation methods

We need initialisation to better handle cases where things aren't quite feasible, but may be feasible on the final solve. This probably involves creating some relaxations of the unit models to allow things to solve to a good initial guess in more conditions.

# Other Things

WaterTap is starting an 8 Week course called WaterTap Acadamy to help people learn to use their stuff. Perhaps we could consider a similar thing for the Ahuora Platform.

Are Virtual Office Hours a helpful resource? Should we use this more to catch up with other people? I wonder how we can be more effective in those things.

IDAES and the other teams have weekly dev calls that we could potentially join.

We should show our playwright tests in more demonstrations, to show that we have some level of reliability - it's easy to understand for non-software people and looks cool.

Reproduction studies are good because it validates other peoples research. Maybe we should emphasise doing reproduction studies more.



