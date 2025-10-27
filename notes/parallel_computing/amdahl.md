---
id: amdahl
aliases: []
tags:
  - master
  - parallel_computing
---

# Amdahl's Law

The **Amdahl's Law** assumes that a program is composed by various parts which
must be _serial_ and some parts that could be _parallelized_.

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

> [!example]-
>
> Lets suppose we have $f = 0.05$ and $6$ CPUs. The Amdahl's law says that the
> maximum achievable speed up is
> $$S(6) \leq \frac{1}{0.05 + \frac{(1-0.05)}{6}} = 4.8$$
> from which we can derive a maximum efficiency of
> $$E(p) = \frac{4.8}{6} = 0.8$$
> that is an $80\%$ of efficiency.

Another interesting thing we can do is to put $p = \infty$ and see what happens

$$\lim_{p \to \infty} \frac{1}{f + \frac{1-f}{p}} = \frac{1}{f}$$

so in general we can say that the speedup with $p = \infty$ tends to

> [!example]-
>
> Lets now consider a problem with $f = 0.1$, a number of CPUs that is
> $p=\infty$ and apply the Amdahl's law
> $$S(\infty) \leq \lim_{p \to \infty} \frac{1}{0.1 + \frac{0.9}{p}} = 10$$
> which gives us a strong upper bound to the performance.

Therefore strong scalability is limited by the serial fraction. We can of course
use this value to make some predictions on the efficiency:

$$E(p) = \frac{S(p)}{p} = \frac{1}{p \cdot f + (1 - f)}$$

## Overhead

The Amdahl's law described above does not take into account the
**parallelization overhead**, so it tends to be _optimistic_. A more realistic
formula can be

$$S(p) \leq \frac{1}{[f + O(p)] + \frac{1-f}{p}}$$

where $O(p)$ is a function that takes into account the overhead.

> [!example]-
>
> If we consider a problem with linear overhead; the formula becomes
> $$S(p) = \frac{1}{f + p \cdot o + \frac{1-f}{p}}$$
> where $o$ is the overhead that must be payed for every processor used.

This formula assumes that the overhead is part of the serial part but could not
always be the case. For example we can have parallel communication among
processes, which is still overhead but can be better than serial communication.

## References

Links: [[laws]]
