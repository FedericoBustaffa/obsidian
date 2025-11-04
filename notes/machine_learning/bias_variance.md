---
id: bias_variance
aliases: []
tags:
  - master
  - machine_learning
---

# Bias Variance

When we train a model on a given dataset, even though the model is able to
generalize well, its final free parameters will be dependent on the training
set. In other words, if we train the model on a different dataset, its free
parameters will be different.

In this sense a specific training set is one possible realization from the
universe of data.

Usually the aim is to train the model in a way that it will not be sensible to
change of dataset (or at least not much), while keeping a good generalization
capability. Of course there will be always a difference in a model trained on
different datasets.

Depending on the training error, the test error and the difference between
models trained on different datasets it is possible (at least in theory) to
measure how much our model is sensible to data. This also let us understand if
the model error is due to overfitting, underfitting or noise in the data.

In order to understand what is the cause it's necessary to compute the so called
**bias-variance decomposition** of expected error at a point $x$:

- **Bias**: quantify the discrepancy between the true function and $h$
- **Variance**: quantify the variability of the response of model $h$ for
  different realizations of the training data.
- **Noise**: irriducible error due to random factors.

In general we have an high bias when our model is too simple and cannot
approximate accurately enough the real function. We have instead an high
variance if the model trained on different datasets always find much different
$h$; and so $h_1(x)$ will be much different from $h_2(x)$.

## Regression

Let's for example take a linear model involved in a regression task and suppose
to have a training set composed by examples $\langle x, y \rangle$ where the
true function is

$$y = f(x) + \epsilon$$

where $\epsilon$ is a Gaussian noise with zero mean and standard deviation
$\sigma$ and the error is the classic MSE.

Let's now suppose that every training set is built from the same probability
distribution $P$ and so we want to train the same model on _all_ the possible
realizations of the training set. Theorically, this can give us the **expected
prediction error** on a given point $x$.

$$E_P [(y - h(x))^2]$$

that is basically the mean of all the errors produced by all the different
trained models. Now we can decompose this expectation in _bias_, _variance_ and
_noise_ in order to measure them.

## References

- [[machine_learning]]
- [[statistical_learning_theory]]
- [[validation]]
