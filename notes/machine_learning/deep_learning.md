---
id: deep_learning
aliases: []
tags:
  - master
  - machine_learning
---

# Deep Learning

It's possible to start talking about **deep learning** for neural network with
more than three layers. Implications of _deep learning_ are mainly

- **Hierarchical feature learning**: each layer produce the input for the next
  one, that sees the output of the previous as features. This relaxes the
  usually needed preprocessing that developers need to do.
- **Distributed representation learning**: the network can learn patterns that
  can be used like _building blocks_ in order to
  - **Generate** new data obtained by composition.
  - Learn a _concept_ **by exclusion**: the network can learn something that is
    in some sense _complementary_ to what is present in the training set.
  - Learn a **continuous representation** of a concept (particularly useful for
    categorical data).

The need for **deep learning** is justified by the fact that, yes we have the
**universale approximation theorem**, but it does not provide informations on
the number of units.

For many modern problems, a single layer network would require an exponential
number of units to achieve good performance. In general, a deep network can
achieve same performance as a flat one, but with much less units.

> [!NOTE] No-Flatten Theorem
> There is also a _pseudo-theorem_, called **no-flatten** that want to define
> better where a flat network requires (in practice) a unfeasible number of
> units.

So the problem of a single layer network can be either the expressity power or
the efficiency issue of having too many units.

## Inductive Bias

Of course this kind of architecture brings a **inductive bias** assuming that
the function the network is trying to learn is a _composition_ of functions (or
at least that there is a nested factor of variation).

If the task match this bias is generally a good idea to have a _deep_
architecture. This is typical with image recognition, where usually the first
layer learns only very specific segments and, going deeper, it starts to build
much more complex structures that in the end put together to recognize
handwritten digits, animals and so on.

Notice also that this bias is quite general, so is no big deal if the function
to learn does not have any compositional hidden structure.

## Curse of Dimensionality

Many learners rely on **local approximation** by assuming that, for two
different points in a very small region of space, the true function output
should be very similar for both.

The problem is that they lack in generalization of the surroundings for which
they don't have examples to train on. This problem is also amplified in highly
dimensional spaces, where points are more _sparse_ and the uncertainty is way
higher between them.

So make assumptions is needed to have a non local generalization in an
exponential number of regions with respect to the number of examples. The
stronger the assumption, the less is the uncertainty. Of course the assumption
could be completely wrong, leading to bad results.

Deep learning approaches instead rely on a compositional and hierarchical
inductive bias, allowing the model to represent complex functions through
reusable and distributed features.

The hierarchical way of learning, potentially increases the number of regions
that can be distinguished by an exponential factor. In other words the model has
a quite general inductive bias, but can also generalize well in regions of space
far from examples that share underlying structure.

## Representation Learning

**Representation learning** is a concept related to the fact that deep learning
allows

## References

- [[neural_networks]]
- [[neural_network_training]]
