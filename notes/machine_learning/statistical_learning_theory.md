---
id: statistical_learning_theory
aliases: []
tags:
  - master
  - machine_learning
---

# Statistical Learning Theory

The **statistical learning theory** (**SLT**) provides the basics for model
complexity and risk analysis. It formalizes what a machine learning model does
in the learning phase with respect to the training set, its complexity and the
true distribution of data.

## Inductive Bias

When a model learn search through an **hypothesis space** $H$ the best
hypothesis $h$ to fit the training data. The hypothesis space is defined by the
complexity of the model: the more the model is complex and flexible, the more
the hypothesis space is big.

> [!NOTE] Consistent Hypothesis
> An hypothesis $h$ is **consistent** with the training set if, for each example
> $\langle x, y \rangle$ in the training set it holds
> $$h(x) = y$$

In order to define and search into $H$ we need a **biased learner** (our model)
that must have an **inductive bias**, that provides in a sense an heuristic to
shrink the space of possible models to something that can be searched more
efficiently. The two main types of inductive biases are

- **Language bias**: that put constraints on the model. Just choosing a family
  of models can be a language bias, but for example in neural networks is not the
  case (universal approximation). Limiting the model flexibility is another
  language bias, for example by searching in the space of linear model regressor,
  only the linear regressors.
- **Search bias**: put constraints on the learning algorithm. For example the
  regularization leads to models with lower weigths.

Typically we are interested in the second because the first basically defines
$H$.

## True and Empirical Risk

If we would have access to the true function that models our data as a black-box
it should be possible to sample as many points as we want without noise even
without knowing the function. So it could be possible to build a perfect dataset
(possibly infinite) on which testing our model. The mean error of the model on
that dataset get the name of **true risk** $R$, and that's what machine learning
aims to minimize. The actual definition of $R$ is the mean risk on the true data
distribution:

$$R = \int L(y, h(x)) \; d P(x, y)$$

The problem is that we of course don't have the function neither in a black-box
form, neither its actual formula. The only thing we have are data coming from
measurements, that can (and most likely will be) noisy.

The error our model does on this dataset is called **empirical risk**
$R_\text{emp}$ that can be used to evaluate the model, knowing that it is an
estimation of $R$ based only on the observed data.

So we can shift the problem into the fitting of this empirical data, trying to
capture the underlying behavior that is the true function. The aim is still to
minimize $R$ and, more important is not to minimize $R_\text{emp}$, because
minimizing it means fitting also the noise (over-fitting).

What misses is a nice and reliable relation between $R$ and $R_\text{emp}$
because, we can't say anything about them. They could be the same because the
sampling is very accurate (no noise) or because the level of precision is low
enough. Or they can be different:

- If $R > R_\text{emp}$ it means that the model is in over-fitting and so it
  will not be able to generalize well.
- If $R < R_\text{emp}$ it means that the model generalizes well but of course
  perform worse on noisy data.

There is the need to introduce a new term in order to set an inequality that
somehow defines a bound on $R$.

## VC-Dimension

The VC-dimension is a value that _evaluates_ $H$, providing a measure of its
complexity.

## References

- [[machine_learning]]
