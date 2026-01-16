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

![[pipeline.png|center]]

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

> [!EXAMPLE]
> Let's suppose we have to compute $F(x)$ and it takes 75 time units for each
> input $x$ so we indicate the completion time with
>
> $$T_s^\text{seq} = 75$$
>
> The function $F$ can be splitted in 3 sub-functions such that
>
> $$F(x) = f_3 (f_2 (f_1(x)))$$
>
> and each stage $f_i$ takes about 25 time units. Let's now feed the pipeline with
> $n$ tasks into the pipeline and, just for now, let's assume that the inter-stage
> communication cost is zero.
>
> The throughput of the sequential version can be computed like the number of
> tasks of the the time to compute them
>
> $$\text{throughput}_\text{seq} = \frac{n}{n \cdot 75}$$
>
> for the pipeline version the throughput can be computed the same way, considering
> the stages time and the fact the pipeline is not always at steady state.
>
> $$\text{throughput}_\text{pipe} = \frac{n}{(n + 2) \cdot 25}$$
>
> that for large $n$ values can be approximated with $1/25$, or one task every 25
> time units.
>
> Let's now consider a more precise example with communication time
> $T_\text{comm}=0$ and $n = 300$. The completion time for the two versions is
>
> $$
> \begin{gather*}
> T_c^\text{seq} = n \cdot T_s^\text{seq} = 22500 \\
> T_c^\text{pipe} = (2 + n) \cdot T_s^\text{stage} = 7550
> \end{gather*}
> $$
>
> with a final speedup of
>
> $$S(3) = \frac{T_c^\text{seq}}{T_c^\text{pipe}} \approx 2.98$$
>
> that is very close to ideal. If we now add some communication cost, for example
> $T_\text{comm} = 5$, the sequential completion time remains the same, while the
> parallel completion time becomes
>
> $$
> T_c^\text{pipe} = 2 \cdot (T_s^\text{stage} + T_\text{comm}) + (n-1) \cdot
> (T_s^\text{stage} + T_\text{comm}) + T_s^\text{stage} = 9055
> $$
>
> which leads to a speedup of
>
> $$S(3) = \frac{T_c^\text{seq}}{T_c^\text{pipe}} \approx 2.48$$
>
> > [!NOTE]
> > Latency is worse than the serial version because of the overhead introduced
> > with the communication between stages. From the standpoint of one individual
> > task there is no improvement but a worsening:
> >
> > - All the stages are executed one after the other.
> > - There is communication overhead between stages.

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

- The service time is
  $$T_s^\text{pipe} = \max_{i=1 \dots k}{(T_s^i (f_i))}$$
  where
  $$
  T_s^i (f_i) = \begin{cases}
      T_{f_i} + T_\text{comm} & \text{no communication overlap} \\
      \max{(T_{f_i}, T_\text{comm})} & \text{otherwise}
  \end{cases}
  $$
- The task latency is
  $$L_\text{pipe} = \sum_{i=1}^k T_s^i (f_i) + (k-1) \cdot T_\text{comm}$$
  with no communication overlap accounted.
- The completion time for $n$ tasks is
  $$T_c^\text{pipe} (n, k) \approx (n + k - 1) \cdot T_s^\text{pipe}$$
- The bottleneck condition verifies if stage $i$ service time is higher than the
  task's inter-arrival time ($T_a$)
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

## References

- [[structured_parallel_programming]]
- [[pipeline_farm]]
