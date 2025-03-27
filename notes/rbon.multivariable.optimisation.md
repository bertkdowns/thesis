---
id: g5aiigguqnthzu281jloki4
title: Optimisation
desc: ''
updated: 1743117524925
created: 1743117524925
---
# What

Being able to globally optimise a RBON based on input data to find the best parameters for rbf centers and sigmas, as well as weights and the affine transformation

# So What

The RBON network training method currently calculates each layer manually, using heuristics. K-means is used to find the centers, and spreads. Then the weights of the RBF products are calculated using least means squared, and the same is done for the final affine transform. 

However, there could be better K-means centers and sigmas, that would give a more optimal result. The weights for the rbf products and affine transformation could likewise be calculated together. 



# RQ

How much does global optimisation of a RBON affect its performance/accuracy?

# Method

Build a RBON in pyomo, and initialise it to the weights calculated by the fit method in julia. Then, set up an optimisation problem to minimise the error of the output values given the input values in pyomo, using the same training data used to fit the RBON initially. Then, solve the system.

The main problem with this is that it could be a really, really big complex problem.

# Findings