---
id: 21zlgfm0oqp4nrdpaw2m449
title: Data Driven Methodologies
desc: ''
updated: 1777351912898
created: 1777337025020
---

A summary of data driven methodologies and when to use each.

# Tools

Tool| Key strengths | Cons
 --- | ---
Paramest | Parameter Estimation, Data Reconcilliation | To learn more complex relationships you need to define the function yourself
PySMO | Super easy to use, works well out of the box | Doesn't scale to large datasets
OMLT | Integrates well with other ML tools e.g Pytorch, good for big data | 
ALAMO | Symbolic regression, more interpretable | Requries a license to use

## Paramest



## Pysmo

## OMLT

## ALAMO

# Tasks

## Data Reconcilliation

*Example: I have flow readings in and out of this splitter. In theory flow in = flow out, but due to measurement inaccuracy they don't quite equal.*

Paramest can do this just fine. The other tools are not really designed for this.

## Estimating a parameter

*Example: I have a compressor model that requires isentropic efficiency to be specified. I want to estimate its isentropic efficiency from the pressure, temperature, and electrical work*

Paramest is basically designed for this.

Other examples:

- Estimating the activity coefficients of a fluid in an activity coefficient model 

## Learning a new property

*Example: I have some data on compressor speed and mechanical work. I want to add to my first principles compressor model and estimation of the speed based on mechanical work.*

Here, you could use Pysmo, OMLT, or ALAMO, depending on what you want.

Want something that just works? Use Pysmo.

Want a somewhat readable equation to represent the relationship? Use ALAMO.

Want to train off a lot of data or have a complex pipeline? Use OMLT with pytorch or another ML framework of your choosing.

In theory, you could construct a function to represent the relationship and then use paramest to learn the unknowns in the function (e.g predict Y from x with y=mx+c, paramest learns best value for m and c) but that's probably overkill. The main case would be for if you know the physical equation you want to model.

## Residual Learning

*Example: I have a compressor model that isn't quite accurate enough. I want to use my data to make my first principles model behave more like the physical compressor.*

See [idaes.residual-learning]. Your thought process would probably go something like this:

> The physical model is predicting a higher pressure than I expect if the mechanical work is high.
>
> This means the efficiency of the compressor must be worse if mechanical work is high.
>
> Let's use a physical model that includes efficiency, and then machine-learn what the appropriate efficiency value is.
>
> Parameter estimation isn't appropriate as efficiency is lower at higher pressures. So we will learn the relationship between inlet pressure and efficiency.
>
> We use data reconcilliation to estimate efficiency based on input and output pressures and mechanical work data, for each case seperately. Then we train an ML model (Pysmo, ALAMO, or OMLT) to predict efficiency based on the input pressure and mechanical work.

Other examples
Activity coefficient property packages where the deviation (activity coefficients) change based on temperature/pressure

## State identification

e.g, trying to figure out the amount of holdup/capacitance in the system, or other dynamic variable's start points. Often this is hard/impossible to measure: Its easy to measure the level of a tank, but it's hard to measure how much residual heat is in a heat exchanger.

One way this could be done is through parameter estimation, where we estimate the initial values of dynamic variables, based on what we see the system do after that.
(see Transient 1D heat exchanger model)