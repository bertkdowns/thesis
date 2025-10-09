---
id: pfwd0ehfvrmgc91pl7afzb6
title: Live Wishlist
desc: ''
updated: 1760050927968
created: 1760044323441
---

# Intro

This document is a brainstorm of what the Ahuora live platform "could be", and what the MVP "could be". It's supposed to be a bit wild, imaginative, to set the vision.

I'll try split the MVP into two parts, one for the live platform, one for the online learning features

# Wishlist / Brainstorm / Ideas

- Can run headless on e.g a raspberry pi

- Can do parameter estimation if we have more variables than we actually need 
   * you can specify how reliable you think the measurements are and it'll take that into account

- for variables we don't have, we can approximate/hardcode

- if we have a similar variable (e.g pump speed) but we really want flow rate, we can over time (through averaging mass balances, comparison with upstream values, and paramete restimation) figure out the relationship between pump speed and the flow rate. E.g learning efficiency and losses etc (via param estimation)

- You export a scenario in the platform and can load the scenario onto the headless version

- you can configure all the live stuff (kalman filters etc) and simulate it in the platform

- Kalman filters, rolling averages, and other techniques can be used to specify values. Potentially add these as blocks on the flowsheet?? or in scenario view

- You can preprocess/postprocess data with whatever python stuff you want.

- While the flowsheet is solving every time step, you can also train a surrogate to predict outputs (using online learning techniques) and record the accuracy of the surrogate vs the platforms result (as surrogates might be quicker/higher resolution)

- Auto figure out what parts of the solution are zero and deactivate those parts of the flowsheet so that things become simpler/more reliable

- Setting minimum/maximum bounds that variables can be fixed to when you upload.

- Automatic anomaly detection

- simple gui with the python version (or a web url if you want to connect to the headless runtime and view what it's doing)

- OPC in - OPC out (Possibly as a layer around a more simpler system) (either as a client to get input and server to send output, or client to get input and client to set output)

- can work with conventional SCADA systems or HMIs

- can modify the loaded scenario using pure idaes if required.

- main thing you need to do to setup is just set all of the input data sources/columns (as a pandas series?)

- automatically interpolates data if data is recieved at different rates

- If a solve fails it can warn you but still predict with e.g surrogate modelling or the last successful solve & extrapolation

- edit constants in real time

- batch processing mode: can switch out the flowsheet (or parts of the flowsheet) for a different model
  * anything which is relevant to both is kept and so it can learn from both
  * anything which is different is handled appropriately.
  * auto-detect when a batch is changed, flow is diverted, etc.

- can model dynamics through transfer functions between solves, e.g hx residual heat - heat up and heat down - and can learn from them.


# Minimum viable products

## Ahuora Live

- Python Package that takes a idaes_service Scenario file and a mapping of name-> propertyValue (e.g from mss), and solves the model, and allows storing the results in a internal history db/ opc-ua server, or sending on to a message-based system.  

## Online Learning

- Online learning model to predict the outputs of the entire flowsheet from the dataframe that comes in, using regression techniques
- Automatic anomaly detection from the inputs & the outputs of the solve.
