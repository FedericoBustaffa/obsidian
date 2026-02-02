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

## Optimizations and Parallelism

The first way to optimize code and reduce the impact of Von Neumann bottleneck
is writing cache aware programs. The **cache** is a smaller memory, much faster
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

---

Another common way to optimize code that introduces a form of synchronous
parallelism, keeping the computation on a single core, is through **vector
units**.

We are talking about actual hardware, capable of process multiple data in
parallel with one clock cycle; they are the basics of SIMD parallelization, that
works in locksteps with a combination of **multiple ALUs** and **vector
registers**.

---

The last way of improving performance on shared memory systems is through
**threading**, exploited by multi-threads and multi-cores processors.

This introduces an asynchronous parallelization where each thread/core has its
own set of registers and program counter, kept by the processor in order to
quickly switch between different contexts, mitigating pipeline bottlenecks due
to dependencies.

## Roofline Model

The **roofline model** is a way to better understand performance limits based on
compute and memory constraints or capabilities. The model gives a much more
realistic view than the raw $R_\text{peak}$ value, also identifying if the
workload is compute or memory bound, and so potential bottlenecks.

## References

- [[parallel_architectures]]
- [[cache]]
- [[simd]]
- [[threading]]
