---
id: self_organizing_maps
aliases: []
tags:
  - master
  - machine_learning
---

# Self Organizing Maps

A model based on vector quantization and that can be used for clustering and
dimensionality reduction is the **self organizing map (SOM)**. This model, in
order to avoid confinement to local minima, uses the **soft-max** adaptation
rule: not only the winner reference is updated but all references are affected
depending on their distance to the input $x$.

The self organizing map consists of $N$ neurons located on a regular
low-dimensional grid (usually 2D). Each unit can be identified by the coordinate
on the map, receives the same input $x$ and has a weight $w$ that lives in input
space.

SOMs learn a map from input space to a lattice of neural units that preserves
topology. Neighboring units respond to similar input pattern and data points
close in the input space are mapped onto the same or _nearby map units_.

---

The aim of the SOM is to learn a map from vectors to a discrete output space
(the coordinates of the unit on the map). The map is randomly initialized and
has two phases for each iteration

- **Competitive stage**: the winner is the most similar unit to $x$.
- **Collaborative stage**: update weights of the units that have topological
  relationships with the winner unit (soft-max approach).

The algorithm continues until convergence criterion is met.

## References

- [[unsupervised_learning]]
