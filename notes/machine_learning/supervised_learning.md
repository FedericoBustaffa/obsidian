---
id: supervised_learning
aliases: []
tags:
  - master
  - machine_learning
---

# Supervised Learning

In the field of Machine Learning, **supervised methods** involve the model
training by examples. In other words there is an initial phase where we feed the
model with examples that basically are couples, containing a problem and its
solution.

$$<\text{input, output}> = (x, y)$$

This is typically called, **training set**. There are also other useful sets,
for example **validation** and **test** set, that are used to measure the model
performance and make some adjustments if necessary.

The goal is to make the model adapt to the examples, in order to extract the
general pattern so that it will be able to find solutions for unseen problems.

The two main problems that supervised learning models are called to solve are

- **Regression**: the output value is a real number.
- **Classification**: the output value is a categorical value (a class).

Let's also clarify that $x$ is a vector of **features**, that can have different
meanings. For example we can have _numerical_ features like weight and height,
but we can also deal with _categorical_ features, like the eyes color.

Categorical can indeed be useful and be fundamental in the recognition of
patterns, but indeed we cannot put "_blue eyes_" in a function. It's necessary
to encode it in numbers, but now we have the problem to decide an order. Maybe
there is an order, but maybe they all have the same "_weight_".

> [!note] $1$-of-$k$ Method
> A popular equally weight categorical features is by encoding it in a vector of
> $k$ values, where $k$ is number of possible categories the feature can have,
> building a sort of boolean table.

## Model Complexity

The **model complexity** can be seen as an abstract measure on how flexible it
is, but with flexibility come some issues.

For example, a model that is to simple could never catch the underline behavior
of data, resulting in an **underfitting** situation.

> [!NOTE] Underfitting
> The model has bad results on training data.

On the other hand, a very complex (and so very flexible) model can have great
results on training data, but poor results on new problems. This is due to the
fact that all the complexity was used to perfectly fit the example data,
resulting in a situation of **overfitting**.

> [!NOTE] Overfitting
> The model has very good results on training data but bad results on new data.

Both situations evidence the inability of a model to _generalize_.

### Number of Examples

The number of examples also plays a role in the building of a good model and, in
general, the more data (examples) you have, the better the model will learn.

For example, a fairly complex model feeded with few data, can overfit because it
tries to use all its complexity to better solve the few problems it has.
Instead when we give a lot of data, the model has to try a balance to get the
best result for each problem without losing accuracy.

## Index

- [[linear_models]]
- [[knn]]
- [[perceptron]]
- [[neural_networks]]
- [[support_vector_machine]]
- [[decision_tree]]
- [[ensemble]]
- [[random_forest]]

## References

- [[machine_learning]]
