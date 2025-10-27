---
id: laws
aliases: []
tags:
  - master
  - parallel_computing
---

# Laws

To model and predict and to define bounds to parallel programs we can use some
_laws_ that can be very useful to know potentialities and limits.

First we have to establish sequential runtime as a baseline, computing the
completion time $T(1, n)$, then we are interested in the three basic metrics
discussed before:

- _Speedup_: how much faster can we get with $p > 1$ processing elements?
- _Efficiency_: is out parallel program efficient?
- _Scalability_: how does our parallel program behaves with fixed problem size
  (_strong scalability_) and with variable problem size (_weak scalability_).

Once we define those elements we can start to apply some _law_ and formulas to
predict the trend of the performance of our algorithm.

## Main Laws

- [[amdahl]]
- [[gustafson]]

## Comparison

| Law       | Scaling         | Generality                           |
| --------- | --------------- | ------------------------------------ |
| Amdahl    | Strong          | Strict                               |
| Gustafson | Strong and Weak | More general (contains Amdahl's law) |

## References

Links: [[parallel_computing]], [[metrics]]
