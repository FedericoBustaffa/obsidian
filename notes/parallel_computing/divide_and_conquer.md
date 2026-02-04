---
id: divide_and_conquer
aliases: []
tags:
  - master
  - parallel_computing
---

# Divide And Conquer

This pattern is commonly used to _divide_ problems in subproblems, until the reach
of a _minimal_ subproblem that is usually the base case. Then the subproblem is
_solved_ and _merged_ with the result of another subproblem of the same size.

The **divide and conquer** pattern is a bit different from the map pattern in
which the division is only on data. The main difference is that, dividing data
in more partitions let the computation to be faster because each worker needs to
compute less data. However the computational cost of each data unit is the same.

In the divide and conquer approach, the computational cost of the base case is
often better than computing even a "_little_" problem. Also the merged
subproblem created are usually less costly to solve.

So, even if the divide and conquer can be implemented like a map skeleton, the
concept is different and can lead to a more efficient algorithm.

![Divide and Conquer|400](divide_conquer.png)

Achieving efficient parallel DC requires addressing two challenges:

- Scheduling the subproblems across processors to have good load balancing. A
  dynamic scheduling strategy is usually employed.
- Combining results without creating bottlenecks.

To avoid excessive overhead, a threshold value in the _divide_ phase is
necessary. In other words a predefined problem size below which the divide phase
stops must be defined. Another crucial point is that the overhead of recursive
division and task distribution has to be contained in order to do not outweight
the benefits of a parallel execution.

## Implementations

The are two primary options depending on if the workload is balanced or
unbalanced among workers:

- **Balanced workload**: to have a balanced workload it means that subproblems
  all have almost the same computational cost and it is possible to split the
  initial collection in at least $k$ partitions. In this case the implementation
  is a simple _map_ pattern where we have a _divide + scatter_ phase, a _conquer_
  phase and in the a _gather + collect_ phase.
- **Unbalaced workload**: in this case a _farm_ skeleton with dynamic scheduling
  policy is preferred, with the three steps executed in pipeline. For example we
  can have a _work-stealing_ implementation with _coarse-grain_ divide, then
  each process keep dividing and storing in a local task queue. When the queue
  is empty some work stealing from other processes queues is attempted.

## Cost Model

For the divide and conquer parallel pattern the completion time is

$$
T_c^\text{DC} ({C}, k) = T_\text{div} + T_\text{conq} + T_{ws-term} +
T_{coll}
$$

where

$$
T_\text{div} = 2 \cdot \sum_{i=1}^{\lceil \log_2 k \rceil} T_\text{comm}
\left( \frac{n}{2^i} \right) + \frac{T_\text{divide} ({C})}{k}
$$

is the scatter and per-worker cost to divide the collection.

$$T_\text{conq} = \beta \cdot \frac{D}{k} \cdot T_\text{conquer}$$

is the time to _conquer_ the local subproblems and where $\beta = 1$ assumes
perfect load balancing among workers and $\beta > 1$ means the workload is
unbalanced.

$$
T_\text{ws-term} = c \cdot T_\text{steal} + \lceil \log_2 k \rceil \cdot
T_\text{comm} (1)
$$

is the cost of the steal attempt (without success) and barrier cost and where
$c$ is a constant number of attempts to steal work, after such attempts there
must be an assumption that the conquer phase is going to finish.

$$
T_\text{coll} = 2 \cdot \sum_{i=1}^{\lceil \log_2 k \rceil} T_\text{comm}
\left( \frac{n}{2^i} \right) + \lceil \log_2 k \rceil \cdot T_\text{merge}
$$

is the gather and merging cost for all $D$ subproblems, where the cost of
merging can be linear in $D$ or logaritmic for tree-like structures.

---

So in the end we can say that the divide and conquer is a naturally
parallelizable pattern that is also potentially scalable for systems with many
processors.

On the other hand there is a high communication overhead, also due to the fact
that the for some problems it is difficult to statically partion subproblems, so
a dynamic and more costly scheduling is needed.

It's also not easy to to find the threshold under which not to go for the
_divide_ phase. Also the _merge_ phase can be complex and challenging to do in
parallel.

## References

- [[structured_parallel_programming]]
