---
id: cache
aliases: []
tags:
  - master
  - parallel_computing
---

# Cache

A common way to optimize code and reduce the impact of Von Neumann bottleneck is
writing cache aware programs. The **cache** is a smaller memory, much faster
than RAM, capable of boosting performance well used. Modern CPUs typically have
a hierarchy composed by three levels of cache:

- **L1**: very small but very fast (0.5 - 1 ns).
- **L2**: bigger but slower.
- **L3**: bigger than L2 but the slowest among the three (15 - 40 ns).
- **RAM** very big but order of magnitude slower than cache memories (50 - 100
  ns).

Depending on the architecture we can have some **private** levels of cache for
each processor or core (typically L1 and L2), while L3 level is typically
shared.

## Locality

The **locality principle** is the core concept that makes the memory hierarchy
work properly. It increases the probability of reusing data blocks that were
previously moved from one level to the previous, thus reducing the miss rate.
There are two types of locality:

- **Temporal**:
- **Spatial**:

## References

- [[shared_memory_systems]]
