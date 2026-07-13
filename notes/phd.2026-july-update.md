---
id: wibkdkfh6mghi8n11iimmf2
title: 2026 July Update
desc: ''
updated: 1783583069203
created: 1783550831705
---


I feel we have made a good bit of progress in making solving more reliable. My recent work on switching between flow mass and flow mol in [idaes.initialisation-with-custom-constraints] has helped with that.

I have also started work on [[pyomo.hierarchical-modelling]], but it's still in pretty early/draft stages. Combining pyomo's flexibility with a more rigid framework doesn't always work great - particularly as it also has to work with how IDAES works (and how sequential decomposition works.) I need to think more about the architecture for it, or if writing my own modelling framework would be better.

I also have been working on [[case-studies.steam-generating-heat-pump]], which has made a good end to end workflow for using steady-state process models in a live system.

Per-unit-operation [[idaes.diagnostics]] has also been a pretty big win recently.

My big next things could include:

## How can we quickly turn source documents into a working model?

We have currently done some work on [DEXPI integration](https://github.com/waikato-ahuora-smart-energy-systems/Ahuora-Adaptive-Digital-Twin-Platform/pull/2180), to load in a flowsheet from a DEXPI model. This does some very basic estimation of which unit operations are which to allow importing. However, next steps could include making this work for more unit operations, and also extracting relevant properties (e.g heat exchanger area etc) from the DEXPI File. AI tools could also be used to extract or estimate these properties. I have also been working on P&ID PDF to Dexpi conversion [(github)](https://github.com/bertkdowns/digitization-of-piping-and-instrument-diagrams) which could complement this well, as well as [https://github.com/bertkdowns/ahuora-mcp](a tool to allow agents to use the ahuora platform.)

Combining all these things could provide a very solid pipline to turn source documents into a working flowsheet model.


Next steps could be either:

- PID PDF to dexpi conversion reliability
- Better agentic tools for working with flowsheets in the platform (Trawling through source documents, including pdfs, dexpi and other drawings, and  [office documents](https://github.com/iOfficeAI/OfficeCLI)) to find what the design parameters are, and using the detailed [[idaes.diagnostics]] tools to automatically fix errors that might be causing your flowsheet to fail.
- better dexpi import into the platform.

## Hierarchal modelling

The current protoype for [[pyomo.hierarchical-modelling]] that i've made, [pyomo-levels](https://github.com/bertkdowns/pyomo-levels), is not really great. It has some cool tools for auto-generation of a surrogate model, but figuring out the exact interface for building a model that you can switch between different things is ugly. I kind of think i should flesh out my variable replacement library first, come up with a really clean implementation for that, and then rebuild pyomo-levels on top of it.

I also am not using my variable replacement library. It would be nice to figure out the best way to write my variable replacement library, and then actually use it within ahuora-builder. Currently it implements its own replacement logic. This would be very nice. Ahuora-builder (the renamed idaes_service) has a lot of cool things in it too, such as being able to fix non-state variables, and it would be really good to make them accessible to those developing pure idaes flowsheets as well - any time I try test stuff in idaes I immediately get really frustrated with initialisation not working, not knowing what variables to specify, not being able to set flow mass instead of flow mol, etc. Plus, a refactor of ahuora-builder would be a good chance for me to test out how refactoring with agents goes.


# Fun side projects

Can we make a VR PLC/control system? unity/webvr/omniverse could work good. It would be cool to implement some simple UI elements for changing setpoints and viewing values, and uploading a gaussian splat or 3d model, positioning the two together appropriately, and then a user can view the plant in 3d and see the PLC control effects too. This also could be used in other ways - tagging the equipment with the same tags in the platform could allow a 3d preview of unit operations so you know what the physical thing looks like when editing your flowsheet.

If you wanted to go extra fancy, you could use 3d model segmentation such as https://github.com/Jumpat/SegAnyGAussians to detect the specific objects in the scene so you can split them out. An even more fancy method would track pipes across the site (including through walls etc) so that you can accurately see where flow is going - but that would be a massive task.




