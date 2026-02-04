---
id: map
aliases: []
tags:
  - master
  - parallel_computing
---

# Map

A **map** is a pattern in which a single function $F$ can be applied
independently to each element of a collection, producing an output collection of
the same cardinality.

The parallelism is exploited by using a pool of workers, each working
independently on a partition of the input. This paradigm decreases the latency
to compute one input collection and if there is a stream of collection, it
lowers the service time and increases the throughput.

## Implementation

The implementation can use a _farm_ skeleton, for example with a simple schema
where an emitter dispatch tasks to workers and then a collector catch the
results.

The main issue with this pattern is the workload balancing but, in case the
input the workload associated with the collection is balanced, the emitter may
**scatter** the collection in a _block-based_ distribution.

If the workload is unbalanced, a dynamic approach can be used, like
_work-sharing_ or _work-stealing_, if a passive approach is preferred, or an
_on-demand_ distribution can be chosen for an active approach.

## Cost Model

Given a collection of $n$ elements, $k$ workers, one emitter and one collector
we have that the completion time can be obtained with the following formula:

$$T_c^\text{map} (n, k) = T_\text{scatter} + T_\text{worker} + T_\text{gather}$$

where

$$
\begin{gather*}
T_\text{scatter} = T_{\text{split} (n,k)} + k \cdot T_\text{comm}
\left( \frac{n}{k} \right) \\
T_\text{worker} = \frac{n}{k} \cdot T_F + T_\text{comm}
\left( \frac{n}{k} \right) \\
T_\text{gather} = T_\text{gather} \left( \frac{n}{k} \right)
\end{gather*}
$$

Also notice that between the workers stage and the two communication stages,
there is a partial overlap between computation and communication, unless a
_"lockstep"_ behavior is enforced.

### Optimal Number of Workers

It is also possible to find a nice number of workers by finding the value $k$
that minimizes the completion time. Considering the linear communication cost
model we have that

$$T_\text{comm} (n) = t_0 + n \cdot s$$

where $t_0$ is the setup time, $n$ is the number of elements or their dimension
in byte and $s$ can be seen as the latency of sending one element. Assuming that

$$
T_{\text{split}(n, k)} = T_\text{gather}
\left( \frac{n}{k} \right) \approx 0
$$

we have that the completion time can be computed as

$$
\begin{align*}
T_c (n, k) &= k \cdot T_\text{comm} \left( \frac{n}{k} \right) +
\frac{n}{k} \cdot T_F + T_\text{comm} \left( \frac{n}{k} \right) \\
&= (k + 1) \cdot T_\text{comm} \left( \frac{n}{k} \right) +
\frac{n}{k} \cdot T_F \\
&= (k+1) \cdot \left( t_0 + \frac{n}{k} \cdot s \right) + \frac{n}{k} \cdot T_F
\end{align*}
$$

So from this last equation is possible to compute the derivative and see where
it is equal to zero.

$$k_\text{opt} = \left\lceil \sqrt{\frac{n}{t_0} \cdot (T_F + s)} \right\rceil$$

Usually $k_\text{opt}$ is a very large number because $n$ is large and $t_0$ is
low.

### Binary Tree Scatter and Gather

In order to reduce the service time of the emitter and the overall latency of
the map to complete the scatter distribution, a binary tree scatter distribution
can be employed.

![Map Tree Scatter|400](/files/map_tree_scatter.png)

In this way the emitter service time becomes

$$T_s^e = 2 \cdot T_\text{comm} \left( \frac{n}{2} \right)$$

and the scatter latency becomes

$$
T_\text{scatter} = 2 \cdot \sum_{i=1}^{\lceil \log_2 k \rceil} T_\text{comm}
\left( \frac{n}{2^i} \right)
$$

Similarly, the same patter can be used to reduce the collector's service time
and the end-to-end latency.

![Map Tree|400](/files/map_tree_gather.png)

In this way the collector service time becomes

$$T_s^c = 2 \cdot T_\text{comm} \left( \frac{n}{2} \right)$$

and the gather latency becomes

$$
T_\text{scatter} = 2 \cdot \sum_{i=1}^{\lceil \log_2 k \rceil} T_\text{comm}
\left( \frac{n}{2^i} \right)
$$

Typically a real skeleton does not provide explicit implementations for emitters
and collectors, that are usually implemented within the worker.

## Parallel Filesystems

There are cases where we can even avoid the scatter and gather stages, or to be
more precise, they are logical operations. A **parallel filesystem** let the
workers to read and write the same file in different locations (no race
conditions) at the same time.

Advantages of this type of filesystem are that

- If the data is already distributed, the splitting overhead is minimal.
- Splitting and merging are logical operations that do not actually move data.
- In this scenario $T_c^\text{map} (n, p) \approx \frac{n}{k} \cdot T_{F+G}$,
  which can be significantly lower than that obtained from centralized I/O.

The _Hadoop's DFS_ is an example of parallel filesystem, but is quite old;
_Apache_ instead it is more recent and can be more common on HPC architectures.

## Reference

- [[structured_parallel_programming]]
- [[map_reduce]]
