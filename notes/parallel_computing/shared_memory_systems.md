---
id: shared_memory_systems
aliases: []
tags:
  - master
  - parallel_computing
---

# Shared Memory Systems

The basic structure of a **shared memory system** is represented by the Von
Neumann architecture, where an central processing unit takes an input and
produce an output, reading and writing from **memory**.

![Von Neumann Architecture|450](/files/von_neumann.png)

An important aspect of this architecture is the so called **Von Neumann
bottleneck**, originated by the difference between computation speed (typically
high) of the central processing unit and the main memory read and write speed
(nowadays high but significantly lower than computation speed).

<!-- add FLOPS example for Rpeak -->

## Cache

Modern CPUs typically have a hierarchy composed by three levels of **cache**:

- **L1**: very small but very fast (0.5 - 1 ns).
- **L2**: bigger but slower.
- **L3**: bigger than L2 but the slowest among the three (15 - 40 ns).
- **RAM** very big but order of magnitude slower than cache memories (50 - 100
  ns).

Depending on the architecture we can have some **private** levels of cache for
each processor or core (typically L1 and L2), while L3 level is typically
shared.

## References

- [[parallel_architectures]]
