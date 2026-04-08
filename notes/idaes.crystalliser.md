---
id: scvp1mdjtp0k66c9a345im4
title: Crystalliser
desc: ''
updated: 1775619015040
created: 1774329199047
---


The [crystalliser unit model](https://watertap.readthedocs.io/en/latest/technical_reference/unit_models/crystallizer_0D.html) in watertap works pretty well, but there's a few things to know:

- There's a couple of constants that are used internally that you don't really need. it's okay to fix them to the default values:

```python
crystallizer.crystal_growth_rate.fix()
crystallizer.souders_brown_constant.fix()
crystallizer.crystal_median_length.fix()
```

- The bounds on the crystalliser are quite tight, and don't work well with larger flows. To be honest, you probably won't have larger flows, but it is frustrating if you are testing with random numbers and it fails even though the model is solvable. but you can debug those with the idaes Diagnostics toobox `dt.display_variables_near_bounds()` and relax them:

```python
crystallizer.height_crystallizer.setub(300) # was 25, doesn't work for big ones?
crystallizer.height_slurry.setub(300) # was 25, doesn't work for big ones?
crystallizer.diameter_crystallizer.setub(300) # was 25, doesn't work for big ones?
crystallizer.magma_circulation_flow_vol.setub(1000) # was 100, doesn't work for large crystallizers
crystallizer.work_mechanical.setub(500_000_000) # was 5000_000, doesn't work for large crystallizers
```

# Version 1

[Accessible at WaterTap](https://watertap.readthedocs.io/en/1.6.0/technical_reference/unit_models/crystallizer_0D.html)

This crystalliser uses it's own special property package, which has the following state variables:

```
Temperature
Pressure
flow_mass_phase_comp[Vap,H2O]
flow_mass_phase_comp[Liq,H2O]
flow_mass_phase_comp[Liq,NaCl]
flow_mass_phase_comp[Sol,NaCl]
```
This is a FpcTP formulation. 

The crystalliser does a lot of work, the property package doesn't do VLE and LLE calculations, which can cause problems in some unit operations that expect that.

This leads to degrees of freedom like the following variables in a splitter:

```
Independent Block 1:

    Variables:

        fs.c4_purge.outlet_2_state[0.0].flow_mass_phase_comp[Vap,H2O]
        fs.c4_purge.outlet_2_state[0.0].flow_mass_phase_comp[Liq,H2O]

    Constraints:

        fs.c4_purge.material_splitting_eqn[0.0,outlet_2,H2O]

Independent Block 2:

    Variables:

        fs.c4_purge.outlet_2_state[0.0].flow_mass_phase_comp[Sol,NaCl]
        fs.c4_purge.outlet_2_state[0.0].flow_mass_phase_comp[Liq,NaCl]

    Constraints:

        fs.c4_purge.material_splitting_eqn[0.0,outlet_2,NaCl]
```

The splitter does not know how much of each phase goes to the outlet. By default, it writes constraints to split the amount of salt going to each outlet, and the amount of water going to each outlet - but it doesn't say what phase that salt should be in. 

This can be fixed with setting the splitter/heater/unit op to `material_balance_type="componentPhase"`. This means that it maintains the amount of each phase for both salt and water. This adds 2 extra constraints which fully defines the system.

However, this creates the balance that the amount of material in each phase into the heater is the same as the amount of material in each phase out of the heater - i.e there is no phase change. This doesn't work when you're trying to e.g boil water, as it will just superheat the liquid and it will never evaporate.

# Version 2: VLE on the Property Package

Accessible at commit [d4acbfce8b852ed0beb15ba23d1b0cfa86e1562b](https://github.com/waikato-ahuora-smart-energy-systems/Ahuora-Adaptive-Digital-Twin-Platform/pull/1997/changes/d4acbfce8b852ed0beb15ba23d1b0cfa86e1562b)

Ben created an updated crystalliser model which has VLE and SLE calculations on the property package. The same state variables are used. In this model, phase equilibrium calculations are performed on an intermediate state block before splitting the liquid and vapor up, and then the amount of liquid and vapor in each outlet for each phase is fixed exactly. 
This model does not always have a phase equilibrium though, as the standard material_balance_type is still componentPhase and no phase equilibrium was calculated by default. This meant that a heater could not boil water, and only superheat.

The fix is to set the heater to `has_phase_equilibrium=true`. This means that the heater passes `has_phase_equilibrium=true` to the outlet property package when it is built. Then, you set `material_balance_type=component` and the phase equilibrium calculations decide how much of each phase is present (the ratio of liquid to vapor, and dissolved salt to solid salt). The property package uses the VLE and SLE calculations to figure out the per-phase balance of each from component from the total amount fo each component.

Before building the crystalliser we didn't have to worry about setting has_phase_equilibrium=true, as the other property packages use FTPx so an equilibrium is always calculated as phase information is not passed between arcs anyway.

Finished at [53da4de6623c1cff1dbbfbd778165fb77e18b906](https://github.com/waikato-ahuora-smart-energy-systems/Ahuora-Adaptive-Digital-Twin-Platform/pull/1997)

# Version 2

(https://github.com/waikato-ahuora-smart-energy-systems/Ahuora-Adaptive-Digital-Twin-Platform/pull/2000)

I think using FpcTP is a good formulation as it limits the number of phase equilibium calculations. However, currently we specify FTPx in the platform, and switching that to FpcTP will require adding feed blocks to the inlets to calculate the initial phase equilibrium. Additionally, not all unit operations support setting has_phase_equilibrium on the outlets, so the fix that worked for the heater will not work for everything.

So instead, I updated the formulation's default material_balance type to component instead of componentPhase, and made it always calculate the phase equilibrium the same as everything else. This makes it more similar to the existing FTPx property packages, and works a lot nicer. However, as the crystalliser was built with the idea of disabling phase equilibrium, I also had to update the crystallizer model so that phase equilibrium could be calculated on each of the outlets. This was done by getting rid of the central phase_equilibrium block, and only constraining that there was no solid or liquid in the vapor outlet, and no vapor in the liquid outlet. The phase equilibrium for the salt vle in the vapor outlet thus is still present but irrelevant, and the vles for the water determined how much went to each outlet.