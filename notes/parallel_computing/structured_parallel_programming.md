---
id: structured_parallel_programming
aliases: []
tags:
  - master
  - parallel_computing
---

# Structured Parallel Programming

Parallel programming is not easy and always have to find a way to parallelize a
program can be difficult. It is also true that in most cases there are recurring
patterns to achieve parallelization that can be generalized and reused.

This is called **structured parallel programming** and its aim is to simplify
the development of parallel applications by _composing_ them from a small set of
recurring patterns mentioned before.

This approach have pros and cons and in every case is up to the programmer
decide the best approach. In general we can say that the structured approach is
more readable, maintainable and more high-level. In fact it provides patterns
and implementations that a non expert in parallel computation can use.

On the other side the unstructured approach can (or should) be more efficient in
terms of performance. The first motivation for an unstructured approach is most
of the time the need to build an ad-hoc algorithm for the problem, trying to
exploit all the possible optimizations. This approach of course is difficult to
maintain and it is more error prone, because involve the usage of low level
synchronization mechanisms.

So in most cases the structured approach is better because there aren't so many
ways of parallelize an algorithm and it's also easier to think about how to
reduce a given problem to a combination of the known "**forms**".

## Terminology

Before go on let's introduce some terminology, typical to structured approach:

- **Parallel patterns**: recurring high-level abstractions that describe common
  ways to organize parallel computation (pipeline, farm, scan, map ecc.).
- **Algorithmic skeletons**: concrete implementations or library constructs that
  encapsulate specific parallel patterns into ready to use parallel components.
- **Parallel paradigms**: broad conceptual frameworks or model for organizing
  and thinking about parallel computation. They provide an overall classification
  of parallelism approaches, incorporating both structured and unstructured
  methodologies.

So, for a more theoritical view we must concentrate with parallel patterns, then
we will analyze some skeleton by reviewing some library.

## Stream Parallelism

Another aspect to take into account is **stream parallelism**, which is the way
tasks flow from a _computation module_ to another. In other works a **stream of
tasks** is an ordered sequence of a computational or operational units (called
tasks) of the same type that are processed one after the other:

Each task in the stream typically represents a discrete piece of work, such as a
computation, data transformation, or process operation, which contributes to the
overall goal of the system or application.

Stream **source** and **sink** are respectively the first and last stages of the
computation workflow that produce and consume the stream of tasks.

This can be related in some sense to the computation and communication overlap
that we try to obtain in distributed memory systems.

## References

- [[parallel_computing]]
- [[pipeline]]
- [[farm]]
- [[map]]
- [[reduce]]
- [[stencil]]
- [[divide_and_conquer]]
- [[fastflow]]
