# Switching to FPhx

FPhx is an alternative state definition that is supported by the modular property packages framework in idaes [(Docs here)](https://idaes-pse.readthedocs.io/en/2.5.0/explanations/components/property_package/general/state_definition.html).

Our [initial implementation](https://github.com/waikato-ahuora-smart-energy-systems/PropertyPackages/releases/tag/v0.0.1) of Peng-Robinson using Chemsep properties and equation formulations used FTPx as the state definition. However, this causes problems because of the vapour fraction region, where temperature does not change as enthalpy increases (see [001_ph_formulation](./001_ph_formulation)). FTPx in idaes simply cannot represent pure components in this region, because it has no way of knowing "where" in that flat region the component is.  This is not a problem in mixtures, because mixtures increase in temperature as they boil (i think due to intermolecular interactions ,and  one component boiling at a lower temperature to the other)

Hence FPhx is a better choice, because enthalpy is continuous. However, there were some problems initially with implementing FPhx, because IDAES wasn't building the parameters in the right order. This was fixed in an update to IDAES. See [this IDAES Discussion](https://github.com/IDAES/idaes-pse/discussions/1452).

There also have been some other issues in the switch:

# Relative Enthalpies

Enthalpy is a relative measure, unlike temperature which is absolute. This means we have to be careful when switching between property packages - we should add constraints that the temperature is the same, rather than that the enthalpy is the same (though maybe that would cause problems with vapour regions of pure components). However, since all our data is from chemsep, the reference point for enthalpy should all be the same, so we might get away with it.

# Constraints

Just like in [009_idaes_initialisation](./009_idaes_initialisation.md), we need to extend/inherit the state block to support constraints for other properties. However, unlike the Helmholtz Property package, Temperature is still a variable even though the state definition doesn't include temperature. So the initialisation code must be modified to handle extra variables being fixed, in the same way that extra constraints can be fixed. The file [modular_extended.py](https://github.com/waikato-ahuora-smart-energy-systems/PropertyPackages/blob/332a9909b405583ccd996f8bbb0bcf39fcb1c7fb/property_packages/modular/modular_extended.py) was added to handle this. (Note that the initialise method has a lot of extra print statements to try help with debugging)

# Bounds
State definition parameters require  bounds to be set. After trial and error, bounds were set to include the minimum and maximum enthalpy that was needed to get the tests to pass. However, this probably messes up the scaling of other problems - some compounds appear to have quite low enthalpies, and some have much higher enthalpies.

# Math Errors
[In this commit](https://github.com/waikato-ahuora-smart-energy-systems/PropertyPackages/blob/332a9909b405583ccd996f8bbb0bcf39fcb1c7fb/), everything works except for three tests that give a math range error:


```
FAILED property_packages/tests/test_compressor_pr.py::test_compressor_asu - OverflowError: math range error
FAILED property_packages/tests/test_compressor_pr.py::test_expander_asu - OverflowError: math range error
FAILED property_packages/tests/test_mixer_bt_pr.py::test_mixer - OverflowError: math range error
```

(tested using idaes 2.8.0dev0).

I think this could be because of some divide by zero errors, as some variables appear to be set to an initial value of zero. 

So far, this has not been fixed.


## Test case - math range error

This test case can be used to debug why a math range error occours.

```py
# Build and solve a heater block.
from property_packages.build_package import build_package
from pytest import approx

# Import objects from pyomo package 
from pyomo.environ import ConcreteModel, SolverFactory, value, units

# Import the main FlowsheetBlock from IDAES. The flowsheet block will contain the unit model
from idaes.core import FlowsheetBlock
from idaes.core.util.model_statistics import degrees_of_freedom
from idaes.models.unit_models.heater import Heater
from idaes.models.unit_models.pressure_changer import Pump
from idaes.core.util.tables import _get_state_from_port
from idaes.models.unit_models.pressure_changer import PressureChanger, ThermodynamicAssumption
from idaes.core.util import DiagnosticsToolbox


def assert_approx(value, expected_value, error_margin):
    percent_error = error_margin / 100
    tolerance = abs(percent_error * expected_value)
    assert approx(value, abs=tolerance) == expected_value

m = ConcreteModel()
m.fs = FlowsheetBlock(dynamic=False) 
m.fs.properties = build_package("peng-robinson", ["argon", "oxygen", "nitrogen"], ["Liq", "Vap"])

m.fs.compressor = PressureChanger(
    property_package=m.fs.properties, 
    thermodynamic_assumption=ThermodynamicAssumption.isentropic,
    compressor=True)

sb = _get_state_from_port(m.fs.compressor.inlet,0)
m.fs.compressor.inlet.flow_mol[0].fix(1*units.kilomol/units.hour)
m.fs.compressor.inlet.pressure.fix(200000 * units.Pa) # doesn't work from 100,000
sb.temperature.fix((273.15+25) * units.K) # Temperature is not on port
m.fs.compressor.inlet.mole_frac_comp[0, "argon"].fix(0.33)
m.fs.compressor.inlet.mole_frac_comp[0, "oxygen"].fix(0.33)
m.fs.compressor.inlet.mole_frac_comp[0, "nitrogen"].fix(0.33)

m.fs.compressor.outlet.pressure.fix(500000*units.Pa)
m.fs.compressor.efficiency_isentropic.fix(0.75)

assert degrees_of_freedom(m) == 0

#m.fs.compressor.initialize()

dt = DiagnosticsToolbox(m)

dt.report_structural_issues()

solver = SolverFactory('ipopt')
# solver.solve(m)
```

This outputs the following debug message:

```
====================================================================================
Model Statistics

        Activated Blocks: 16 (Deactivated: 0)
        Free Variables in Activated Constraints: 123 (External: 0)
            Free Variables with only lower bounds: 24
            Free Variables with only upper bounds: 18
            Free Variables with upper and lower bounds: 71
        Fixed Variables in Activated Constraints: 47 (External: 0)
        Activated Equality Constraints: 123 (Deactivated: 0)
        Activated Inequality Constraints: 0 (Deactivated: 0)
        Activated Objectives: 0 (Deactivated: 0)

------------------------------------------------------------------------------------
1 WARNINGS

    WARNING: Found 8806 potential evaluation errors.

------------------------------------------------------------------------------------
2 Cautions

    Caution: 16 variables fixed to 0
    Caution: 42 unused variables (42 fixed)

------------------------------------------------------------------------------------
Suggested next steps:

    display_potential_evaluation_errors()

====================================================================================
```

The evaluation errors seem a bit meaningless to me, it's hard to read them. I'm thinking the variables fixed to zero are the problem, because dividing by near-zero is a good way to get massive numbers.

```
>>> dt.display_variables_fixed_to_zero()
====================================================================================
The following variable(s) are fixed to zero:

    fs.properties.PR_kappa[argon,argon]
    fs.properties.PR_kappa[oxygen,oxygen]
    fs.properties.PR_kappa[nitrogen,nitrogen]
    fs.properties.argon.enth_mol_form_liq_comp_ref
    fs.properties.argon.cp_mol_ig_comp_coeff_B
    fs.properties.argon.cp_mol_ig_comp_coeff_C
    fs.properties.argon.cp_mol_ig_comp_coeff_D
    fs.properties.argon.cp_mol_ig_comp_coeff_E
    fs.properties.argon.enth_mol_form_vap_comp_ref
    fs.properties.argon.entr_mol_form_liq_comp_ref
    fs.properties.oxygen.enth_mol_form_liq_comp_ref
    fs.properties.oxygen.enth_mol_form_vap_comp_ref
    fs.properties.oxygen.entr_mol_form_liq_comp_ref
    fs.properties.nitrogen.enth_mol_form_liq_comp_ref
    fs.properties.nitrogen.enth_mol_form_vap_comp_ref
    fs.properties.nitrogen.entr_mol_form_liq_comp_ref

====================================================================================
```
Except, these all seem pretty standard. Argon probably is an ideal gas so that's why things are fixed to zero, but that could be the problem. I'm pretty sure the kappa values are zero across all tests. Also, test_asu_pr uses argon, oxygen, and nitrogen, just as a state block, and it's all passing, so i'm not sure.

Initialisation has a math range error, while just calling solve() without initialisation returns infeasable. I assume the math range error is because the solver decides the solution is infinity, which is infeasible.


Some other things I could look into is switching back to ftpx (should only be one line changed in common_parsers.py, and theoretically shouldn't even break anything in the existing tests) and running the same model diagnostics to check for any differences. Or talk to a chemical engineer about what else the problem could be, and make sure the problem is actually sensible (i'm unsure of the source of the test originally, i could try find that too.) Lastly, I could comb more closely through the initialisation code that is causing the math range error, and try isolate just that state block config (if its' a state block) and ensure that can be solved first.

as the fix mentioned in the discussion is only coming out in idaes 2.8.0, it's probably okay to put it to the side for now.