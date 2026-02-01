---
id: work_span_model
aliases: []
tags:
  - master
  - parallel_computing
---

# Work-Span Model

One of simplest model is the **Work-Span** model, which gives us more strict
bounds to compared to Amdahl's and Gustafson's laws.

It represents the parallel program as a set of tasks that can be dependencies of
each other, organized as **DAG** (Directed Acyclic Graph).

![[work_span_dag.png]]

Every task can be executed if and only if all its dependencies were completed.

The model makes also some **assumptions** on the architecture of the system
where the algorithm is executed:

- The machine has $p$ identical processors.
- The machine has a **greedy scheduler** that basically execute any ready task
  without any overhead.

The greedy scheduler basically execute task whose dependencies have already been
satisfied as long as there is an available processor.

That been said, the completion time $T_p$ is defined as the time needed to execute
the algorithm with $p$ identical processors and a _greedy scheduler_. There are
also two special values of $p$ that give the name to the model:

- $T_1$ is the **work** and is the time taken by the sequential version of the
  algorithm.
- $T_\infty$ is the **span** and is the theoretical time taken by the algorithm
  if executed with an infinite amount of processors.

The _span_ is also called **critical path** because it represents the DAG's
_longest_ path starting from the first to the final level of the graph.

![[work_span_critical_path.png]]

Note also that the _work_ is the sequential version of the algorithm, not the
parallel version with only one processor.

## Speedup Bounds

The speedup is computed as usual but we can make say, depending on the level of
parallelism reached, the speedup respects the following inequality

$$S(p) = \frac{T_1}{T_p} \leq \frac{T_1}{T_1 / p} = p$$

that basically states that the maximum speedup achievable is _linear_ in the
number of processors $p$, assuming that

- the algorithm is perfectly parallelizable
- the greedy scheduler does not add any overhead

This also means that the completion time has a lower bound, directly deduced by
the formula above:

$$T_p \geq \frac{T_1}{p}$$

Another limit for the speedup is obviously that we cannot do better than the
version with $p = \infty$ processors:

$$S(p) = \frac{T_1}{T_p} \leq \frac{T_1}{T_\infty}$$

which bring another lower bound on the completion time

$$T_p \geq T_\infty$$

that it is a little bit less interesting than the other because, with $p$ fixed,
it is generally true that

$$T_p \geq \frac{T_1}{p} \geq T_\infty$$

However $T_\infty$ is still an interesting measure to define the very edge
limits of the algorithm with this model.

> [!note] Brent's Theorem
>
> Let's assume
>
> - that every task takes 1 time unit to complete
> - a greedy scheduler
> - to have a machine with enough processors that the algorithm finishes in
>   $T_\infty$ time step
>
> A machine with fewer processors (let's say $p$) has the following completion
> time upper limit
>
> $$T_p \leq \frac{T_1 - T_\infty}{p} + T_\infty$$
>
> > [!note]- _Proof_
> > The proof is based on the fact that if the _DAG_ has $n$ levels, at
> > each level $i$ we have to compute $m_i$ operations. With $p=1$ we have that
> > the total time is
> >
> > $$T_1 = \sum_{i=1}^n m_i$$
> >
> > and with $p = \infty$ we have that
> >
> > $$T_\infty = \sum_{i=1}^n 1 = n$$
> >
> > So now we can say that with $p > 1$ processors we have
> >
> > $$
> > T_p = \sum_{i=1}^n \bigg\lceil \frac{m_i}{p} \bigg\rceil \leq
> > \sum_{i=1}^n \frac{m_i + p - 1}{p}
> > $$
> >
> > Let's conclude the proof by some calculations like
> >
> > $$
> > \begin{align*}
> > \sum_{i=1}^n \frac{m_i + p - 1}{p} &= \sum_{i=1}^n \frac{m_i}{p} +
> > \sum_{i=1}^n \frac{p}{p} - \sum_{i=1}^n \frac{1}{p} \\
> > &= \frac{1}{p} \sum_{i=1}^n m_i + n - \frac{n}{p} \\
> > &= \frac{T_1}{p} + T_\infty - \frac{T_\infty}{p}
> > = \frac{T_1 - T_\infty}{p} + T_\infty
> > \end{align*}
> > $$
> >
> > which concludes the proof.

The Brent's theorem of course has some implications relative to completion time
and speedup. For example, if we consider the speedup we have that

$$S(p) \leq \min \left( p, \frac{T_1}{T_\infty} \right)$$

and this is deduced by the previous lower bounds for the completion time.
Obviously if lower $T_\infty$, the maximum achievable speedup will become
greater. This also mean that to have a good speedup $T_1$ must be significantly
larger than $T_\infty$ so that

$$T_1 - T_\infty \approx T_1$$

and the Brent's formula becomes

$$T_p \approx \frac{T_1}{p} + T_\infty$$

if $T_\infty \ll T_1$. So when designining an algorithm the focus must be on
reducing the _span_ because it is the fundamental asymptotic limit on
scalability. We can now deduce a lower bound for the speedup as follows

$$
S(p) \geq \frac{T_1}{\frac{T_1}{p} + T_\infty} =
\frac{p \cdot T_1}{T_1 + p \cdot T_\infty} =
\frac{p}{1 + p \cdot (T_\infty / T_1)}
$$

Overall we have the following lower and upper bounds for time

$$\frac{T_1}{p} \leq T_p \leq \frac{T_1}{p} + T_\infty$$

and for speedup we have

$$
\frac{p}{1 + p \cdot (T_\infty / T_1)} \leq S(p) \leq
\min \left( p, \frac{T_1}{T_\infty} \right)
$$

and can be proved that

$$S(p) = \frac{T_1}{T_p} \approx p$$

if $T_1 / T_\infty \gg p$.

---

That been said, the greedy scheduler achieve (almost) linear speedup if the
problem is **overdecomposed** to create much more parallelism than the number of
processors and this generally takes the name of **parallel slack** that is
defined as $\frac{T_1}{T_\infty}$.

### Limitations

We also have to remember that all the formulas seen before assume

- No parallel overhead.
- The memory bandwidth is not a limiting resource.
- The scheduler is greedy in scheduling tasks.

All assumptions that cannot be implemented in a real context.

## References

- [[parallel_computing]]
