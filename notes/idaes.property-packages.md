---
id: xb8aud8n56489624sotyoq7
title: Property Package Structure
desc: ''
updated: 1773700645238
created: 1773694496771
---

![Three classes are needed to define a property package in IDAES: the ParameterBlock, the indexed StateBlock, and the StateBlockData.
The most important methods we usually need to implement are highlighted in bold italics.](assets/property_package_components.drawio.svg)

Property packages define the thermophysical properties of a fluid in a reusable manner. They are one of the crucial building blocks to create unit models in IDAES.

This article shows a basic overview of property packages, using the Watertap Seawater property package as a simple example. For reference, please look at:

- The [Watertap Seawater Property Package Source code](https://github.com/watertap-org/watertap/blob/8cce4013f9f9b137f8aa995da6e59e258319e8b7/watertap/property_models/seawater_prop_pack.py#L72)
- Watertap's [Documentation of the Seawater Property Package](https://watertap.readthedocs.io/en/1.5.0/technical_reference/property_models/seawater.html)

I'm going to start from the most important part, the StateBlockData, as that actually is where you reference the temperature and pressure and other thermophysical properties, and then explain the other parts from there.

# StateBlockData class

The most important part of the StateBlockDataClass looks like this:

```python
@declare_process_block_class("SeawaterStateBlock", block_class=_SeawaterStateBlock)
class SeawaterStateBlockData(StateBlockData):
    """A seawater property package."""

    def build(self):
        """Callable method for Block construction."""
        super().build()

        # Add state variables
        self.flow_mass_phase_comp = Var(
            self.params.phase_list,
            self.params.component_list,
            initialize={("Liq", "H2O"): 0.965, ("Liq", "TDS"): 0.035},
            units=pyunits.kg / pyunits.s,
            doc="Mass flow rate",
            ...
        )

        self.temperature = Var(
            initialize=298.15,
            bounds=(273.15, 1000),
            units=pyunits.K,
            doc="Temperature",
        )

        self.pressure = Var(
            initialize=101325,
            bounds=(1e3, 5e7),
            units=pyunits.Pa,
        )
```

The `@declare_process_block_class` bit is basically preamble to register it as a component in IDAES. Make sure you reference the correct block_class here that this is for (we'll discuss that next).

When this block is created in IDAES, the  `build()` method is called. This gives the model a chance to set up all the variables and equations it needs. 

The most important thing to do here is to create the "state variables". These are the variables that are generally set to fully define the model's state. Usually, this is flow rate, temperature, and pressure, but you can change this (e.g using enthalpy instead of pressure, or specifying total flow and component ratio). Some discussion on different state variable choices is given [here](https://idaes-pse.readthedocs.io/en/2.8.0/explanations/components/property_package/general/state_definition.html) (but note it is in the context of idaes's [Modular Property Package Framework](https://idaes-pse.readthedocs.io/en/2.8.0/explanations/components/property_package/general/index.html))

In theory, you don't need to add anything more than the state vars! But, most unit operations expect some other properties to be avaliable as well, such as:

- `enth_mol` Molar Enthalpy
- `entr_mol` Molar Entropy
- `mole_frac_comp` Molar fraction of each components
- `mole_frac_phase_comp` Molar fraction of each component in each phase

etc.

You could define the variables and equations/expressions for these in the build method as well. IDAES also has a build_on_demand system where these can be declared in seperate methods, and they'll only get added to the model if they are used. For example, here's the method to define `flow_vol_phase`:

```python
    def _flow_vol_phase(self):
        self.flow_vol_phase = Var(
            self.params.phase_list,
            initialize=1,
            bounds=(0.0, None),
            units=pyunits.m**3 / pyunits.s,
            doc="Volumetric flow rate",
        )

        def rule_flow_vol_phase(b, p):
            return (
                b.flow_vol_phase[p]
                == sum(b.flow_mass_phase_comp[p, j] for j in b.params.component_list)
                / b.dens_mass_phase[p]
            )

        self.eq_flow_vol_phase = Constraint(
            self.params.phase_list, rule=rule_flow_vol_phase
        )
```

Note that when it defines the new property, it also defines the constraint - they go together.

# StateBlock class.

The StateBlock class is like the "housing" for the StateBlockData. IDAES allows models to be indexed by time (and space) so the stateBlock class holds potentially many StateBlockData objects inside it. Often, we are doing 0D simulation at steady state, so there is only one StateBlockData per state block, but we have to be aware of the difference to avoid some common errors.

By convention, the initialize() method lives on the StateBlock. (I think the main reason is just that it's easier to initialise them all at once.)

# ParameterData class

This doesn't have much in it, the main things are:


- `_state_block_class` is a reference to the `StateBlock` class
- the build method defines the components and phases, and any constant parameters.
- the define_metadata method tells `build_on_demand` which properties can be dynamically created (that you wrote methods for in the StateBlockData)

```python
@declare_process_block_class("SeawaterParameterBlock")
class SeawaterParameterData(PhysicalParameterBlock):
    """Parameter block for a seawater property package."""

    CONFIG = PhysicalParameterBlock.CONFIG()

    def build(self):
        """
        Callable method for Block construction.
        """
        super(SeawaterParameterData, self).build()

        self._state_block_class = SeawaterStateBlock

        # components
        self.H2O = Solvent()
        self.TDS = Solute()

        # phases
        self.Liq = LiquidPhase()

        # defining a constant, e.g molar weight of each component
        self.mw_comp = Param(
            self.component_list,
            initialize={
                "H2O": 18.01528e-3,
                "TDS": 31.4038218e-3,
            },
            units=pyunits.kg / pyunits.mol,
            doc="Molecular weight",
        )
    ...

        @classmethod
    def define_metadata(cls, obj):
        """Define properties supported and units."""
        obj.add_properties(
            {
                "flow_mass_phase_comp": {"method": None}, # these are defined in the build method so it is not needed
                "temperature": {"method": None},
                "pressure": {"method": None},
                "mass_frac_phase_comp": {"method": "_mass_frac_phase_comp"},
                "dens_mass_phase": {"method": "_dens_mass_phase"},
                "flow_vol_phase": {"method": "_flow_vol_phase"},
        ...
```



For a more comprehensive explanation on the makeup of IDAES property packages, read [IDAES's How-To Guide on Custom Property Packages.](https://idaes-pse.readthedocs.io/en/2.8.0/how_to_guides/custom_models/property_package_development.html)