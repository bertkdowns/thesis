---
id: fm9l95e6zq0f6zhgl4inx9q
title: Direct_steam_injection
desc: ''
updated: 1746510046414
created: 1746402348520
---

# The Goal

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

We then create a state block where the flows of both inlets are combined, but the temperature and pressure remains the same as it was in the inlet `properties_milk_in`. This calculates the amount of enthalpy if `properties_steam_in` was at the same temperature and pressure. 

We then calculate how much enthalpy to add. To do this, we create a new state block with the steam property package, with the temperature and pressure of the milk inlet, but the composition of the steam inlet. We can then calculate the difference in enthalpy between these two. 

In theory, the amount of enthalpy to cool the steam to the same temperature as the other inlet is the same as the about of enthalpy added to the system compared to if the steam was the same temperature. So, we add the enthalpy difference to the enthalpy from `properties_mixed_unheated` to find the total enthalpy, and this should be the enthalpy of the outlet. We can use the same temperature and pressure as before. 


# The Problem

it's not calculating this correctly.  There are a few potential reasons:

 - I'm not sure if degrees of freedom match - I thought it would be fine to have define_state=false, but I get two degrees of freedom if I don't set define_state=false in the two non-inlet milk state blocks. 
 - Even though the temperature, pressure, and flow is identical between the steam_in and steam_cooled, the enthalpy difference is massive!

Below is an example output

```
milk_in
Flow (mol): 1
Temperature (K): 300.0
Pressure (Pa): 101325
Enthalpy (mol): -282833.54409501614
Enthalpy (mol, Liquid Phase): -282833.6412636071
Enthalpy (mol, Vapor Phase): -241763.72173727475
Mole Fraction (Liquid Phase, Water): 0.9899999752662261
Mole Fraction (Vapor Phase, Water): 0.9999999867374979
Mole Fraction (Liquid Phase, Milk): 0.010000017001854853
steam_in
Flow (mol): 1
Temperature (K): 299.99999999999915
Pressure (Pa): 101325
Enthalpy (mol): 2029.3553673100446
Enthalpy (mol, Liquid Phase): 2029.3553673103027
Enthalpy (mol, Vapor Phase): 45936.238860469515
Mole Fraction (Liquid Phase, Water): 1.0
Mole Fraction (Vapor Phase, Water): 0.0
steam_cooled
Flow (mol): 1.0
Temperature (K): 300.0
Pressure (Pa): 101325.0
Enthalpy (mol): 2555.656348572762
Enthalpy (mol, Liquid Phase): 2555.6563485727634
Enthalpy (mol, Vapor Phase): 46163.37340575213
Mole Fraction (Liquid Phase, Water): 1.0
Mole Fraction (Vapor Phase, Water): 0.0
steam_delta_h
Delta H: -526.3009812627174
mixed_unheated
Flow (mol): 2.000000001128657
Temperature (K): 300.0
Pressure (Pa): 101325.0
Enthalpy (mol): -284261.94370477996
Enthalpy (mol, Liquid Phase): -284262.1013912604
Enthalpy (mol, Vapor Phase): -241763.72334079517
Mole Fraction (Liquid Phase, Water): 0.9949999797856206
Mole Fraction (Vapor Phase, Water): 0.999999993370091
Mole Fraction (Liquid Phase, Milk): 0.005000014655482271
output
Flow (mol): 2.0000000017218085
Temperature (K): 296.4917010733296
Pressure (Pa): 101325.0
Enthalpy (mol): -284525.0942194751
Enthalpy (mol, Liquid Phase): -284525.2566993139
Enthalpy (mol, Vapor Phase): -241881.5678078452
Mole Fraction (Liquid Phase, Water): 0.9949999786951605
Mole Fraction (Vapor Phase, Water): 0.9999999923650749
Mole Fraction (Liquid Phase, Milk): 0.005000014856216085
```


IDAES model_diagnostics gives us some weird results. Howe much properties_steam_cooled.enth_mol is underdefined, but set_temperature, which constrains steam_cooled.temperature, is overdefined? shouldn't hthey cancel out?

