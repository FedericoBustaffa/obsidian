---
id: stencil
aliases: []
tags:
  - master
  - parallel_computing
---

# Stencil

Commonly used for scientific and engineering simulations, the **stencil**
pattern is used to update points on a $n$-dimensional grid. An example of
stencil computation is given by the Jacobi algorithm, where each element of a
matrix is updated with the mean of its four neighbors.

To parallelize this algorithm, is common to partition the input data domain in
subdomains (**grids**) each processed by a worker. As said before, each point is
updated following a specific pattern that uses its neighbors. This implies that
the majority of points in each subdomain, can be updated simultaneously; points
on the border of a subdomain may need gathering data from the neighbors.

Usually the computation goes on until a termination condition is reached and
logically the stencil is a _map_ and _reduce_ pattern.

## 5-Points

One of simplest stencils is the 2D-grid, that is also _regular_ and _symmetric_
like a $n \times n$ matrix. In this case the involved points are 5, the main
point and its four neighbors.

Another typical way of computing a stencil is by creating a copy of the grid, in
which the result will be stored. This easily prevents errors because in a
stencil computation every update is done considering the initial grid values and
not the updated one.

Also to simplify the logic of the computation, so called **halo cells** are
added, in order to let the compute the border cells the same way of the others
without the need of any conditional statement.

![[stencil2D.png]]

The main challenge in this case is to partition data elements to the processors,
in order to reduce the communication overhead and balance the workload. This is
a $NP$-hard problem but there are two main heuristics that can be employed:
**row** and **tile** paritioning.

Supposing to have $k$ workers, the first method produces $n/k$ partitions that
are blocks of rows. In the second case, every tile is formed by $\sqrt{k} \times
\sqrt{k}$ elements, for a total of $n^2 / k$ partitions.

### Cost Model

Considering that we can overlap the computation of internal elements with the
communication of the border elements, the stencil completion time is

$$
T_c^\text{stencil} (n^2, k, F) = T_\text{scatter} (n^2, k) + T_\text{compute}
(n^2, k, F) + T_\text{gather} (n^2, k)
$$

where

$$
T_\text{compute} (n^2, k, F) = m \cdot \left[ \max{(T_F \cdot (\sqrt{k} - 2)^2,
4 \cdot T_\text{comm} (\sqrt{k}) )} + 4 \cdot T_F \cdot \sqrt{k} \right]
$$

![[stencil2D_tile.png]]

More complex stencils can be **irregular** or **dyanmic**, for example
unstructured grids or _wavefront_ that could add dependencies and complexity to
the computation, changing the actual cost of the parallel computation.

## References

Links: [[structured_parallel_programming]]
