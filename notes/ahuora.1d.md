

# 1d modelling

How are we even gonna do that in the platform?
Do we want to just give some hyperparameters, e.g overall heat transfer, but then it's simulated on a more granular level?

# 1d pipe 

1d modelling in dynamics is good for simulating delay. We could add a pipe (basically equivalent to a bunch of heaters in a row) to simulate a system that takes a while to respond to changes. - pretty much first order plus time delay.

Maybe we also just want to be able to specify a transfer function, e.g for time delay. This might be helpful for modelling other dynamics. This could be a state block? Like how aspen hysis has a transfer function option.

# 1d heat exchanger

It is also good for modelling heat transfer. In a counter-current heat exchanger, there is not a consistent temperature driving force the whole way across. 

![Temperature driving force across a countercurrent heat exchanger](assets/temperature_driving_force.drawio.svg)

in the places where there is a higher temperature driving force, more heat will be exchanged. In the places where there is a lower temperature driving force, not much heat will be exchanged. So having a 1d heat exchanger means we can model heat exchange much more accurately.


# General observations

For now, we can consider that each timestep is basically the same. That is, for a pipe or heat exchanger, we can have the same volume segments, and the same heat transfer coefficient across each.

We'll have to make a way of showing the properties across a bunch of state blocks eventually, but for now we dont even need to do that, we can model it without the user showing the internals.

![Heat Exchanger 1d](assets/heat_exchanger_1d.drawio.svg)