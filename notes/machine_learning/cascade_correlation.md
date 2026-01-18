---
id: cascade_correlation
aliases: []
tags:
  - master
  - machine_learning
---

# Cascade Correlation

In the **cascade correlation** method the model starts with few units and add
them based on the training algorithm, so both weights and topology are learned.
This approach allows to better deal with hypothesis space of flexible size, also
by training a single unit at each step.

Starting for example from a single unit, the model il trained until convergence;
if the error is still to high an hidden unit is added, in order to maximize the
**correlation** between the output of the unit and the residual error of the
previous network.

After the training, the new unit weights are frozen and the output layer is
retrained. When a unit is added is directly connected with all inputs and all
previously added units.

## References

- [[neural_networks_training]]
