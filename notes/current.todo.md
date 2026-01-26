---
id: ybtrcaddmxfv8rdm6yqqq52
title: Todo
desc: ''
updated: 1769468356417
created: 1768442759192
---


# LSL Heat Pump Digital Twin

- What could we do to use the ahuora platform to DT it? What can we calculate?

# Platform Bugfixes

- Water pipe model
- dynamics with petsc sovler
- cleaner interface for p&id control

# Excel Integration

- Setup keycloak access tokens

# Oji Fibre Project

- Get data from the csv file to rapidscada/thingsboard, with solving the model in the platform too.

# Release Idaes-Service

Release the core of idaes-service as ahuora-model-loader, so that other people can use ahuora models in their pyomo work. At the same time, we can clean it up a bit - seperate the server stuff from the sdk, and seperate out the adapterLibrary file to maybe a directory or something.

# Proposals

- Data preprocessing. It's one of the biggest bottlenecks. Is there something needed there? Or should we just use node-red etc?
- Diagnostics - Write a proposal - some ideas are in [[current.biggest-bottlenecks]].
- Parameter Estimation - Write a proposal for a good methodology to do that as well. Surely we can test on the geothermal data pretty easily?
- Machine learning  of property packages.

# Custom property packages

- merge in the branch!
