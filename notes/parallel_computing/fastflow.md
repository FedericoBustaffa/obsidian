---
id: fastflow
aliases: []
tags:
  - master
  - parallel_computing
---

# FastFlow

An interesting research project in the field of structured parallel programming
is **FastFlow**, that provides a **two level** interface. The more low level
interface is called **building blocks**, while the other is simply called
**parallel patterns**.

The _building blocks_ are a flexibile and powerful interface, but more error
prone than the _parallel patterns_ (built on top of the _building blocks_).

A FastFlow application is a **directed graph** whose node are computing entities
and edges are channels carrying **pointers**. Graph nodes can be _stateless_ or
_stateful_, and they synchronize through **message-passing** for accessing
shared data.

A channel that connects two FastFlow nodes is implemented by using an
_Single-Producer Single-Consumer_ lock-free FIFO queue. Connecting single nodes
through one or more SPSC queues enable:

- **Input non-determinism**: each node can receive data from multiple sources
  without knowing which node will fire and without assuming any order.
- **Output selectivity**: one node can send to all nodes connected to it or
  selectively choose which nodes to send data to.

The communication channels have two possible implementations, the lock-free one
(default) and a blocking version, implemented through mutex variables and
condition variables.

Channels can also have bounded or unbounded (default) capacity but there are
some special channels, called **feedback channels** that can only be unbounded.

## Basic Sequential Node

In FastFlow every computational construct is a **node**, but there is an actual
_primitive_ node that can be seen as a single thread waiting for tasks to be
executed.

## References

Links: [[structured_parallel_programming]]

- [fastflow github](https://github.com/fastflow/fastflow)