```
====================================================================================
ERROR: Units problem with expression 300.0 -
fs.dsi.properties_milk_in[0.0].temperature
====================================================================================
The following component(s) have unit consistency issues:

    fs.dsi.set_steam_cooled[0.0]

For more details on unit inconsistencies, import the assert_units_consistent method
from pyomo.util.check_units
====================================================================================
====================================================================================
Dulmage-Mendelsohn Under-Constrained Set

    Independent Block 0:

        Variables:

            fs.dsi.properties_steam_cooled[0.0].enth_mol
            fs.dsi.properties_steam_cooled[0.0].flow_mol
            fs.dsi.properties_out[0.0].temperature
            fs.dsi.properties_out[0.0]._t1_Vap_Liq
            fs.dsi.properties_out[0.0]._teq[Vap,Liq]
            fs.dsi.properties_out[0.0].mole_frac_phase_comp[Liq,water]
            fs.dsi.properties_out[0.0].flow_mol_phase[Liq]
            fs.dsi.properties_out[0.0].mole_frac_comp[water]
            fs.dsi.properties_out[0.0].mole_frac_phase_comp[Vap,water]
            fs.dsi.properties_out[0.0].mole_frac_phase_comp[Liq,milk_solid]
            fs.dsi.properties_out[0.0].phase_frac[Liq]
            fs.dsi.properties_out[0.0].flow_mol_phase[Vap]
            fs.dsi.properties_out[0.0].mole_frac_comp[milk_solid]
            fs.dsi.properties_out[0.0].temperature_bubble[Vap,Liq]
            fs.dsi.properties_out[0.0]._mole_frac_tbub[Vap,Liq,water]
            fs.dsi.properties_out[0.0].phase_frac[Vap]

        Constraints:

            fs.dsi.set_composition[0.0,water]
            fs.dsi.eq_energy_balance[0.0]
            fs.dsi.properties_out[0.0]._t1_constraint_Vap_Liq
            fs.dsi.properties_out[0.0]._teq_constraint_Vap_Liq
            fs.dsi.properties_out[0.0].equilibrium_constraint[Vap,Liq,water]
            fs.dsi.eq_mass_balance_out[0.0,water]
            fs.dsi.properties_out[0.0].component_flow_balances[water]
            fs.dsi.properties_out[0.0].sum_mole_frac
            fs.dsi.eq_mass_balance_out[0.0,milk_solid]
            fs.dsi.properties_out[0.0].phase_fraction_constraint[Liq]
            fs.dsi.properties_out[0.0].total_flow_balance
            fs.dsi.properties_out[0.0].component_flow_balances[milk_solid]
            fs.dsi.properties_out[0.0].eq_temperature_bubble[Vap,Liq]
            fs.dsi.properties_out[0.0].eq_mole_frac_tbub[Vap,Liq,water]
            fs.dsi.properties_out[0.0].phase_fraction_constraint[Vap]

====================================================================================
====================================================================================
Dulmage-Mendelsohn Over-Constrained Set

    Independent Block 0:

        Variables:


        Constraints:

            fs.dsi.set_steam_cooled[0.0]

====================================================================================
====================================================================================
8 WARNINGS

    fs.dsi.eq_energy_balance[0.0]: Potential evaluation error in (1 - fs.dsi.properties_out[0.0].temperature/fs.milk_properties.water.dens_mol_liq_comp_coeff_3)**fs.milk_properties.water.dens_mol_liq_comp_coeff_4; base bounds are (-0.5452845641524886, 0.5779055213017478); exponent bounds are (0.081, 0.081)
    fs.dsi.eq_energy_balance[0.0]: Potential evaluation error in (1 - fs.dsi.properties_out[0.0].temperature/fs.milk_properties.milk_solid.dens_mol_liq_comp_coeff_3)**fs.milk_properties.milk_solid.dens_mol_liq_comp_coeff_4; base bounds are (-0.5452845641524886, 0.5779055213017478); exponent bounds are (0.081, 0.081)
    fs.dsi.eq_energy_balance[0.0]: Potential evaluation error in (1 - fs.dsi.properties_mixed_unheated[0.0].temperature/fs.milk_properties.water.dens_mol_liq_comp_coeff_3)**fs.milk_properties.water.dens_mol_liq_comp_coeff_4; base bounds are (-0.5452845641524886, 0.5779055213017478); exponent bounds are (0.081, 0.081)
    fs.dsi.eq_energy_balance[0.0]: Potential evaluation error in (1 - fs.dsi.properties_mixed_unheated[0.0].temperature/fs.milk_properties.milk_solid.dens_mol_liq_comp_coeff_3)**fs.milk_properties.milk_solid.dens_mol_liq_comp_coeff_4; base bounds are (-0.5452845641524886, 0.5779055213017478); exponent bounds are (0.081, 0.081)
    fs.dsi.eq_energy_balance[0.0]: Potential division by 0 in ((fs.dsi.properties_steam_in[0.0].enth_mol - fs.dsi.properties_steam_cooled[0.0].enth_mol)*fs.dsi.properties_steam_in[0.0].flow_mol)/fs.dsi.properties_mixed_unheated[0.0].flow_mol; Denominator bounds are (0, 100)
    fs.dsi.properties_milk_in[0.0].equilibrium_constraint[Vap,Liq,water]: Potential division by 0 in fs.milk_properties.water.pressure_sat_comp_coeff_B/(fs.dsi.properties_milk_in[0.0]._teq[Vap,Liq] + fs.milk_properties.water.pressure_sat_comp_coeff_C); Denominator bounds are (-inf, inf)
    fs.dsi.properties_mixed_unheated[0.0].equilibrium_constraint[Vap,Liq,water]: Potential division by 0 in fs.milk_properties.water.pressure_sat_comp_coeff_B/(fs.dsi.properties_mixed_unheated[0.0]._teq[Vap,Liq] + fs.milk_properties.water.pressure_sat_comp_coeff_C); Denominator bounds are (-inf, inf)
    fs.dsi.properties_out[0.0].equilibrium_constraint[Vap,Liq,water]: Potential division by 0 in fs.milk_properties.water.pressure_sat_comp_coeff_B/(fs.dsi.properties_out[0.0]._teq[Vap,Liq] + fs.milk_properties.water.pressure_sat_comp_coeff_C); Denominator bounds are (-inf, inf)
```

