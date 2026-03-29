---
id: scvp1mdjtp0k66c9a345im4
title: Crystalliser
desc: ''
updated: 1774385559480
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


The crystalliser does a lot of work, the property package doesn't do VLE and LLE calculations, which can cause problems in some unit operations that expect that:

This leads to degrees of freedom like the following:

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

This can be fixed with setting the heater to `material_balance_type="componentPhase"`.

We might need to set the property packages default material balance type to fix this. However, note that this won't do the equilibrium calculations at all - it relies on the unit operation to do that.