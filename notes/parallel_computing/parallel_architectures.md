---
id: parallel_architectures
aliases: []
tags:
  - master
  - parallel_computing
---

# Parallel Architectures

Parallel architectures can be characterized by two main aspects:

- **Single Node**: vector registers, multi-threading and multi-processing.
- **Multiple Nodes**: interconnection network of multiple nodes, forming the
  parallel architecture.

There are multiple criteria to classify the type of architecture and parallelism
the given system has to offer.

- **Flynn's Taxonomy**.
- **Memory layout**: based on memory organization and hierarchy.
- **Cores and node number**.
- **Interconnection networks**.

## Flynn's Taxonomy

The **Flynn's Taxonomy** is based on the instruction type and data stream; it
contains 4 possibile classifications for parallel systems:

- **SISD (Single Instruction Single Data)**: classic Von Neumann model where a
  single instruction is applied on a single stream of data.
- **SIMD (Single Instruction Multiple Data)**: a single instruction is applied on
  multiple items simultaneously.
- **MISD (Multiple Instruction Single Data)**: multiple instruction are applied on
  a single item, producing multiple outputs.
- **MIMD (Multiple Instruction Multiple Data)**: multiple instructions are applied
  to multiple data.

## Memory Layout

The other possible classification method is about memory organization and
hierarchy in _shared memory_ systems like:

- **SMP (Symmetric Multi-Processor)**: all processors or cores have roughly equal
  access time to memory. Also called **UMA (Uniform Memory Access)**.
- **NUMA (Non-Uniform Memory Access)**: layout where all processors has access to
  a local private memory and to a global shared memory. In this case the time
  access is asymmetric based on which memory is accessed.

Inherently, distributed systems are NUMA, because of their strongly asymmetric
access to their private local memory.

In general, parallel systems goals and chellenges are to optimize the cache
usage and minimize the Von Neumann bottleneck; for distributed systems the main
goal is a fast communication protocol via fast messaging libraries, routing and
flow control.

## Distributed vs Shared Memory Systems

In a _distributed memory system_ each node is a complete computer system (SMP or
NUMA) multiprocessor and processes on different nodes communicate _explicitly_
by sending messages across a network. Tipically for high-performance computing
the aim is on homogeneous nodes and fast network topologies.

On the other side _shared memory systems_ comprise a (modest) number of cores,
all with direct hardware access to a shared memory space. Modern architectures
all have 2 or 3 levels of cache to mitigate the expensive access to the main
memory.

In general we can say that distributed memory systems are more scalable, costly
and less energy efficient. From the standpoint point of the programmer

- In **shared memory systems** have faster communication between
  threads or processes; however the efficient management of synchronization and
  locking is generally a critical point of paying attention.
- For **distributed memory systems**, the most important aspect is to reduce the
  cost of communication as much as possible.

## References

- [[parallel_computing]]
- [[shared_memory]]
- [[distributed_systems]]
