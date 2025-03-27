# Background

Surrogate modelling is pretty easy to do in steady state systems using PySMO or OMLT. However, dynamic systems also have a time element, which make them a lot more complex to model in terms of parameters. Instead of mapping data-to-data, they map function-to-function. In effect, they are solving an ODE or PDE.

Operator networks do this, but haven't been used for surrogate modelling.

This would extend the benefits of surrogate modelling to dynamic modelling. It enables learning systems we don't have physical equations for, adapting models based on data to represent degradation and changes over time, and a potential speedup of computation on complex systems.


# RQ

How does the method of generating the input functions (U) affect the accuracy in learning V?

How accurately can operator networks act as a surrogate of a complex chemical process in IDAES?

What adjustments need to be made to the RBON to enforce/constrain it to the domain of LTI systems?

# Method


# Results