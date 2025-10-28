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
somehow defines a bound on $R$ because the ideal case is the one in which

$$R \leq R_\text{emp}$$

for noisy dataset.

## VC-Dimension

The VC-dimension is a value that _evaluates_ $H$, providing a measure of its
**complexity**. The VC-dimension can be thinked, by restricting on the
classification case, as the number of points that $H$ can correctly discriminate
for every possible labeling.

In order to better formalize this concept lets introduce the concept of
**shattering**. Let be

- $X$: input space.
- $N$: the size of the input space.
- $H$: the hypothesis space.
- A binary classification problem.

We can define a **dichotomy** as a particular partition or labeling of the $N$
points. So the same set of points can has $2^N$ possible dichotomies.

A particular dichotomy is **represented** in $H$ if there exists an hypothesis
$h \in H$ that realizes the dichotomy.

> [!NOTE] Shattering
> The hypothesis space $H$ **shatters** $X$ if and only if $H$ can represent all
> the possible dichotomies on $X$.

In other words $H$ _shatters_ $X$ if and only if, for every labeling of the
points there exists an hypothesis $h \in H$ that can label them correctly.

> [!IMPORTANT] VC-Dimension
> The **VC-dimension** of a class of functions $H$ is the maximum cardinality of
> a set of points in $X$ that can be shattered by $H$.

In practice to see if $H$ has VC-dimension at least $k$ we need to find at least
one disposition of $k$ points, for which each dichotomy is represented in $H$.
Doesn't matter if there exists one disposition in which a dichotomy is not
represented. In this case we can say that

$$VC(H) \geq k$$

On the other hand, when for every possible arrangement of $k$ points, there
exists always at least one dichotomy that is not represented in $H$. And so

$$VC(H) < k$$

In general the VC-dimension of a class of linear separators (LTU) in a
$n$-dimensional space is $n+1$, in fact, for points in $\mathbb{R}^2$ the
VC-dimension is $3$.

## References

- [[machine_learning]]
