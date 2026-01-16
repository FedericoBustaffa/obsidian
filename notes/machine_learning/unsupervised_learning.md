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

The problem with unsupervised learning is that, in absence of a target, is not
possible to determine if the algorithm did well (at least not easily). Things
become more complicated when also the number of clusters is not known a priori.

Of course if we know the label of each pattern and the model partition every
sample without knowing it, is possible to say if the cluster is good. Otherwise
we have to check metrics like how far each point is from the center of the
cluster (**centroid**). This approach has a big limitation because if we have as
many centroids as points, the final error will be zero, but the clusters are the
points themselves so there is no information gain.

## Index

- [[autoencoder]]
- [[k-means]]
- [[self_organizing_maps]]

## References

- [[machine_learning]]
- [[representation_learning]]
