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
allows a network to automatically discover representations needed for a specific
task.

In general, in computer science is true that a task that involves information
processing of some kind can be very easy or very difficult, depending on
information representation.

A good representation often makes the task easier, but for some problems a
manual design of feature representation is very challenging. In ML words,
engineering the $\phi(x)$ function of LBE is often very difficult. Neural
networks, and in particular deep ones, can learn the $\phi$ function. This is
because for them $\phi$ is also a function of the weights: $\phi(x, w)$, and so
can be learned.

A deeper aspect is the fact that neural networks, automatically find **hidden**
representations or structure in information, that is way more difficult for a
human.

### Pre-Training

In order to obtain _hidden representations_, one historical case is
**pre-training**, that exploit the concept of **semi-supervised learning**.

To perform _pre-training_ is necessary to build an **autoencoder**, that at its
core, is a network that tries to reproduce its input. That's way is called
_semi-supervised_ (or _self-supervised_) learning; there is still an _error_
function to optimize but there is no real target.

![Autoencoder|500](/files/autoencoder.png)

Compressing the input and then decompressing it has the effect of catch
underlying structure in it (**undercomplete** autoencoder). It's also possible
to have an autoencoder that maps the input in a higher dimensional space
(**overcomplete** autoencoder); this can be useful for example in classification
to make the problem linearly sperable in that new space.

One of the first methods to train a deep neural network without backpropagation
is proposed by the **Bengio algorithm**, that performs a so called **greedy
layer-wise unsupervised pre-training** by training a multi-layer network one
layer at a time.

1. Start with a single layer network that tries to reproduce its input.
2. Remove the output layer and add a new hidden layer. This hidden layer is
   trained to reproduce the output of the previous layer.
3. Repeat until the desired number of layers is reached. At each step weights of
   already trained layers are freezed; only one layer at a time is trained.
4. In the end add a _task layer_ for supervised task, that will be trained on
   actual targets.

In the end the effect is that hidden layers contains hierarchical
representations of the world, but not a specialized way for the task, like for a
modern MLP, trained end-to-end with backpropagation.

This might not work as well as a network fully specialized for a specific task,
but has some interesting implications.

First of all, all layers but the last, contains good representations and so good
starting weights for a generic supervised task. It also works as a
regularization technique because it smooths the noise, and reduce probability
of overfitting.

On the other hand a too extreme compression may end up flattening crucial
differences in input data; while a mapping to higher dimensionality can cause
loss of relationships.

### Transfer Learning

Once hidden representations are available, with **transfer learning** is
possible to use them for supervised tasks like classification and regression.

This is done under the assumption that some features discovered in the
pre-training phase are useful for different tasks.

So now is also possible to use a pre-trained model for different tasks without
the need to retrain it entirely; the only thing needed to be retrained is the
output layer to make predictions.

Is also possible to change the input domain under the assumption that some
learned features between different domains are shared.

## Distributed Representation

Deep learning also brings to the table the concept of **distributed
representation** of data, that tries to improve to the **symbolic** one.

There are fields, like in NLP, where data has no mathematical meaning (or at
least it's not clear to define o recognize it); in fact words cannot be easily
mapped in a vectorial space because their meaning, context and semantics are
something too abstract.

Intuitively we can say that "cat" is more similar to "dog" than "train", but how
to map this to math is not trivial at all. The simplest way is for example by
doing a one-hot encoding of every word in a vocabulary, but this results in a
vector space with $N$ dimension where $N$ is the number of words in the
vocabulary. Let's also notice that every word is distant $\sqrt{2}$ from every
other, so is pratically impossible to catch any relation between couples of
words.

But here is the deep learning trick that let a model learn a meaningful
representation automatically. Starting from a _one-hot_ encoded dataset, is
possible to train an autoencoder with text so that it learns how much words are
related just by looking at things like occurences in similar sentences.

In this way is possible to go from a one-hot representation to a vectorial
representation with each entry being a continuous value. We can imagine that the
net learns the concept of animal in some sense and so now "cat" and "dog" are
closer in the new space with respect to "train".

This new representation also **quantify** how much of a feature is present in a
certain word

## References

- [[neural_networks]]
- [[neural_network_training]]
