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

![Clustering|600](/files/clustering.png)

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

## Vector Quantization

The **vector quantization** technique aims to **encode** a data _manifold_ using
only a finite set of vectors $w = (w_1, \dots, w_K)$ called **references**.

So now a data vector $x$ from the original dataset is encoded and described by
the **best-matching** (or _winner_) reference vector $w_{i^*(x)}$ of $w$ for
which the **distortion error**, defined as some function $d(x, w_{i^*(x)})$ is
minimal.

This procedure has the effect of dividing the manifold in a set of subregions

$$V_i = \{ x \in V \mid \| x - w_i \| \leq \| x - w_j \| \}$$

for all $j$ and where $V$ is the original manifold, defining a **Voronoi
polyhedra**.

The goal in these scenarios is typically to find the best partitioning of
unknown distribution in input space into regions approximated by a _centroid_.
This corresponds to build a set of **vector quantizers** on which input
(continuous) space vectors are mapped.

### Quantization Error

The **quantization error** could be defined based on the task and type of data
but the most common one is the **squared error**:

$$d(x, w_{i^*(x)}) = \| x - w_{i^*(x)} \|^2$$

that can be seen as a **loss function**. Generally the _quantization error_ is
defined as the mean error over the distribution of data, computed between each
point and its centroid.

$$
E = \int f(d(x, w_{i^*(x)})) p(x) \; dx =
\int \| x - w_{i^*(x)} \|^2 p(x) \; dx
$$

where $p(x)$ is the probability distribution of input $x$. In a discrete version
is defined as

$$E = \sum_i^l \sum_j^K \| x_i - w_j \|^2 \cdot \delta(i,j)$$

where $\delta$ is the characteristic function defined as

$$
\delta(i, j) = \begin{cases}
1 & \text{if } w_j \text{ is the winner} \\
0 & \text{otherwise}
\end{cases}
$$

Of course is possible to minimize $E$ by choosing an optimal set of $w$, that is
the solution of vector quantization problem.

## References

- [[machine_learning]]
- [[representation_learning]]
- [[autoencoder]]
- [[k-means]]
- [[self_organizing_maps]]
