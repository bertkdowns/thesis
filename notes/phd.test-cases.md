---
id: dmdj4zdlcrilmijb4w8dsdm
title: Test Cases
desc: ''
updated: 1753159820183
created: 1753054617322
---

For now, I have three different test cases to look at:


# Tennessee Eastman Process

The good thing about the tennessee eastman process is that I have a simulator that I can get data from. Thus, I can try model it and then I can immediately see how well my model is at predicting everything. I'm mostly gonna treat it as a black-box problem and try model each part until I have a model that is good enough for model predictive control.



# IDAES HDA Flowsheet

The good thing about this is that it's fully IDAES, so I already have a model. However, I can look at turning the steady-state model into a dynamic model and running control systems on it. Plus, we can't implement it in the platform yet, so I can work on doing whatever else is required to make it work in the platform. That can give some ideas on what to work on next.


# Geothermal Data

I have a flowsheet that I can build myself relatively easy, and there shouldn't be too much dynamics. But as this is real data it is a good test case for data cleaning methods, trying to find something that can learn well from data. Plus, I can do physics informed stuff to try and get a model that better reflects real-world performance - the adaptive layer that I talked about in my literature review.

# Heating system

Another project I have is modelling a heating system, to see if there is retrofit opportunities to move away from gas to heat pump heating. It's hard to know where that will go, but it could be helpful.