---
id: l12ssot4gt0512ycbssgj9k
title: Evaporator 3 Effect
desc: ''
updated: 1771909840401
created: 1771879960753
---


[3 Effect Evaporator in the flowsheet](assets/evaporator-3-effect.png)



[Bipartite graph of constraints and variables and their connections](assets/graph-overview.png)


This graph shows that the model is actually pretty densly connected, even though from the flowsheet layout it looks like it's quite spread out. though I think this is a bit of an illusion - it's really that everything is using the same coeffients in the property package. I would like to see a version which doesn't use the same coefficients like that everywhere, and I wonder if that somehow reduces the performance (even though duplicating the constants would add more fixed variables to the model, maybe it would be simpler to solve mathematically (could be subtly biasing the solver or something?))

The graph is a bipartite graph - there are nodes for the constraints, and nodes for the variables. the variables are connected to all the constraints they are used in.

[Area in the graph where only a few things are connected to each node](assets/graph-sparsely-connected.png)

However on the outskirts of the graph, one node is not connected to many things. this is likely the more "high level" stuff - like a simple energy balance (enth_in + work = enth_out).



[Area in the graph where one node is connected to many other nodes](assets/graph-densly-connected.png)

There are some nodes that are connected to many other nodes. I think these are likely


[Area in the graph where many nodes are connected to similar things](assets/graph-simarly-connected.png)

<iframe src="https://bertkdowns.github.io/thesis/assets/flowsheet-graph.html" style="width:100%;height:650px"></iframe>