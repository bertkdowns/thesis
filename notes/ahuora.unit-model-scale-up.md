---
id: 5yfgew9bcmqn0yjgnux21kc
title: Unit Model Scale Up
desc: ''
updated: 1776398291338
created: 1776386674955
---

(This is written as a prompt for an ai agent.)

Right now we support around 20-30 different unit operations on the ahuora platform. In future, we may want to scale up to hundreds of different unit operations. However, having to add so many different files to define a new unit operation is very frustrating. These include:

- For custom unit models we have to create an idaes unit model -  see crystalliser.py for an example
- We need to create a model config file for the unit operation in django : see crystalliser_config.py
- We need to create the adapter - e.g crystalliser_adapter for use in idaes_factory
- We need to add the adapter to the adapter_library.py in ahuora_builder
- We need to add it to frontend/src/pages/flowsheet-page/flowsheet/LeftSideBar/PanelItems.tsx

We also need to update the .gen.ts files using ./generate_frontend_data and ./generate_api_types.


However, in the future we might have many more unit operations. It would be good to streamline this process. One idea is to have a single .py file or IDAES unit operation class, and standardise some additional methods for any additional config that is required e.g in the unit_op_config file etc, for any information that is not already contained in the classes. Then Django could call those methods to get the properties it needs rather than looking up the config file.

There are also some special cases where things get harder, such as:

Machine learning blocks don't have their properties in the config file as they are created dynamically when you load in the block. This is kind of frustrating and I wish I knew a better way to keep it consistent with other things.

Streams also have a config file (for energy_stream, material_stream, humid_air_stream, etc) but they are not technically idaes unit operations. However, in our platform in django they are seperate simulationObjects. If we get rid of the config file, where do we put their configuration and how do we keep it compatible with the unit operations for django?

Mixers and splitters have different port adapters because there may be more or fewer inlets. How can we deal with that? 


Do an audit of the files and similar operation_config files and operation_adapter files, and write a summary of your findings, potential problems, two or three alternatives to solving this problem (the single python file idea or something different), your recommended pick from those alternatives, and a staged implementation plan  for how to improve the situation. Each stage of the implementation should result in a fully working version, so we have an agile improvement process (e.g stage 1: move the config to the same file as the operation, stage 2 refactor the config file into seperate methods, etc)

.