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

## References

- [[neural_networks]]
- [[neural_network_training]]
