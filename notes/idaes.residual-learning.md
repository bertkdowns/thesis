---
id: s0yomhm114bnvus6rypqdt9
title: Residual Learning
desc: ''
updated: 1776912379121
created: 1776894351366
---

Residual Learning is when instead of learning a value, we learn the error in a prediction of the value. The goal is to improve the accuracy of the original prediction. 

In an equation oriented framework, predictions are done implicitly through relationships between variables, i.e constraints. To adjust the value of a calculated variable, we must first choose a constraint that has an affect on it to adjust.

Suppose we have the system to define the work of a heater with constant flow and pressure, but varying temperature:

```
T_in : Temperature in
H_in : Heat Energy in.
H_duty : Heat duty
H_out : Heat Energy out
T_out : Temperature out
```

with the following equations:

```
T_in = F(H_in)
H_in + H_duty = H_out
T_out = F(H_out)
```
Where F is a highly accurate physical property calculation that calculates the temperature from the heat energy in the stream.

By specifying the inlet temperature and the heat duty, this model can be used as a prediction for outlet temperature. 

# A naieve implementation of residual learning

Suppose we have the dataset:

T_in | H_duty | T_out (true)
-- | -- | --

We can then calculate the predicted temperature for each case by solving the model, and then calculate the residual:

```
R_additive = T_out (true) - T_out (pred)
R_multiplicative = T_out (true) / T_out (pred)
```

(Todo: read some papers about whether to use additive or multiplicative error or something else)

T_in | H_duty | T_out_true | T_out_pred |R_additive
-- | -- | -- | -- |  --

We can then learn a function for the residual based on T_in and H_duty:

```
F_R (T_in, H_duty) = R
```

and this can be used in the model, i.e we add the constraint:

```
T_out_adjusted = T_out_pred + F_R(T_in, H_duty)
```

## Limitations

We consider F to be a highly accurate physical property calculation. This residual approach may result in a more accurate temperature, but the relationship between H_out is still defined based on the less accurate T_out_pred temperature. If we trust the prediction for t_out, we should be able to calculate a more accurate H_out using the relationship `F(H_out_adjusted) = T_out_adjusted`. This is because the equation we are least certain about is `H_in + H_duty = H_out` (because this is a simplification of the heater properties, e.g does not take into account heat loss.)

# An alternative implementation

As we realise the heat balance constraint has the most uncertainty in accuracy, and it has a direct relationship to temperature,it makes more sense to calculate the residual for that constraint. We can add a residual R_H to it:

```
H_in + H_duty = H_out + R_H
```

This has a more physical meaning: it represents the heat lost to the environment. We can then use this in our model. To calculate R_H for each test case, we fix T_in, H_duty, and T_out and solve for R_H. Then, we can learn a function for R_H based on T_in and H_duty. 

As those values for R_H were calculated by fixing T_out, using this value of R_H will now give more accurate preditions for T_out.

Fundamentally, the only thing we have changed is the location of the residual.

# Other notes

This basically shows that updating a variable based on a residual can be implemented by adding another variable to represent the variation into the formulation, and training a model to predict that.  The main difficulty is in figuring out where in your system to add the flexibility to represent the variation. Once a variable has been added to represent the residual, there is fundamentally no difference between learning a residual and a new property.

Machine learning can also be seen as a more advanced version of parameter estimation, where instead of learning a constant value for a parameter/variable, a function for the parameter/variable is learned instead.
