---
id: aoeoypqlxm676ot3pjrmc03
title: 1d Proposal
desc: ''
updated: 1757570777638
created: 1757570105102
---


TL; DR: Lets just do read-only 1d properties first, and store them as an array in PropertyValue.


# The Problem

To more accurately simulate certain kinds of systems, 1-D  (and sometimes 2d or 3d) discretisation is very helpful. This cuts the continuous model into a series of smaller areas, and everything is modelled at a higher resolution. This results in more accurate simulations. However, our property value system is not really set up to handle 1d properties: everything is designed around scalar variables.

# Example

A heat exchanger is the simplest example: https://idaes-pse.readthedocs.io/en/stable/reference_guides/model_libraries/generic/unit_models/heat_exchanger_1D.html


This heat exchanger is broken up across it's length with a bunch of smaller state blocks, and a different amount of heat is transferred across at each one. This helps to see if there is actually a temperature driving force the whole way across the heat exchanger or not.

![Heat Exchanger 1d](assets/heat_exchanger_1d.drawio.svg)


See [[ahuora.1d]] for more info.


# A couple of approaches

## Make a new property value for each discretised point

- that would be a lot of propertyValue entries
- it would allow us to use all the existing DOF replacement stuff, but to replace all of them you'd have to do it one by one (unless we added an abstraction on top)
- you would have to make it add or remove propertyValues when people change the number of segments (similar to adding/removing compounds or outlets)

## Make the existing propertyValue store an array (or multi-dimensional array) for the discretised set

- this would involve less propertyValue entries
- we could use the existing DOF replacement stuff to replace all of them with another property (assuming the other property is discretized at a similar resolution) but we couldn't dof replacement just one section


## Something completely different

Any creative ideas?

# What About...

## DOF Replacement?

If I am setting each individual HTC in a heat exchanger, does it make sense to replace one of these DOFs with something else? Or is it better to treat the entire continuous set together, and you replace all 20 of the discretizations with another variable that is discretized into 20 pices (e.g the temperature profile)

## Entering Individual Values?

Does it ever make sense to enter the HTC profile across the length of the HX? Should the user be able to upload a CSV file or fill in a table for this? Or is it more common to write a constraint or expression for the profile.

I don't think we should worry about this yet, as we're just worrying about the read-only case.

## Setting the Size/Resolution?

This isn't really a propertyValue, it's more a configuration argument. You can't do DOF replacement etc. Maybe we need a new table to specify the continuous set configuration.

## 2D/3D cases?

We need to support multi-dimensional arrays, and some way of specifying the order. Ideally this should be the same as IDAES

## Time?

We currently don't display any time-based results other than in the solution table. This feels like we have two different ways of doing a similar thing. We might need to combine them at some point.

## How we combine with normal indexed items?

How do we know the order of things, if you are indexing by mole_frac_comp, at time 0, for compound benzene, for section 4 of the HX? How does IDAES_factory know which index to put first?

Is having two different ways of indexing compounds with separate property values, vs sections with the same property value, a bad idea?

## How do we load the data back into the propertyValue?

This is more of an implementation question, we just gotta write the code in property_value_adapter.

## How do we set up the config file?

I guess the config for a property could look something like this:

```
properties: {
   ...
   "heat_transfer_coefficient":{
       "displayName": "Heat Transfer Coefficient",
       "type": "numeric",
       "unitType" : "heat_transfer_area",
       "indexSets" : ["discretization"] 
   },
indexSets:[
  {
    "id": "discretization"
    "displayName": "Discretization Resolution"
    "type":"continuous"
   "default_resolution": "10"
  }
]
}
```

We'd have to make a new table for the discretization resolution config options. This would be kinda similar to how property packages work right now.


## How do we display it in the frontend?

I guess we just have a graph or table or something.



