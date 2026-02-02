---
id: amdahl_law
aliases: []
tags:
  - master
  - parallel_computing
---

# Amdahl's Law

The **Amdahl's law** assumes that a program is composed by various parts which
must be _serial_ and some parts that can be _parallelized_.

This law describes the theoretical limit on the achievable speedup when using
multiple processors for a fixed problem size. It states that the speedup is
fundamentally limited by the **fraction of sequential code**. The upper bound
described is often unrealistic and really far away from the actual speedup
obtained in reality.

To derive the Amdahl's law we consider the completion time of the serial version

$$T_c (1) = T_\text{ser} + T_\text{par}$$

where $T_\text{ser}$ is the serial part that cannot be parallelized and
$T_\text{par}$ is the part the _can_ be parallelized. We now assume that the
best achievable speedup is linear, from which we can derive an upper bound.

$$T_c (p) \geq T_\text{ser} + \frac{T_\text{par}}{p}$$

So now we can compute the speedup like

$$
S(p) = \frac{T_c (1)}{T_c (p)} \leq \frac{T_\text{ser} +
T_\text{par}}{T_\text{ser} + \frac{T_\text{par}}{p}}
$$

Instead of using the absolute execution times, lets now use their relative
fraction; $f$ is the serial fraction whereas $1-f$ is the parallelizable
fraction. The completion time is now expressed in terms of

$$
\begin{align*}
T_\text{ser} &= f \cdot T_c (1) \\
T_\text{par} &= (1-f) \cdot T_c (1)
\end{align*}
$$

with $0 \leq f \leq 1$. So the speedup becomes

$$
S(p) = \frac{T_c (1)}{T_c (p)} \leq \frac{T_\text{ser} +
T_\text{par}}{T_\text{ser} + \frac{T_\text{par}}{p}} =
\frac{f \cdot T_c (1) + (1-f) \cdot T_c (1)}
{f \cdot T_c (1) + \frac{(1-f) \cdot T_c (1)}{p}} =
\frac{1}{f + \frac{(1-f)}{p}}
$$

So we can use this law to predict the performance of a parallel program
providing the serial and parallel fractions.

Another interesting thing we can do is to put $p = \infty$ and see what happens

$$\lim_{p \to \infty} \frac{1}{f + \frac{1-f}{p}} = \frac{1}{f}$$

Therefore strong scalability is limited by the serial fraction. We can of course
use this value to make some predictions on the efficiency:

$$E(p) = \frac{S(p)}{p} = \frac{1}{p \cdot f + (1 - f)}$$

## Overhead

The Amdahl's law described above does not take into account the
**parallelization overhead**, so it tends to be _optimistic_. A more realistic
formula can be

$$S(p) \leq \frac{1}{[f + O(p)] + \frac{1-f}{p}}$$

where $O(p)$ is a function that takes into account the overhead.

This formula assumes that the overhead is part of the serial part but could not
always be the case. For example we can have parallel communication among
processes, which is still overhead but can be better than serial communication.

## References

- [[parallelization_laws]]
