---
id: autoencoder
aliases: []
tags:
  - master
  - machine_learning
---

# Autoencoder

An **autoencoder** is a network that tries to reproduce its input. That's way
is called _semi-supervised_ (or _self-supervised_) learning; there is still an
_error_ function to optimize but there is no real target.

![Autoencoder|500](/files/autoencoder.png)

Compressing the input and then decompressing it has the effect of catch
underlying structure in it (**undercomplete** autoencoder). It's also possible
to have an autoencoder that maps the input in a higher dimensional space
(**overcomplete** autoencoder); this can be useful for example in classification
to make the problem linearly sperable in that new space.

## References

- [[unsupervised_learning]]
