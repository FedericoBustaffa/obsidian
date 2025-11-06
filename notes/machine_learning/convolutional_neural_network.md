---
id: convolutional_neural_network
aliases: []
tags:
  - master
  - machine_learning
---

# Convolutional Neural Network

The name **convolutional neural network** takes inspiration from the
**convolution** operation that, conceptually, is a weighted average of a
function $f$, weighted by another function $g$ moved over time.

$$
(f * g)(x) = \int_{-\infty}^\infty f(\tau) \cdot g(x - \tau) \; d\tau
 = \int_{-\infty}^\infty f(x - \tau) \cdot g(\tau) \; d\tau
$$

We can imagine $g$ limited in an interval (window) and we let it _slide_ over
$f$; the convolution on a point $x$ is

## References

- [[neural_networks]]