Here is the current build method:


```python
def build(self):
    # build always starts by calling super().build()
    # This triggers a lot of boilerplate in the background for you
    super().build()

    # This creates blank scaling factors, which are populated later
    self.scaling_factor = Suffix(direction=Suffix.EXPORT)


    # Add state blocks for inlet, outlet, and waste
    # These include the state variables and any other properties on demand
    # Add inlet block
    tmp_dict = dict(**self.config.property_package_args)
    tmp_dict["parameters"] = self.config.property_package
    tmp_dict["defined_state"] = True  # inlet block is an inlet
    self.properties_milk_in = self.config.property_package.state_block_class(
        self.flowsheet().config.time, doc="Material properties of inlet", **tmp_dict
    )

    # We need to calculate the enthalpy of the composition, before adding additional enthalpy from the temperature difference.
    # so we'll add another state block to do that.
    tmp_dict["defined_state"] = False
    tmp_dict["has_phase_equilibrium"] = True
    self.properties_mixed_unheated = self.config.property_package.state_block_class(
        self.flowsheet().config.time, doc="Material properties of mixture, before accounting for temperature difference", **tmp_dict
    )

    # Add outlet block
    tmp_dict["defined_state"] = False
    tmp_dict["has_phase_equilibrium"] = True
    self.properties_out = self.config.property_package.state_block_class(
        self.flowsheet().config.time,
        doc="Material properties of outlet",
        **tmp_dict
    )

    # Add steam inlet block
    steam_dict = dict(**self.config.steam_property_package_args)
    steam_dict["parameters"] = self.config.steam_property_package
    steam_dict["defined_state"] = True  
    self.properties_steam_in = self.config.steam_property_package.state_block_class(
        self.flowsheet().config.time, doc="Material properties of steam inlet", **steam_dict
    )

    
    
    # To calculate the amount of enthalpy to add to the inlet fluid, we need to know the difference in enthalpy between steam at that T and P
    # and steam at its inlet conditions. Note this is assuming that effects of composition (the steam will no longer be pure water) are negligible.
    # Note that this state block is just for calcuating, and not an actual inlet or outlet.
    
    steam_dict["defined_state"] = False  # This doesn't affect pure components.
    steam_dict["has_phase_equilibrium"] = True
    self.properties_steam_cooled = self.config.steam_property_package.state_block_class(
        self.flowsheet().config.time, doc="Material properties of cooled steam", **steam_dict
    )

    # Add ports
    self.add_port(name="outlet", block=self.properties_out)
    self.add_port(name="inlet", block=self.properties_in, doc="Inlet port")
    self.add_port(name="steam_inlet", block=self.properties_steam_in, doc="Steam inlet port")


    # CONDITIONS

    # STEAM INTERMEDIATE BLOCK

    # Temperature (= other inlet temperature)
    @self.Constraint(
        self.flowsheet().time,
        doc="Set the temperature of the cooled steam to be the same as the inlet fluid",
    )
    def set_temperature(b, t):
        return b.properties_steam_cooled[t].temperature == b.properties_in[t].temperature
    
    # Pressure (= other inlet pressure)
    @self.Constraint(
        self.flowsheet().time,
        doc="Set the pressure of the cooled steam to be the same as the inlet fluid",
    )
    def set_pressure(b, t):
        return b.properties_steam_cooled[t].pressure == b.properties_in[t].pressure


    # Flow = steam_flow
    @self.Constraint(
        self.flowsheet().time,
        self.config.steam_property_package.component_list,
        doc="Set the composition of the cooled steam to be the same as the steam inlet",
    )
    def set_composition(b, t, c):
        return 0 == sum(b.properties_steam_cooled[t].get_material_flow_terms(p, c) - b.properties_steam_in[t].get_material_flow_terms(p, c)
            for p in b.properties_steam_in[t].phase_list)
    
    
    # CALCULATE ENTHALPY DIFFERENCE
    @self.Expression(
        self.flowsheet().time,
    )
    def delta_h(b, t):
        """
        Calculate the difference in enthalpy between the steam inlet and the cooled steam.
        This is used to calculate the amount of enthalpy to add to the inlet fluid.
        """
        return (b.properties_steam_in[t].enth_mol - b.properties_steam_cooled[t].enth_mol) * b.properties_steam_in[t].flow_mol


    # MIXING (without changing temperature)

    # Pressure (= inlet pressure)
    @self.Constraint(
        self.flowsheet().time,
        doc="Equivalent pressure balance",
    )
    def eq_pressure_balance_mixed(b, t):
        return (
            b.properties_mixed_unheated[t].pressure
            == b.properties_in[t].pressure
        )
    
    # Temperature (= inlet temperature)
    @self.Constraint(
        self.flowsheet().time,
        doc="Equivalent temperature balance",
    )
    def eq_temperature_balance(b, t):
        return (
            b.properties_mixed_unheated[t].temperature
            == b.properties_in[t].temperature
        )
    
    # Flow = inlet flow + steam flow
    @self.Constraint(
        self.flowsheet().time,
        self.config.property_package.component_list,
        doc="Mass balance",
    )
    def eq_mass_balance(b, t, c):
        return (
            0 == sum(b.properties_in[t].get_material_flow_terms(p, c)
                        + (b.properties_steam_in[t].get_material_flow_terms(p, c)
                        if c in b.properties_steam_in[t].component_list  # handle the case where a component isn't in the steam inlet (e.g no milk in helmholtz)
                        else 0)
                - b.properties_mixed_unheated[t].get_material_flow_terms(p, c)
                for p in b.properties_in[t].phase_list
                if (p,c) in b.properties_in[t].phase_component_set) # handle the case where a component is not in that phase (e.g no milk vapor)
        )

    # OUTLET BLOCK

    # Pressure (= inlet pressure)
    @self.Constraint(
        self.flowsheet().time,
        doc="Pressure balance",
    )
    def eq_pressure_balance(b, t):
        return (
            b.properties_out[t].pressure
            == b.properties_in[t].pressure
        )
    
    # Enthalpy (= mixed enthalpy + delta steam enthalpy)
    @self.Constraint(
        self.flowsheet().time,
        doc="Energy balance",
    )
    def eq_energy_balance(b, t):
        return (
            b.properties_out[t].enth_mol
            == b.properties_mixed_unheated[t].enth_mol
            + (b.delta_h[t] / b.properties_mixed_unheated[t].flow_mol)
        )
    
    # Flow = mixed flow
    
    @self.Constraint(
        self.flowsheet().time,
        self.config.property_package.component_list,
        doc="Mass balance for the outlet",
    )
    def eq_mass_balance_out(b, t, c):
        return (
            0 == sum(b.properties_out[t].get_material_flow_terms(p, c)
                        - b.properties_mixed_unheated[t].get_material_flow_terms(p, c)
                for p in b.properties_out[t].phase_list
                if (p,c) in b.properties_out[t].phase_component_set) # handle the case where a component is not in that phase (e.g no milk vapor)
        )

```

