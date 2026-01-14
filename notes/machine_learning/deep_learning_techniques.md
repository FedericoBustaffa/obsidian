---
id: deep_learning_techniques
aliases: []
tags:
  - master
  - machine_learning
---

# Deep Learning Techniques

In order to train a deep neural networks standard appraches and techniques are
not enough. In the past training was done by pre-training but nowadays there are
better way to train a model end-to-end for a specific task.

The main issues with deep neural networks are related to the gradient, that can
become too small or too big depending on weights magnitude. Multiplication
through many layers can cause these issues causing the failure of training.

## Exploding Gradient

When the model backpropagate the error through many layers, when the weights are
big, the gradient magnitude grows exponentially, causing very unstable training
and overflows.

### Gradient Clipping

During the optimization, the model could encounter cliffs in the cost function,
causing a big variation in the gradient and causing big jumps in the
optimization process.

![Loss Function Cliff|600](/files/gradient_clipping.png)

In order to avoid this and mantain a smooth stable training, is possible to
**clip** the gradient if its norm exceeds a threshold $v$, so if $\| g \| > v$,
where $g$ is the gradient vector, the new gradient becomes

$$g = v \cdot \frac{g}{\| g \|}$$

Take into account that the clipping can be done layer by layer during
backpropagation or _globally_ in a single step:

- **Layer wise**: during backpropagation each layer computes its gradient
  vector and clip it if necessary. Can be finer for each layer but can modify
  proportions between layers.
- **Global**: each layer computes its gradient vector. The clipping is based on
  the norm of the global gradient vector of each layer. This is more stable and
  preserve relative gradient directions through layers.

The standard for most libraries is the global approach.

## Gradient Vanish

If weights are small can happen that the gradient for first layers is too small
and the update is almost zero.

## References

- [[deep_learning]]
- [[neural_networks]]
- [[neural_networks_training]]
