---
id: reduce
aliases: []
tags:
  - master
  - parallel_computing
---

# Reduce

A **reduce** is a pattern used to aggregate or combine results from multiple
parallel computations into a single output. It operates on an input collection
and produces a single element in output.

Let $C$ be a collection and $\oplus$ be the **aggregate operator**, the result
of a reduce computation is

$$x = \text{reduce} (C, \oplus)$$

the aggregate operator is at least **associative** but can also be
**commutative**, giving more freedom to the runtime to reorder operands freely.

The parallelism may be exploited by arranging the computation as a balanced
tree. The leaves combine pairs of inputs, then the internal nodes apply $\oplus$
to the result of their subtrees, all the way up to the root.

## Implementation

A possible implementation of the reduce pattern is done by partitioning the
input collection among all $k$ workers. Each worker computes the reduce on its
local partition and sends the result to its neighbor.

In the first iteration worker $i$ sends data to worker $(i + 1) \% k$ and
receives data from worker $(i - 1) \% k$. For other iterations worker $i$ sends
the received data to worker $(i+1) \% k$. After $k-1$ iterations, all the
workers have the reduced value locally.

If $\oplus$ is both associative and commutative, a **tree-based reduce** can be
better, but this time only one worker will have the reduced value locally.

A possible variant of the tree-based implementation is the so called
**all-reduce**, where in the end all workers have the reduced value. This
implementation involves the **recursive doubling algorithm**, computed in
$\log_2 k$ iterations.

## Cost Model

Given a collection of $n$ elements, $k$ workers and the $\oplus$ reduce
operator, the completion time is

$$
T_c^\text{reduce} (n, k) = T_\text{scatter} + T_\text{local} +
T_{*\text{-reduce}}
$$

where

$$
\begin{align*}
T_\text{scatter} &= T_{\text{split} (n,k)} +
k \cdot T_\text{comm} \left( \frac{n}{k} \right) \\
T_\text{local} &= \frac{n}{k} \cdot T_\oplus \\
T_\text{tree-reduce} &= \log_2(k) \cdot (T_\oplus + T_\text{comm} (1)) \\
T_\text{all-reduce} &= \log_2(k) \cdot (T_\oplus + 2 \cdot T_\text{comm} (1)) \\
\end{align*}
$$

To find the optimal number $k$ of workers we can compute the derivative of the
function $T_c^\text{reduce} (n, k)$ and find its minimum.

## Reference

Links: [[structured_parallel_programming]], [[map_reduce]]
