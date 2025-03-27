# LTI Systems

Linear Time Invariant systems are common in signal processing.

IDK if the systems we're modelling are linear or not, but time invariance is important.

In dynamic modelling, the conditions of the outputs at time point t are influenced by all the previous values of the input function, but never by the future values of the input function.

By default, operator networks don't have this constraint, meaning that changing the input function U(t) may actually change the predicted value for V(t-1), even though in the real world that makes no sense - you can't react to changes in the future!

# RQ

What changes need to be made to RBON architecture to constrain it to always model an LTI system?

How does this affect its accuracy in modelling LTI systems?

