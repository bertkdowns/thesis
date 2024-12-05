# IDAES service expressions


# Purpose


# Research questions


How does modelling property packages and everything else affect the simplicity and reusability of idaes service?

# Method/Process

Update the IDAES Service tests to reflect the new architecture:

{
    "pp1":{
        "Type": "property package"
        "args": {}
    }
    "pump"{
        "type: "pump"
        "args":""
        "properties":{
            inlet: {
                "type": port
                properties:{
                    "pressure": {
                        "type": "property"
                        "value":{"type":"numeric_indexedblock"
                            "indexes":{"0":1}}
                        "units":"pa" 
                    }
                }
            }
            "deltaP":{
                "type": property
                value:[0,[1]]
                "units": "pa
                }
        }
    }
    "expr-1" {
        "type": "expression"
        "expression": "pump.inlet.pressure + pump.deltaP"
    }
}


properties[0,{
    "pressure": {
        "type": "property"
        "value":1
        "units":"pa" 
    }
}]



Because this treats property packages the same as other unit operations, some codebase restructuring will be needed.

It also treats properties the same as unit model blocks

This relies heavily on the idea of union types/additive types. Pydantic will be a good framework for this.

if the expression relies on other blocks/unit models to be built first, then we will have to support this too. this could be done using a private field _built on the pydantic classes, and some sort of recursive build method. Alternatively, we could enforce that the specification order must be followed (this is probably good enough for now.)

after updating the tests, we should update the pydantic types, and use that to rewrite everything else.

After this is done for idaes_service, then we'll have to modify idaes_factory to support the updated schema too.

## Sympy expression parsing in pyomo

Pyomo has some support for parsing sympy expression, as long as you have a map between sympy `Symbol`s and pyomo `Var`s. The class to do that is PyomoSympyBimap, and we'll have to inherit from that to make a bimap we can populate with properties in the model.

The process will be:

replace the dots in variable names with somethign else (sympy won't parse dots in symbol names)

convert the string into a sympy expression

figure out what the symbols are, and find their equivalents in the pyomo model, and then create the bimap between them (or extend the bimap class to do this dynamically)

use pyomo's method to convert sympy to pyomo expression

add the expression to the pyomo model.

# Result


