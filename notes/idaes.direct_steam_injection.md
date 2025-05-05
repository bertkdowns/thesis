---
id: fm9l95e6zq0f6zhgl4inx9q
title: Direct_steam_injection
desc: ''
updated: 1746413447588
created: 1746402348520
---

# Problem

We want to do direct steam injection with a milk property package. the issue is that the milk property package can't handle pure water well, especially at high temperatures. So, our solution is to make a mixer-translater that translates the stream into the milk property package and mixes it simultaneously. Once they're mixed, because it's no longer pure water, the milk property package will be fine.

The main problem with translating between property packages is that we don't know if the reference enthalpy is the same. E.g 0 enthalpy could be at 1 atm 25 degrees celsius, or 1 atm 0 degrees celsius, etc. 

However, in theory, there are only two things that we care about the steam:

- How much additional heat (enthalpy) it adds to the stream
- How much additional water it adds to the stream

![High Level View of Direct Steam Injection](assets/direct_steam_injection.drawio.svg)

We are assuming that all the pressure of the steam is lost, i.e the pressure of the `milk_solids + water` is maintained.

We are trying to make it work without needing both property packages to have the same reference enthalpy. Instead, we can just calculate the change in heat energy in the stream between those two conditions, to figure out how much energy is extra.

![Structure of Direct Stream Injection Unit Operation - State Blocks](assets/direct_steam_injection_structure.drawio.svg)

`properties_milk_in` and `properties_steam_in` are the inlet state blocks, where the temperature, pressure, and flow rates can be considered to be set by an upstream process. (in reality, it doesn't always work like that, you can back-calculate, but for illustrative purposes it's easier to think of constraints as directional.) 

The enthalpy difference is calculated, and added to the enthalpy it would be at if it was unheated.

