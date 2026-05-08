---
id: 0crmvjpa2bzrhqg0jkr63i6
title: Dt Hp Paper Abstract
desc: ''
updated: 1770941045446
created: 1769632426690
---

# A Repeatable Digital Twin Architecture for the AI era: Case study of a Butane Heat Pump Digital Twin


What is stopping Digital Twins from becoming an industry standard?

Building a Digital Twin involves connecting a Data-Driven or First-Principles process model to real-time data, for the purposes of gaining better insight into the current process state or simulating the impact of a choice on the future state of the process. Data collection systems are already used across the process engineering sector. Yet building a digital twin of an industrial process is still a complex and costly task. Literature that covers Digital Twins often focuses on very different use cases, and industry standard procedures have yet to emerge. A large part of the problem is that the techniques to run a model in an offline batch simulation are very different to the techniques used to run a model in a real-time scenario.

To investigate this problem, this paper discusses the development of a Digital Twin for a Butane Heat Pump. The layers of a Digital Twin are reviewed including reading individual sensors, aggregating across data sources, building a model, linking the process to the model, and linking the model to the control system.

Knowing which type of model should be built, and linking the process to the model is shown to be the main challenge with Digital Twin implementation. First principles, surrogate models, data driven models, and hybrid models are commonly used in offline historical analysis of chemical processes. Each requires different levels of adaptation to fit into the needs of a real-time digital twin. Implementations of each are shown for the Butane Heat Pump case study.

The results show that while data driven models are naturally the easiest to connect to a real-time system, they offer little predition benefit outside of the typical operating regions. First principles models have a much higher development cost, and are much less flexible to model unusal process states, but under certain circumstances can outperform data driven models significantly. Hybrid modelling and surrogate modelling techniques, including parameter estimation, are shown as ways to find a middle ground between these two extremes. 

A software solution is presented which enables first-principles models built in the Ahuora Digital Twin Platform to work alongside data driven models for a more flexible and reliable real-time Digital Twin. This will decrease the time it takes to build a future Digital Twins, unlocking new efficiency opportunities across the Chemical Process industry. 