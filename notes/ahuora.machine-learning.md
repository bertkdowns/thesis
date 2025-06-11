

# Machine Learning

There's two different ways that we could use machine learning in the platform:

1. Train a surrogate model based on our first principles simulations, to make a full ML digital twin that we can use in a plant
2. Make ML models of unit ops that we don't have, so we can use them in a first principles simulation.

the first one, to be honest, seems pretty easy. You've just gotta generate a bunch of data from the platform and make a ML model of it, using literally any ML platform you like.

Though, ideally, you also want it to be able to update based on new data, so maybe you use an adaptive model, something that supports online training. Or you just get  a bunch more data and batch retrain it every now and again.


The second one requires using a library like OMLT or PySMO, something that can convert an ML model to a set of algebraic pyomo equations. I odn't think there's a way to do this for dynamic models yet, so that is a potential research area for me. However, perhaps it makes sense to get them all working in the platform first.


## Parameter Estiamtion

A third use case, that's similar, is parameter estimation. You have a first principles dataset, and you just want to estimate a couple of parameters, e.g efficiency, from a bunch of data that you have, to make it line up as best as you can.


## Which of these use cases is the best one to work on?

It's hard to know without case studies.



# Do we need dynamic machine learning?

If there's not much dynamic effect, e.g the flows are high enough that change is observed pretty much immediately, it's overkill and will generate worse results.

If nearby changes are happening, and it is noticable - e.g a heated tank - then lstms or something might be good.

For things like fouling, a different strategy will be required - maybe include the timestep in your data?
 

# How do we re-learn a model over time? When collecting new data, how do we clean it?

Some of the data from the plant won't be good for training on. So we need some level of anomaly detection to help make the ML models resistant to that kind of thing. That might be a preprocessing step before creating the surrogate model to use.

We can also compare the difference between this ML model and the previous, or long time previous, version, to see how the predictions change over time.

We also need to make some kind of pipeline to do all that, rather than having to manually prepare the data.

# ResNets?

Resnets basically start with the values, and then learn the error each time. I gotta learn more about it, but it seems to work better. 

TODO: read this paper more:

https://www.sciencedirect.com/science/article/pii/S0098135425002157?via%3Dihub



# How can we make a physics informed model?

In that paper, and in the traditional method of doing PINNs, you add to the loss function the material and energy balances. e.g you add loss when the derivatives aren't what is expected. This helps the model fit well outside the training region. Then, you gradually decrease the penalty of the physics part of the loss function as you continue to train the model, so that it better fits the training data. the idea is that it will better fit the training data, without losing all of its knowledge about effects outside the training region (because you enforced those effects from the physics equations).


In the Ahuora platform, we have first principles equations for everything. However,  getting this into a format that works as a pytorch loss function might be frustrating. It would be nice if there's a shortcut way of doing it.

One approach is to use the training data to estimate parameters as best as you can. e.g use the temperatures and pressures in the training data to estimate the overall heat transfer coefficient. then you can generate a bunch of "fake" data using the idaes model, including outside the original operating range. Then, you can train the neural network on that first. Ideally, that will make it learn the physics, the general relationships between the variables. then you can gradually add more and more of the real data to fine tune the model, and reduce the amount of data from the surrogate model. The idea is, it will still know the general relationships from when it was trained by the surrogate model.


Normal NN method:

- create neural network strucutre
- train nn on raw data, minimising Mean squared Error


Physics informed NN method:

- create neural network strucutre
- define physical constraints (mass and energy balances etc), and formulate them into a loss/penalty function
- train nn on raw data, minimising the MSE and the Physics informed penalty
- gradually remove the Physics informed penalty as training progresses.



Surrogate Informed NN method:

- create the model in IDAES
- use raw data for parameter estimation of unknowns in model (e.g for heat transfer coefficient on a heat exchanger). This creates a first principles representaiton
- use the first principles idaes model (with the estimated parameters) to generate a bunch of synthetic data (including data outside the training region)
- create neural network structure
- train the neural network based on the synthetic data (making a surrogate model)
- fine-tune the neural network based on the real data.