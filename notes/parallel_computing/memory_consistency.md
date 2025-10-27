---
id: memory_consistency
aliases: []
tags:
  - master
  - parallel_computing
---

# Memory Consistency Models

A **memory consistency model** is a set of rules that defines how memory
operations behave and are observed across different threads or processors. It
establishes the order in which operations appear to execute, defining when the
result of a write operation by one thread becomes visible to other threads. It
ensure _correctness_ and can enable or disable _optimizations_.

This is not a rule but the intuition says that a _read_ operation should return
the latest value written in a memory location. As we can immagine, in a
multiprocessor context, this concept is not as straigthforward as in a single
threaded application.

Modern compilers often reoder instructions in memory for optimization purposes,
based on the underlying architecture. Obviously swapped instructions have no
dependecies between each other (like a read and a write on the same variable).

When we deal with _sequential_ code this is not an issue at all as the
dependeciens aren't broken and the final result remains unchanged.

Even in a _lock-based_ concurrent programming this is not an issue, as the
synchronization is done correctly. This is because in a critical section, only
one thread at a time can read or write a memory location, so there is no
ambiguity on which is the latest value written on that location. For the same
reasons, mutexes create a sort of _"safe"_ area, where instructions can be
reordered freely.

In _lock-free_ programming this is not so trivial and must be handled in the
correct way to ensure correctness. Let's consider a situation where we have
three variables `a`, `b` and `flag`, all set to 0. One thread executes

```cpp
a = 1;
b = 1;
flag = 1;
```

while a second thread executes

```cpp
while (flag == 0); // spin
valA = A;
valB = B;
print(valA, valB); // output should be: 1 1
```

If the instruction order is preserved, the second thread cannot proceed beyond
the while loop, until the first thread does not set all the variables to 1. So
the moment the two variables `valA` and `valB` are set, we can be sure that the
printed values will be two ones.

Can happen that the compiler decides to move the `flag = 1` instruction before
`a = 1`, the final result is unpredictable and can change with every execution.

> [!important]
>
> Note that there is no cache coherence mechanism involved with this issue. This
> is not something related with the value stored in some specific private cache,
> that can differ from other the value stored on another cache (considering the
> same variable obviously).

There are four types of memory operation ordering:

- $W_x \to R_y$: write to $x$ must commit before the subsequent read of $y$.
- $R_x \to R_y$: read to $x$ must commit before the subsequent read of $y$.
- $R_x \to W_y$: read to $x$ must commit before the subsequent write of $y$.
- $W_x \to W_y$: write to $x$ must commit before the subsequent write of $y$.

Different memory models may relax some (or all) of these ordering constraints.

## Sequential Consistency

The most intuitive, yet most restrictive memory model is **sequential
consistency**, where all the four memory operation ordering are maintained.

We can say that a multiprocessor system is **sequentially consistent** if the
result of any execution is the same as if the operations of all the processors
were executed in sequential order. The order in which memory operations appear
must be the same that the programmer specified in the code.

Typically this is not implemented by any of the modern architectures, but can be
used as a benchmark for other memory consistency models with more relaxed rules.

## Relaxed Consistency

The main reason for relaxed consistency is to gain performance by hiding memory
latency, for example overlapping memory access operations with other independent
operations.

A common mechanism in every modern processor is the _write buffer_, a queue that
holds data from recent writes not yet commited to main memory. At some point
this buffer will be flushed at the actual write will be completed.

This mechanism introduce a situation where reads can be performed before writes
that are still in the buffer. This has nothing to do with compiler reordering
instructions, it is just something that can happen at hardware level and breaks
the sequential consistency but improves performance, as reads are much faster
than writes.

### Total Store Order and Processor Consistency

Let's now introduce two models where the $W_x \to R_y$ rule is relaxed:

- **Total Store Order** (**TSO**): all writes from each processor are seen in
  the same order by all processors. Loads in a processor may bypass pending stores
  in a write buffer.
- **Program Consistency** (**PC**): writes to different memory locations may be
  observed in different orders by different processors.

These two models ensure that writes to same location are observed in the same
order from any processor.

### Partial Store Order

An even more relaxed model is the **partial store order** (**PSO**) where also
the $W_x \to W_y$ rule is relaxed. This is done for example because the
processor could reoder writes in the write buffer depending on cache misses or
hits.

Many modern architectures have very relaxed memory models that can improve
performances by overlapping operations in a smart way to keep the pipeline as
full as possible. On the other hand this results in a higher programming
complexity.

## Restoring Order

The programmer can restore or enforce stricter memory ordering through some primitives:

- **Fences**: a memory barrier that defines a point where memory operations
  reordering cannot happen. In other words, memory operation that is before the
  fence cannot happen after and viceversa. This means that all operations before
  the fence must complete before any operation after it can start. Fences can be
  specialized only for loads or stores if a so strict mechanism is not needed.
- **Specific location primitives**: primitives that work at finer granularity,
  enforcing a specific order in relation to a specific memory location (_CAS_ or
  _test&set_ operations for example).

Note that a standard mutex performs implicitly the same that a _fence_ does for
what concern the memory operations reodering.

### Data Race Free Guarantee

A **data race** as we know, verifies when two or more threads access and (at
least one thread) modify the same memory location at the same time, without
synchronization between accesses.

If a program is **data race free** is sequentially consistent and all operations
behave as if they occurr in a single global order that respects each thread's
program order.

The _sequential consistency_ alone does not automatically guarantee that a
program is data race free; programmers must explicitly ensure it by adding
directives to instruct the compiler on the correct memory operations order.

The rationale behind memory reordering is the principle of "**optimize for the
common case**", since the most memory accesses do not conflict, architectures
avoid incurring in the overhead of strict ordering for every operation.

## References

Links: [[parallel_computing]], [[lock_free]]
