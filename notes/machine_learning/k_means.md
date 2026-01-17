---
id: k_means
aliases: []
tags:
  - master
  - machine_learning
---

# K-Means

One of the simplest clustering algorithm based on vector quantization is the
**K-means** that, given a number of centroids, finds the optimal position of
them in order to minimize the quantization error.

For this algorithm there are batch, mini-batch and on-line versions but the core
is the same. For the batch version the algorithm

1. Randomly initialize $K$ centroids in input space.
2. Each patter is assigned to the closest centroid.
3. Move each centroid to the _mean_ point of its group of points.
4. If no convergence criterion is met go back to point 2.

The on-line and mini-batch versions the algorithm considers only some samples at
a time. This introduces stochastic noise in the process and so it's typically
better to move in small steps, introducing the learning rate in the formula:

$$\Delta w_{i^*} = \eta \cdot \delta(i, i^*) \cdot (x_i - w_j)$$

Of course is also possible to use this formula for the batch version, but in
that case the algorithm is deterministic.

Note that putting each centroid in the mean of its group is equivalent to
minimize the error function for that specific cluster.

The inductive bias of K-means is about the clusters shape: it works well on
**hyperspherical** clusters, while for other shapes like _two moons_ is needed a
**kernel K-means** that maps the input space in higher dimension.

## References

- [[unsupervised_learning]]
