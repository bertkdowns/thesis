---
id: c3le5xbabefr0v0v9fiza9g
title: 2026 February Update
desc: ''
updated: 1771906419867
created: 1771819099133
---

Major Recent stuff:

- [[case-studies.lab-heat-pump]]
- [[digital-twins.data-streaming-architecture]]
- [[case-studies.evaporator-3-effect]]

What I think is next is two big questions:

- How do we make solving more reliable?
- How do we actually link up a model to live data?

## How do we make solving more reliable

With building the [[case-studies.evaporator-3-effect]] flowsheet, the model would solve, but was so unstable that even a change in flow rate of 1 kg/s would cause it to fail. However, it was not actually infeasible - a series of updates changing the flow by 0.1 kg/s each time was able to slowly lower the flow until it did solve at that point. This shows that the idaes [Homotopy Meta-Solver](https://idaes-pse.readthedocs.io/en/stable/reference_guides/core/homotopy.html) would be able to solve the model. So that's one potential solution (though it would require refactoring so that instead of updating constraints for expressions, we add variables for expressions and fix them.)

### Better model formulation


### Better Model Scaling




### Slack variables for model diagnostics


### phasing between model complexity levels


## How do we actually link up a model to live data?


