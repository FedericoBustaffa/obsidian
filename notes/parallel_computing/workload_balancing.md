---
id: workload_balancing
aliases: []
tags:
  - master
  - parallel_computing
---

# Workload Balancing

One of the main problems with parallel computing is **workload balancing**; the
way to distribute work and data among workers is not always trivial and can be
done in various ways.

First of all, a problem complexity is not always bounded to the input size like
in matrix multiplication or, in general algebraic operations. That kind of
operations do not take into account the magnitude of the summed or multiplied
numbers, they can be parameters and the algorithm complexity will remain the
same. So in this case we can think about some way to partition the matrices and
get an improvement.

Let's now suppose to have an unordered array of integers as input and we want to
obtain another array whose elements are the Fibonacci's function computed on
every elements of the input array.

$$A_i = \text{Fibonacci}(B_i)$$

In this case we could partition the array in $p$ equal parts where $p$ is the
number of processors but what if the array is something like this

$$B = \{ 1, 2, 2, 4, 40, 30, 100, 78 \}$$

Suppose to have $p = 2$ workers and split the array in two parts; the first
worker will get values that are computationally less expensive and will end its
work way sooner than the other.

## Static Distribution

One of the methods to distribute work among workers is **static distribution**,
where

- We have to compute the data partition before the start of the program.
- The partition is fixed in size and once chunks of data are _assigned_ to a
  partition they cannot be moved anymore.
- The output could also be partitioned so we need to spend some more effort at
  the end to build the final result.
- Each partition have more or less the same size in terms of
  - _Data_: if the algorithm complexity is bounded to the number of elements of
    the input (matrix multiplication).
  - _Workload_: if the algorithm complexity is bounded to the single unit
    computation we have to make (Mandelbrot set).

### Block Distribution

The most simple way to distribute work statically is the **block distribution**
method, where a partiton has $\lfloor n / p \rfloor$ where $n$ is the input size
and $p$ the number of workers.

A little problem with this method is that if

$$n \mod p \neq 0$$

we could end up in a situation where there are elements left without being
assigned to a partition. For example $n = 16$ and $p = 5$

$$\lfloor n / p \rfloor = 3$$

but $3 \cdot 5 = 15$ so an element left behind. A simple way to fix this is to
account also the carry of the division, so that the processor $p_i$ receives a
block of size

$$
\begin{cases}
\lceil n / p \rceil & \text{if } i < k \\
\lfloor n / p \rfloor & \text{otherwise}
\end{cases}
$$

where $k = n \mod p$ is the carry of the division.

![[block.png]]

---

This method is good for caching because all the elements assigned to a worker
are adjacent in memory. On the other hand the method is not very flexible and
could not be ideal we heterogeneous tasks.

### Cyclic Distribution

This kind of distribution _fix_ the problem of handling the carry in a natural
way consuming all the elements one by one, assigning them to workers in a cyclic
way.

![[cyclic.png]]

So if we think about an array of elements like before, a task (one element of
the array) $t_i$ is assigned to the worker $p_{i \mod p}$. In this way we can
consume all the array elements until the last obtaining the best possible
partitioning in terms of _tasks per worker_.

---

Good for easily partition the problem but suffers from poor cache locality and
false sharing. The false sharing particular problem could be avoided or
mitigated with local variables used as accumulators.

### Block-Cyclic Distribution

The last is a mix between the two where we still assign tasks in a cyclic
fashion but instead of having a granularity of $1$ we have a granularity of $c$
that is the **chunksize**.

With this method the task $t_i$ is assigned to processor
$p_{(i \text{ div } c) \mod p}$

![[block_cyclic.png]]

In this particular scenario we also have a value called **stride**, obtained by
multiplying the number of processors with the chunksize

$$\text{stride} = p \cdot c$$

and it is, given $i$, the distance between the element $i$ of the chunk $j$ and
element $i$ of the chunk $j+1$ in the same block.

## Dynamic Distribution

In some cases we don't have enough information to static partition the problem
or the data structure, so we have to rely on a dynamic approch.

This can also be very useful with very heterogeneous tasks because, if well
implemented, the workload distribution is adjusted at runtime.

Some special cases of cyclic or block-cyclic static distribution can in some way
approximate a dynamic distribution, but there main difference is that the static
way of distributing tasks is done _before_ the actual computation. A dynamic
method starts immmediately the computation and the workers are feeded as soon as
they are available.

It is also possible, if we notice some kind of structure in the tasks, to make
some optimizations, for example defining the single task size like in static
method of tasks distribution. Obviously this does not change the dynamic nature
of the method.

It is also possible to optimize and compute dynamically the task size but this
is not trivial at all and can lead to more overhead than a simpler method.

### Thread Pool

One of the main implementation of a dynamic workload balancing method is the
**thread pool**. A very simple implementation has a fixed number of threads,
spawned at the beginning and that will wait for tasks until the thread pool
lives.

To deliver tasks we can think about many ways and policies but one of the
simplest relies on a single **task queue**. The main thread push tasks into the
queue, while the workers access it concurrently trying to get jobs as soon as
they finish the previous.

The implementation follows the _producer-consumer_ paradigm where the queue can
be bounded or unbounded, depending on the scenario and the quantity of tasks
that can possibly be pushed into the queue.

Usually the number of workers is lower or equal than the number of available
cores on the machine and, to prevent big too expensive memory usage the bounded
queue has a blocking behaviour in case it fills up completely.

## References

Links: [[parallel_computing]]
