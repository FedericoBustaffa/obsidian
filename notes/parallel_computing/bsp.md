---
id: bsp
aliases: []
tags:
  - master
  - parallel_computing
---

# BSP

The **BSP** (Bulk Synchronous Parallel) model is a offer a framework to design,
analyse and programming general purpose parallel systems. In other words it
provides a way to implement parallel algorithms and a structure composed by
three main parts:

- A collection of **processors** with their own **local memories**.
- A **communication network** to move data.
- **Barriers**: a strong synchronization mechanism.

All the $n$ processors are identical and they can communicate through the
network via message passing, in a uniform amount of time.

This differs a little from the PRAM model which use the global memory as a space
to write messages among processors.

## Algorithm

The BSP algorithm is composed by a sequence of **supersteps**, each of which
consists of

- **Computation**, **communication** steps or both (**mixed superstep**).
- **Global barrier** synchronization which ensures that the previous computation
  or communication supersteps have completed for every processor.

The global barrier also gives the name _Bulk Synchronous_ to the model.

### Cost

The BSP model takes into account also the communication time in a fine grained
way, introducing the concept of $h$-relation which tells us the maximum number
of data words exchanged by processors in a single communication (or mixed)
superstep.

The cost of an $h$-relation is given by the following formula

$$T(h) = h \cdot g + l$$

where $g$ is the **gap** and $l$ is the **latency**. The _gap_ is communication
cost of a single data word and the _latency_ is the cost of the global barrier
synchronization.

When we have that all processors send or receive exactly $h$ data words, we can
talk about **full $h$-relations**. They can be useful to approximate $g$ and $l$
on a real machine, by measuring execution times for a range of full
$h$-relations, varying $h$ and $p$.

For the cmputational part we have a simpler formula that takes into account only
the amount of work to be done and the latency given by the global barrier

$$T_\text{comp} = w + l$$

in particular $w$ represents the maximum number of operations to be done among
all processors.

Overall we have that a BSP based algorithm has a computational cost of

$$C_\text{BSP} = a + b \cdot g + c \cdot l$$

by adding the cost of all the supersteps. With this formula we can obtain a
number (in FLOPS) that is quite more precise than an order of magnitude.

## References

Links: [[computational_models]]
