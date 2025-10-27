---
id: activation_functions
aliases: []
tags:
  - master
  - machine_learning
---

# Activation Functions

Until now we used a **step** activation function that is the sign of weighted
sum of weights and features, but there are many types of activation function
that can lead to differences in training. The three main types of activation
functions are

1. **Linear**: not a real activation function because it produces a real value
   that can't be used to make classifications.
2. **Threshold**: like the step function used in the perceptron.
3. **Non linear**: generally a non linear _squashing_ value function.

This last type has the property to be smoothed differentiable threshold
function.

## Sigmoids

In the **sigmoidal** type of functions there are two that are often used as
activation functions: _logistic_ and _hyperbolic tangent_.

The **logistic** function is defined by the following formula

$$f_\sigma (x) = \frac{1}{1 + e^{-ax}}$$

where $a$ indicates the slope of the function (the higher it is the steeper the
curve will be). It is defined for all $x \in \mathbb{R}$ and produces real
values in the range $[0, 1]$.

The **hyperbolic tangent** is quite similar but produces real values in the
range $[-1, +1]$ and can be defined as

$$f_\text{symm} = 2 f_\sigma (x) - 1 = \tanh \left(\frac{ax}{2} \right)$$

where $a$ has the same meaning of the logistic function. Note also the $a = 0$
lead to a linear function, while $a \to \infty$ lead to a step function.

For the logistic function generally an output value greater or equal to $0.5$
(the threshold) correspond to the positive class, while a lower value correspond
to the negative class.

The difference from the LTU is that is possible to change this threshold and
even consider a _rejection zone_ around the threshold in order to avoid fragile
decisions.

For the $\tanh$ the threshold is in $0$ but is possible to do the same as the
logistic.

## Other Activation Functions

Just to list them, there are many other activation functions that behave in many
different ways and used for different purposes, here some of them:

- **Radial Basis**: used in _RBF_ networks; one example is the gaussian
  $$f(x) = e^{-ax^2}$$
- **Softmax**: used for multiple output classifications.
- **Stochastic neurons**: output will be $+1$ with probability $P(net)$ or $-1$
  with probability $1 - P(net)$.
- **Tanh-like**: used for efficient computation.
- **Rectifier**: one of the most popular in Deep-Learning is the **ReLU**
  (rectified linear unit)
  $$f(x) = \max (0, x)$$
- **Softplus**: a smooth approximation of ReLU:
  $$f(x) = \ln (1 + e^x)$$

Most of them perform comparably but of course there are some that became popular
for different reasons and based on the field of applications.

## Differentiability

An interesting aspect for activation functions emerges by seeing at their
**derivatives**:

- **Linear**: a constant.
- **Threshold**: not defined, in fact is not used in LMS.
- **Sigmoids**: for $a = 1$ we have
  $$
  \begin{align*}
  \frac{d f_\sigma(x)}{dx} &= f_\sigma (x) (1 - f_\sigma (x)) \\
  \frac{d f_{\tanh} (x)}{dx} &= 1 - f_{\tanh} (x)^2
  \end{align*}
  $$

The fact that sigmoids are **differentiable** lead to the possibility for a
gradient based algorithm.

## References

- [[perceptron]]
- [[neural_networks]]