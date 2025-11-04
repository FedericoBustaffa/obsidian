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
measure how much our model is sensible to data.

## References

- [[machine_learning]]
- [[statistical_learning_theory]]
- [[validation]]
