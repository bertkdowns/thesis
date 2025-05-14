---
id: m8vp45b9okqhkmmb8ksrouz
title: Flowsheet Visualiser
desc: ''
updated: 1743117524988
created: 1743117524988
---
# Flowsheet Visualiser software Review



[flowsheet.py](https://github.com/IDAES/idaes-ui/blob/main/idaes_ui/fv/flowsheet.py) shows an example of the data format used by the visualiser:

```JSON
{
    "model": {
        "id": "<model name>",
        "unit_models": {
            "<component name>": {
                "image": "<image name>",
                "type": "<component type name>",
                "...": "more values..."
            },
            "...": "etc."
        },
        "arcs": {
            "<arc name>": {
                "source": "<component name>",
                "dest": "<component name>",
                "label": "<label text>"
            },
            "...": "etc."
        }
    },
    "cells": [
        {"id": "<component_name>", "...": "other values used by JointJS.."},
        "..."
    ]
}
```

From what I understand, the "cells" sections is for JointJS positions, and they might be auto generate.

However, the general use case is to build a model in idaes, and then call .visualise to use the flowsheet visualiser interface. This constructs the frontend using all the idaes internals.

The main visualiser function is here:

[fsvis.py](https://github.com/IDAES/idaes-ui/blob/main/idaes_ui/fv/fsvis.py#L52)


A demo flowsheet is avaliable at [demo_flowsheet.json](https://github.com/IDAES/idaes-ui/blob/main/idaes_ui/fv/tests/demo_flowsheet.json) , but it's pretty unreadable unless you use code folding.

# Questions still to answer

Is the .json file enough to create an IDAES model? or do you still need the idaes model constructed in python first?

# Potential applications

We could add functionality to create this model type directly - but is that pointless if we can already generate the idaes code itself? the idaes code can be used to generate the model easily enough.

Or, idaes service could be used to generate a model for opening in the flowsheet visualiser - but it only makes sense if the .json file alone is enough to create an idaes model.

Or, should we support importing from these flowsheet visualiser files directly into our platform?

Is there anything about the data structure that could be useful for us to consider?