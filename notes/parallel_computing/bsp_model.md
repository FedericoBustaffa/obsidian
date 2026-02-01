---
id: bsp_model
aliases: []
tags:
  - master
  - parallel_computing
---

# Bulk Synchronous Parallel Model

The **bulk synchronous parallel (BSP)** model is a framework to design, analyse
and programming general purpose parallel systems. In other words it provides a
way to implement parallel algorithms and a structure composed by three main
parts:

- A collection of **processors** with their own **local memories**.
- A **communication network** to move data.
- **Barriers**: a strong synchronization mechanism.

All the $n$ processors are identical and they can communicate through the
network via message passing, in a uniform amount of time.

![BSP Model|400](/files/bulk_model.png)

This differs a little from the PRAM model which use the global memory as a space
to write messages among processors.

## Supersteps

The BSP model works as a sequence of **supersteps**, each consisting of three
main simple steps:

- **Computation**: local progress of tasks (floating point operations).
- **Communication**: data exchange among processes.
- **Global barrier** synchronization which ensures that the previous computation
  or communication supersteps have completed for every processor.

In this sense the algorithm is ruled by the slowest worker and the speed of data
exchange.

### Computational Cost

The computational step is defined in function of

- The amount of **work** $w$, defined as the maximum number of operations
  performed in the superstep by any processor.
- The **latency** $l$ of the global barrier.

In total a computation step simply costs the sum of the two:

$$T_\text{comp} = w + l$$

### Communication Cost

To measure the communication step cost, the BSP introduces the concept of
$h$-relation, that is a communication superstep in which every processor sends
and receives at most $h$ data words. In other words, the maximum number of data
words sent or received by a processor.

So the cost of an $h$-relation is defined by

- The **gap** $g$ or the cost to send a word.
- The **latency** $l$ of the global barrier.

as

$$T(h) = h \cdot g + l$$

When we have that all processors send or receive exactly $h$ data words, we can
talk about **full $h$-relations**. They can be useful to approximate $g$ and $l$
on a real machine, by measuring execution times for a range of full
$h$-relations, varying $h$ and $p$.

### Total Cost

Overall we have that a BSP based algorithm has a computational cost of

$$C_\text{BSP} = a + b \cdot g + c \cdot l$$

by adding the cost of all the supersteps. With this formula we can obtain a
number (in FLOPS) that is quite more precise than an order of magnitude.

## References

- [[parallel_computing]]
