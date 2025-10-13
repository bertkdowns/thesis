---
id: pfwd0ehfvrmgc91pl7afzb6
title: Live Wishlist
desc: ''
updated: 1760386898619
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


### Potential class Structure:

```
AhuoraLiveSolver Class:

  init(file_name.json) -> AhuoraLiveSolver:
    Constructor method, that creates the class and loads the JSON file in.
    Validates that the scenario is correct and is a mult-steady-state scenario,
    and creates a list of all the input parameters that need to be passed
    in order to solve the model.

  solve(data_dictionary) -> Results:
    Loads in all the values from the key-value dictionary based on the mappings, 
    validating to make sure that nothing is missing.
    Initialises (if required e.g first time) and solves the model.
    Returns the solve results in a suitable format, e.g the format that IDAES-service
    sends to idaes_factory (or something different if that isn't the best)

OPCIngestor Class:
  Not sure exactly how this class should look, but it should be able to establish
  a connection to an opc server, and get all the required data for the ahuora
  live solver, either by subscribing to updates and maintaining state, or 
  by re-requesting the data when a call to get the values is made.

  dump_data_dictionary() -> data_dictionary:
    Dumps all the required data in a dictionary of suitable format for passing to the AhuoraLiveSolver class

OPCRecorder Class:
  
  store_data(Results):
    Stores the results in the OPC server. Note that it should take the same input format as is outputted by the Solve method.

We also need a way to trigger solves on a regular basis, which might require some thought. Should we trigger a
solve every minute? Or should we trigger a solve every time a value changes? or every time an external service
requests updated solve information?
```

After we've made an OPC-UA version of this, it would be good to make one that works via MQTT or some other protocol too (maybe
ask what they're using for the HMI in some of the factories near us)



## Online Learning

- Online learning model to predict the outputs of the entire flowsheet from the dataframe that comes in, using regression techniques
- Automatic anomaly detection from the inputs & the outputs of the solve.
- should interact with the Ahuora Live system.


(maybe look at CapyMoa? That's the library that the online learning people use)

### Potential class structure:

```
AhuoraOnline class:

  predict(data_dictionary) -> Results, certainties:
    based on the input data, it predicts what it thinks the most likely results will be if the solve was completed, and the certainties.
    The idea is this should be pretty accurate but much faster than solving from first principles, especially for more complex models.
    Note that the type (inputs and outputs) of this is the same as for the AHuoraLiveSovler class.

  train(data_dictionary, Results):
    updates the model to be better at predicting in future, from the results of an ahuora solve.

Other methods could include:
  - a method to determine if this data point is worth solving with ahuora and training on, or if the model is confident enough in its 
    prediction that it doesn't need to solve it.
  - a method to flag anomalies, if we get data coming in that is very different to what we expect the input data to be.
```

# What I can do

Since these projects are intended to be done by others, I probably shouldn't try develop the core of it. However, I can do some related things:

- Find/Make an OPCUA server for the Tennessee Eastman Process, or simulate one from the data I was given of the factory/geothermal plant.
- Update the Ahuora platform to provide the json files with more of the scenario data
- better define what the Results format should be out of each. Should you have to define what results you want as an input to the AhuoraLiveSolver? Possibly this is best, but does that limit its ability to be a digital twin? don't you want as much as possible? How does the AhuoraOnline know the data format to predict?


