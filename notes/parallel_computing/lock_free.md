---
id: lock_free
aliases: []
tags:
  - master
  - parallel_computing
---

# Lock-Free Programming

An alternative to the _lock-based_ approach is the so called **lock-free**
approach, where race conditions and concurrency are handled without mutexes or
condition variables.

Starting from C++11, **atomic data types** were introduced to handle concurrency
scenarios without the need to acquiring locks. These atomic types are made to
support **atomic operations**, that are executed entirely or not at all.

For scenarios with high level of concurrency, atomic operations can be faster
than using mutexes and they avoid some of the typical _lock-based_ situations,
like:

- Thread **suspension**/**resume** overhead.
- **Deadlock** conditions.
- **Priority inversion**: a thread with high priority is waiting to acquire a
  resource held by a low-priority thread.

So with _lock-free_ programming we can avoid some issues, but other problems are
introduced. In general there are problems with _starvation_, that can happen
also with mutexes, and something called **livelock**, in which threads
continuously interfere with each other so that there is no real progress.

So in this context three main **progress guarantees classes** were defined,
starting from strongest, we have:

- **Wait-freedom**: every thread completes its operation in a finite number of
  steps, independently of other threads. This is the the strongest class and
  avoid starvation.
- **Lock-freedom**: guarantees that at least one thread makes progress.
- **Obstruction-freedom** guarantees progress only if a thread runs without
  contention.

The C++ atomic operations typically provide _lock-freedom_ guarantees.

## Lock-free operations

The `std::atomic` type is neither copyable nor movable and the type `T` passed
has to be trivially copyable (data copyable with `memcpy` like operations). The
basic operations provided by the `std::atomic` class are:

- **store** and **load**: write and read the value.
- **exchange**: write a new value and returns the old one.
- **compare and exchange**: exchange only if the value is equal to the
  _expected_ one.
- **fetch operations**: perform a read-modify-write operation.
- **wait and notify**: available from C++20, provide a behaviour similar to
  condition variables.

Some of these operations might be implemented or not, depending of the type `T`
passed as template parameter. For basic types like integers, all operations are
available but, te be sure a query can be done with the
`std::atomic::is_lock_free` function.

### CAS - Compare And Swap

One of the most useful and powerful atomic operations is the **CAS** that let us
make atomic assignments based on an expected value. In C++ we have two methods

- `compare_exchange_weak`: more efficient but can fail and returns `false`.
- `compare_exchange_strong`: it always produces the expected result but it is
  less efficient.

The semantics for this operation are:

1. Compare the _expected_ value with the value stored in the atomic variable.
2. If they are equal, the _desired_ value will be stored in the atomic,
   otherwise writes the value stored in the atomic to the _expected_.
3. Returns `true` if the swap operation in step 2 was successful, `false`
   otherwise.

The signature of the method is something like

```cpp
value.compare_exchange_weak(T& expected, T desired);
```

and should always be performed in a loop.

## Memory Ordering

Related to memory consistency models, the C++ standard defines a model that
ensure sequential consistency for data race free programs. With C++ atomics we
ensure both data race free condition and sequential consistency for atomic
variables only (specific) and also for non atomic variables with _fences_.

The standard provides six memory ordering options for atomic types:

- `std::memory_order_relaxed`: all memory ordering are relaxed.
- `std::memory_order_consume`: not implemented.
- `std::memory_order_seq_cst`: sequential consistency.
- `std::memory_order_acquire`: only valid for load operations and prevents other
  load or store to be moved before it.
- `std::memory_order_release`: only valid for store operations and prevents
  other loads or stores to be moved after it.
- `std::memory_order_acq_rel`: only valid for atomic exchange, compare-exchange
  and fetch operations. The load is done with an acquire and the store with a
  release memory ordering.

The default for any atomic operation is `std::memory_order_seq_cst` so it can be
omitted, but it is also the most costly because prevents compiler and
architectures reordering optimizations of any kind.

## Barriers

A mechanism to define a strong synchronization point is the **barrier**, which
ensure that all threads reach the barrier before they can go on. Barriers are
also reusable, so that it can be used in iterative or phased algorithms where
you need multiple barriers points across iterations.

The barrier behaviour is blocking but involves context switch overhead, so when
we expect a very low waiting time on the barrier, a non blocking implementation
could be faster (**spin barrier**).

## ABA Problem

As lock-based data structures there are also lock-free data structures, that can
be significantly faster but not so easy to implement. This is due to the fact
that with lock-free programming we have a fine-grained level of safe access to
data.

We cannot define an entire region of code that must be executed atomically
before another thread modifies the same data. So we have the problem that single
instructions are executed atomically, there is no such thing as a mutex that
protects an entire set of complex instructions.

One big problem is the so called **ABA problem**, that verifies when one thread
reads the value $A$ of a memory location and get suspended. Another thread
modifies the value $A$ to $B$ and then back to $A$. The previous thread, when
resumed, cannot know that a intermediate change occurred.

This is a problem typically with pointer-based data structures, where moving
pointers, allocating and freeing memory can lead to a dangling pointer
inconsistence in the end.

<!-- example with stack -->

The common solution to this problem is to add an extra **tag** or **version
number** to the data, that is incremented every time the pointer is updated.

The CAS operation then must be done on both the pointer and the tag as a single
atomic operation. In this way the CAS will fail even if the pointer address is
the same, because the tag has been incremented.

The problem is that a **CAS2** (double-word CAS) instruction is needed but is
not supported by all architectures, so a more portable implememntation is
**deferred reclamation** that prevents node reuse while pending requests exist
and **hazard pointers** are used for safe reclamation of memory.

## References

- [[parallel_computing]]
- [[memory_consistency]]
