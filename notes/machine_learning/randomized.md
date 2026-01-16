---
id: randomized
aliases: []
tags:
  - master
  - machine_learning
---

# Randomized Neural Networks

A modern constructive approach for neural networks is the so called
**randomized** approach, that adds a degree of randomness. In particular
**randomized neural networks** can achieve good results with relatively small
computational effort.

A randomized neural network is a normal network that leaves hidden layer weights
frozen after initialization; only the output layer is trained, mitigating the
common problems of training a deep neural network. Advantages of this approach
are that the training is not deep and so the model doesn't suffer all gradient
issues of a DNN and it also reduces the overall computation time.

For a classification problem we can think for example at a single hidden layer
that is wider than the input. This is like mapping the input in an higher
dimensional space, where could be linearly separable.

This is the same idea of SVMs kernel trick, except that SVMs kernels have to be
_valid_, here we are exposing **random relationships** between data. It seems
strange that such a thing even work but there is theorem that ensures it.

> [!IMPORTANT] Cover Theorem
> A complex patter-classification problem, cast in a high-dimensional space
> **non-linearly**, is more likely to be **linearly separable** than in a
> low-dimensional space, provided that the space is not densely populated.

## References

- [[neural_networks]]
- [[deep_learning]]
