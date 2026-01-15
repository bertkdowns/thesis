---
id: 3co7slkgealz5qexf6nnwmq
title: Biggest Bottlenecks
desc: ''
updated: 1768450689812
created: 1768445768989
---


The way I see it, there are four main bottlenecks to our stuff being used, and DTs being adopted more.

They are presented here, in order of largest-smallest.

# What in the world do we actually use them for?

Okay, so digital twins obviously can be good for performance modelling, fault analysis, real-time decision making, training, etc, but *how*?

Some more concrete ways can be:

- Providing an up-to-date model for model predictive control
- Providing better visibility into the system (calculating what normally isn't calculated)
- Providing an up-to-date model to manually optimise to test out other potential scenarios
- Providing a virtual copy that you can do whatever you like to (overriding live inputs) and see what happens

But even these are not particularly specific. Obviously, that's partly because we are talking in very general terms here. 

# Getting data in the right format

We get data coming in via modbus, serial, third party data aquisition (DAQ) devies, PLCs, etc. This can be a pain and sometimes it is not accessible without significant work.

It can end up going through a bunch of different formats. You might then send that data to a relational or time-series database, e.g postgres or InfluxDB or clickhouse, and that might take another protocol or two (http, sql, MQTT, etc).

This is a lot of work and a lot of different protocols to get through, and makes it much easier for something to go wrong. It would be nice to have a better solution but I don't know how to do that (other than enforcing e.g everything uses MQTT, or everything uses OPC-UA, or something. A standard toolkit for me would be good.)

Another big thing around this is repeatability. How can we test our system? Can we just replay all MQTT events of data coming in from sensors - is that an appropriate level, effecively virtualising our factory from there up? Should we use our DT model to emulate the data of those sensors, sending MQTT events from 

# The solver not always solving

Sometimes, initialisation fails. Often the solver gets stuck in a loop especially if an initial guess is wrong, and then the initialisation values are way off. This is a major cause of failed solves.

Other times, the model might be specified a bit wrong. For example, if you set the heat transfer coefficient of a heat exchanger and the area, it will assume that *that much* heat is transferred. However, if the fluid flow is slower or colder, it sometimes doesn't have enough heat to transfer and fails. Even if you set the outlet temperature instead of heat transfer coefficient, it could be that the stream is too hot or cold to even get there. Better debugging for this kind of thing would be really good too.


Some ideas to solve this:

- Initialise everything that hasn't been initialised before, but if it has been initialised before, don't try initialise again. It's not worth it. This includes if you have any saved values from previous solves - if we can load these in, don't try initialise. (there might be some cases where this is a bad idea though, we might need to do a better study of this.)
- Add some manual diagnostics checks to objects. E.g on every idaes unit op we could add a debug() function, that uses whatever current values the solver has given and tries to explain them. For example, in the heat exchanger it could check and say "One fluid has a lot of residual heat, but the other one doesn't have enough, possibly this is a problem". This could get very hard to do though as there are so many potential problems, and it is not very specific. Alternatively, this could be done in conjunction with some of the diagnostic toolboxes recommendations.

# Dealing with missing data in a first principles model

I'm trying to build a first principles model, and then I realise I only have temperature data, not pressure. I can sometimes estimate, but not always. OR i'm not sure what the efficiency curves are.

Some of this could potentially be calculated by regressing/parameter estimation from a bunch of past results. However, we don't have a good system to do that.  We need to come up with one.


