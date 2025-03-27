---
id: dew8oru9mfpiytmnxz0e5rw
title: Idaes
desc: ''
updated: 1743117524913
created: 1743117524913
---
# How this might link to other stuff

Dynamics, Surrogate modelling

How to make something more reliable

How to make digital twins that can update based on learned data/live data

How to update the centers/sigmas of a RBON to improve accuracy over just using kmeans

# Purpose

To make it possible to do surrogate modelling for dynamics

To help me visualise and understand how RBONs work

# Research Questions

How are RBON's implemented in surrogate modelling/pyomo?

Can RBON's be used to model dynamics?

Do they support solving backwards?

Are optimisation tools such as IPOPT a valid way to calculate parameters for a RBON?

# Method

Based on the RBON paper (citation needed) a Pyomo model is created.

Sets used to specify dimensions of the input of the higher and lower networks, and the weights.

Weights can be loaded from an input file or by fixing variables.

Fitting can be done in the julia model on training data, and then the model can be run in PYOMO/IDAES.


# Findings

## Complexity 

Complexity is dependent on:
- The number of sensor locations of the function U
- The number of query points in V (these are specified in the vector y)
- The dimesion of the query points in y 
- the dimesnion of the input of the function U

As well as
- the number of rbfs in the upper network
- the number of rbfs in the lower network.

The number of centers and sigmas is equal to the total number of rbfs (upper  + lower)


The number of RBF weights used in linear regression (main weight layer) is = num_lower_rbf * num_upper_rbf

The number of linear transformation weights for the affine transformation is = num__query_locations


total number of constraints from inputs to rbfs is O( num_sensor_locations * num_sensor_dimensions  * num_upper_rbfs + num_query_dimensions )
total number of constraints from weights to rbfs is roughly O( num_lower_rbf * num_upper_rbf * num_query_locations)
total number of cronstraints from weights to linear layer is rought

# Discussion