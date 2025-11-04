---
id: ensemble
aliases: []
tags:
  - master
  - machine_learning
---

# Ensemble

Training models on different training sets, generally leads to obtain different
models. It is possible to take advantage of this fact by using them together in
a so called **ensemble**, that can be exploited in many different ways.

## Bagging

The simplest ensemble is the **bagging**, where every model basically produce an
output and the final output will be:

- The **average** in case of _regression_.
- The **majority vote** in case of _classification_. Or the sign of the mean of
  the continous outputs.

This is generally convenient because high variance models can perform well on
average. Or in other terms, the average of $h$ reduce the variance.

## Boosting

The **boosting** consists in differentiate each trainings focusing on errors by
training multiple classifiers one after the other. In each iteration the new
classifier is trained weighting more the examples misclassified before.

The final output will be a combination of weighted votes, where classifiers with
a lower error rate weight more.

This technique improve a lot capabilities of so called _weak learners_ that,
eventually could classify correctly every pattern.

## References

- [[machine_learning]]
- [[bias_variance]]
