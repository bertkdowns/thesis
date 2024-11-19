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
                properties:
            }
            "deltaP":{
                "type": property
                value:[0,[1]]
                "units": "pa
                }
        }
    }
}


# Result

