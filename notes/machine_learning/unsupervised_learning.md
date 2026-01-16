---
id: unsupervised_learning
aliases: []
tags:
  - master
  - machine_learning
---

# Unsupervised Learning

The **unsupervised learning** paradigm is quite different from the _supervised_
one; in this approach examples are **not labeled**, so there is no _error_ in
the model output.

It's not even possible to properly talk about _prediction error_ because the aim
of unsupervised learning is not to predict but is to catch underlying structures
in the input data. Usually we are talking about **clusering** and
**dimensionality reduction** tasks (**representation learning**).

## Clustering

A typical task for unsupervised learning is **clustering** where the model tries
to partition data into **clusters**, that are subsets of similar data.

Usually the number of cluster must be known before the algorithm runs, or it
must be a grid search hyperparameter, assuming there is a good way to determine
if the algorithm performed good clustering.

## Index

- [[autoencoder]]
- [[k-means]]
- [[self_organizing_maps]]

## References

- [[machine_learning]]
- [[representation_learning]]
