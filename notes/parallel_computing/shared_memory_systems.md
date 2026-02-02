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

![Von Neumann Architecture|500](/files/von_neumann.png)

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

In other words the roofline model tries to show how far the application is from
the achievable performance on a given system. In order to make this evaluation,
the model considers the relationship between **operational intensity**,
**memory bandwidth** and **peak computational performance**, considering also
_loops_ as the main source of computational workload.

The first assumption the model does is a simplified view of the hardware
architecture:

- **Execution units** operate at maximum speed $R_\text{peak}$ measured in
  FLOPS.
- **Data store** is any source of data.
- **Data channels** to and from the data store have and work at a peak bandwidth
  $B$.

The other set of assumptions is on the application:

- It is composed by a sequence of loops with a large amount of iterations $L$.
- At steady state the considered loop does $N$ FLOP using $D$ bytes of data
  transferred to or from the data store.

Considering that, the ratio

$$I = \frac{N}{D}$$

is called **computational intensity** measured in FLOP/byte.

With this model should be clear if our application's bottleneck is the
computation or the memory channels' bandwidth. Starting from the data channels
performance, which can be expressed in FLOPS with the factor

$$
I \cdot B \quad \left( \frac{\text{FLOP}}{{\text{byte}}} \cdot
\frac{\text{byte}}{\text{seconds}} = \text{FLOPS} \right)
$$

Both $R_\text{peak}$ and $I \cdot B$ are upper limits, therefore, the expected
performance is

$$P = \min (R_\text{peak}, I \cdot B)$$

![Roofline Model|350](/files/roofline_model.png)

## References

- [[parallel_architectures]]
- [[cache]]
- [[simd]]
- [[threading]]
