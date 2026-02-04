---
id: farm
aliases: []
tags:
  - master
  - parallel_computing
---

# Farm

A **farm** is a parallel pattern that improve efficiency by **replicating** the
same (often _stateless_) function $F$ $k$ times. Each function replica is
executed by a **worker**. The goal is to improve the throughput of one single
stage of the computation.

The most common pattern involves an initial phase of task distribution, a
computation phase and a final phase in which the results are gathered.

![Farm|700](farm.png)

Often the notation to describe farms involve the definition of two entities in
addition to worker, **emitters** and **collectors**.

In real implementations (farm skeletons) emitters and collectors usually are not
standalone implemented entities and it is the worker the does the emmitter and
collector job. In other cases can happen that emitters and collectors are
implemented to better overlap communication and communication, for example in a
multithreaded and distributed application.

![Farm Skeletons](farm_skeletons.png)

In order to reduce the cost of a sequential communication, the farm can
implemented with a tree of emitters and collectors, trying to achieve a
logarithmic communication time.

## Cost Model

The cost model can vary a lot, depending on the implementation; for simplicity
let's consider a skeleton with one emitter and one collector, which take the
same amount of time to distribute and collect tasks, respectively.

$$T_s^e = T_s^c$$

The farm has $k$ workers and the emitter's service time is lower than the
inter-arrival time and workers' service time

$$T_s^e < T_a \quad \land \quad T_s^e < T_s^w$$

If $n$ tasks are submitted, the **completion time** is

$$
T_c (n, k) = (k + 1) \cdot T_\text{comm} + \frac{n}{k} \cdot
(T_s^w + T_\text{comm})
$$

![Farm Completion Time](farm_time.png)

In general, given a farm with $k$ workers, each with a service time of

$$
T_s^w (F) = \begin{cases}
T_s^\text{seq} + T_\text{comm} & \text{no communication overlap} \\
\max{(T_s^\text{seq} (F), T_\text{comm})} & \text{otherwise}
\end{cases}
$$

The service time is

$$
T_s^\text{farm} (F, k) =
\max{\left( \frac{T_s^w (F)}{k}, T_s^e, T_s^c \right)}
$$

The task latency is

$$L_\text{farm} = T_s^w (F) + T_s^e + T_s^c$$

The completion time for $n$ tasks is

$$T_c^\text{farm} (n, k) = (k+1) \cdot T_\text{comm} + \frac{n}{k} \cdot T_s^w$$

A farm is a _bottleneck_ if its service time is higher than the inter-arrival
time:

$$T_s^\text{farm} (F) > T_a$$

therefore the minimal $k$ that ensures the stage not being a bottleneck is

$$k_\text{opt} = \left\lceil \frac{T_s^w (F)}{T_a} \right\rceil$$

## Task Scheduling

The emitter must assign input tasks to workers in order to ensure an even
workload balancing. In practice the right choice depends on the problem and on
the computational payload of each task.

Let's suppose that the time needed to compute a task is more or less the same
for every tasks

$$F(x_i) \approx F(x_j)$$

then a **round robin** task scheduling could be effective. Otherwise, in case of
heterogeneous tasks an **on-demand** task scheduling could be better so that
each worker is feeded as soon as it finish the previous computation.

The possible implementations can vary a lot and, based on the context a _work
sharing_ or a _work stealing_ approach can be a good option.

## Preserve Ordering

A farm described as above does not ensure output ordering, and depending on the
problem is not even necessary. Order the output should not be done (unless is
necessary) to improve performance and reduce the logic complexity of the
algorithm.

Sometimes the order is important so it becomes necessary to add some logic to
ensure it. A common way is to bind every task with a numerical and monotically
increasing ID, that can be used in the end to find the correct location for the
computed result.

## Stateful Functions

The most common scenario is that a task submitted to a farm is _stateless_, but
there are cases where a state should be hold, and that should be handled
properly.

Holding a state and ensure consistence can be challenging in a distributed
system, not only for performance issues, but also for an higher logic
complexity.

The first scenario is the **read-only state** where, in case of distributed
memory systems, each worker hold a replica of the state and there is no need for
a synchronization beyond the initial data distribution. For shared memory
systems a single shared copy of the state is sufficient.

For **read-write state** there are two main distinctions:

- **Monolitich state** (non partitionable): the entire state must be updated for
  all workers whenever a write occurs.
- **Partitionable state**: the state can be divided into disjoint segments so
  each worker handles a portion independetly.

In the first case the cost scales with the frequency of writes and the
portability of updates. For shared memory systems a usual implementation
involves a _multiple reader single writer_ approach, meanwhile for distributed
systems there are _multicast_ based approaches to notify workers that a write
occurred.

In the other case it is common to apply some kind of _hashing_ to the task that
in order to identify the right partition and send the task to the proper worker.

## References

- [[structured_parallel_programming]]
- [[pipeline_farm]]
