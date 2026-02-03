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

- **Temporal**: refers to the tendency of a program to access the same memory
  location multiple times in a short period of time.
- **Spatial**: refers to the tendency of a program to access memory locations
  that are spatially close to each other.

Other useful terminology to reason about cache locality are

- **Cache jit**: the data is present in the cache.
- **Cache miss**: the data is not present in the cache.
- **Miss penalty**: the time spent transferring a cache line into the first
  level cache and the requested data to the processor.
- **Miss rate**: the fraction of memory accesses not found in the cache, defined
  by
  $$\text{MR} = \frac{\text{\# misses}}{\text{\# memory accesses}}$$
- **Hit rate**: the fraction of memory accesses found in the cache:
  $$\text{HR} = 1 - {MR}$$

So now it's possible to measure CPU time by the following formula

$$\text{CPU}_\text{time} = \text{ClockCycles} \cdot \text{ClockCycleTime}$$

where the number of clock cycles is defined as function of

- **Instruction count (IC)**: the number of instructions executed, that can be
  further detailed as
  $$\text{IC}_\text{CPU} + \text{IC}_\text{MEM}$$
- **Clock cycles per instruction (CPI)**: average clock cycles per instruction,
  defined as
  $$\text{CPI} = \frac{\text{ClockCycles}}{\text{IC}}$$
  that again can be detailed as
  $$
  \text{CPI} = \frac{\text{IC}_\text{CPU}}{\text{IC}} \cdot
  \text{CPI}_\text{CPU} + \frac{\text{IC}_\text{MEM}}{\text{IC}} \cdot
  \text{CPI}_\text{MEM}
  $$

so now the above formulation for the CPU time becomes

$$
\text{CPU}_\text{time} = \text{IC} \cdot \text{CPI} \cdot
\text{ClockCycleTime}
$$

Considering that each memory instruction may generate a cache hit or miss with a
given probability, and given the hit rate the probability of a cache hit, we
have

$$
\text{CPI}_\text{MEM} = \text{CPI}_\text{MEM-HIT} + (1 - \text{HR}) \cdot
\text{CPI}_\text{MEM-MISS}
$$

So now, knowing the clock frequency of the CPU, the clocks per instruction etc.
is possible to see how a decrease in hit rate can worsen performance by a lot.

## Cache Algorithms

Typically the cache hierarchy is not directly managed by the user, but by a set
of **caching policies** that determine

- which data is cached during program execution.
- where the data is stored.
- what cache line should be evicted if the cache is full.

Typically the data loaded from memory is a **cache line**, composed by several
items stored contiguosly in memory, in order to enforce _spatial locality_.

So the cache is organized in a number of cache lines and the cache line where
the data is stored is decided by some **cache mapping strategy**:

- **Direct mapping**: each memory block is restricted to exactly one cache line.
- **N-way set associative**: each memory block can be placed in any of $N$ lines
  within a set.
- **Fully associative cache**: any memory block can occupy any cache line.

In the end there is the need for a policy to replace cache lines when the cache
is full and a new line must by loaded:

- **Last recently used (LRU)**: remove the least recently used cache line ,
  accordingly to the temporale locality principle.
- **Pseudo LRU**: approximation of LRU keeping a set of bits that tracks which
  ways of a set have been recently used.

Another important concept is the **working set** of program, defined as the
collection of data the program actively accesses during a specific time
interval. If the working set entirely fits into the cache, the risk of cache
misses is minimized.

## Cache Coherence

## References

- [[shared_memory_systems]]
