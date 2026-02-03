---
id: pipeline
aliases: []
tags:
  - master
  - parallel_computing
---

# Pipeline

The **pipeline** is a parallel design pattern that enhances computational
efficiency by dividing a computation into a sequence of stages, where each stage
processes data and passes its output to the next stage.

The stages operate in parallel, enabling overlapping computation and improved
throughput.

![Pipeline](pipeline.png)

In general it can be applied on problems where a big computation $F(x)$ can be
divided in something like

$$F(x_i) = f_k (f_{k-1} (\dots f_1 (x) \dots))$$

so instead of computing $F(x_i)$ sequentially in one big step, it's possible to
distribute the computation over $k$ stages, each one of them computing their
part.

This is efficient only if we have an sequence of tasks $X = \{x_1, \dots, x_n\}$
that must be executed independently.

Another important aspect to take into account is communication:

- The pipeline has to receive the input and return the output.
- The stages must communicate to each other.

In general, the pipeline is feeded with tasks and produces outputs continuously.

## Metrics

Supposing that the stream of tasks is more or less continuous, the pipeline go
through three phases over time: **fill**, **steady** and **draining**.

When all stages are working in parallel we can talk about **steady state
phase**, where all different computations of different $f_i$ are overlapped,
providing a speedup that can be approximated with $k$ (the number of stages).

Let $T_F$ be the time to compute $F(x)$ for every input $x$ and suppose that $F$
can be splitted in $k$ stages, with each stage taking $T_f \approx T_F / k$ time units for each
input $x$.

Supposing we need to compute $n$ tasks, the sequential time becomes
$n \cdot T_F$, with a throughput of

$$\frac{n}{n \cdot T_F} = \frac{1}{T_F}$$

but if the $k$-stages pipeline is feeded with the same amount of tasks, the
speedup is approximately

$$S(k) = \frac{T_F}{T_f} = \frac{T_F}{T_F / k} = k$$

while the throughput at steady state is

$$
\frac{n}{n \cdot T_f} = \frac{1}{T_f} = \frac{1}{T_F / k} =
k \cdot \frac{1}{T_F}
$$

that is approximately $k$ times greater. Let's now consider also the
communication time between stages $T_\text{comm}$; for a single task going
through the pipeline costs

$$k \cdot (T_f + T_\text{comm})$$

but considering $n$ tasks and the computation and communication overlapping, we
can say that now the total time is approximately

$$T_\text{pipe} (n) = n \cdot (T_f + T_\text{comm})$$

for a speedup of

$$
S(k) = \frac{T_F}{T_f + T_\text{comm}} =
\frac{T_F}{\frac{T_F}{k} + T_\text{comm}} =
k \cdot \frac{T_F}{T_F + k \cdot T_\text{comm}}
$$

and a throughput at steady state of

$$
\frac{1}{T_f + T_\text{comm}} =
k \cdot \frac{1}{T_F + k \cdot T_\text{comm}}
$$

> [!NOTE]
> Latency is worse than the serial version because of the overhead introduced
> with the communication between stages. From the standpoint of one individual
> task there is no improvement but a worsening:
>
> - All the stages are executed one after the other.
> - There is communication overhead between stages.

In general a $k$-stages pipeline with balanced stages has a completion time of

$$T_c^\text{pipe} = (n + k - 1) \cdot (T_s^\text{stage} + T_\text{comm})$$

if we consider that the _sink_ stage sends out the result. In case of unbalanced
stages, the whole pipeline completion time is conditioned by the slowest stage.

Let's suppose to have three stages where $T_s^i$ is the completion time of the
stage $i$, with these values

$$T_s^1 = 15 \quad T_s^2 = 25 \quad T_s^3 = 12$$

Let's also suppose that, like before, $T_\text{comm} = 5$ and $n = 300$. In
this way we have sequential completion time of

$$T_c^\text{seq} = n \cdot T_s^\text{seq} = 15600$$

and for the parallel version we have a completion time of

$$
T_c^\text{pipe} = (T_s^1 + T_\text{comm}) +
n \cdot (T_s^2 + T_\text{comm}) + T_s^3 = 9032
$$

That is in fact a similar time as if all stages have the service time of the
slowest stage (in this case stage 2):

$$T_c^\text{pipe} = (300 + 3 - 1) \cdot (25 + 5) - 5 = 9055$$

So in this case, when the pipeline reach the steady state, the slowest stage
(also called **bottleneck stage**) is the one the dictates the pipeline's
service time.

## Cost Model

Given a pipeline of $k$ stages such that

$$T_s^\text{seq} (F) = \sum_{i=1}^k T_s^i (f_i)$$

- The **service time** is
  $$T_s^\text{pipe} = \max_{i=1 \dots k}{(T_s^i (f_i))}$$
  where
  $$
  T_s^i (f_i) = \begin{cases}
      T_{f_i} + T_\text{comm} & \text{if no communication overlap} \\
      \max{(T_{f_i}, T_\text{comm})} & \text{otherwise}
  \end{cases}
  $$
- The task **latency** is
  $$L_\text{pipe} = \sum_{i=1}^k T_s^i (f_i) + (k-1) \cdot T_\text{comm}$$
  with no communication overlap accounted.
- The **completion time** for $n$ tasks is
  $$T_c^\text{pipe} (n, k) \approx (n + k - 1) \cdot T_s^\text{pipe}$$
- The bottleneck condition verifies if stage $i$ service time is higher than the
  task's **inter-arrival** time ($T_a$)
  $$T_s^i (f_i) > T_a^i$$

In general, if a stage is a bottleneck must be parallelized, otherwise its input
buffer grows indefinitely (if unbounded).

### Optimal Number of Stages

Let $T_a$ be the inter-arrival time of stream items to a stage computing the
function $F$. We want to increase the throughput of the stage by using a
pipeline of $k$ stages each compuging $f_i$ such that

$$T_s^\text{seq} (F) = \sum_{i=1}^k T_s^i (f_i)$$

To avoid the pipeline to be a bottleneck, in the ideal case of balanced stages,
it must hold

$$\frac{T_s^\text{seq} (F)}{k} \leq T_a$$

additionally we want to minimize the task latency

$$L_\text{pipe} = \sum_{i=1}^k T_s^i (f_i) + (k - 1) \cdot T_\text{comm}$$

The minimal $k$ that satisfies both equations is

$$k_\text{opt} = \left\lceil \frac{T_s^\text{seq} (F)}{T_a} \right\rceil$$

with $k_\text{opt}$ being the optimal number of stages.

## References

- [[structured_parallel_programming]]
- [[pipeline_farm]]
