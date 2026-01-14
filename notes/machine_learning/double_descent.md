---
id: double_descent
aliases: []
tags:
  - master
  - machine_learning
---

# Double Descent

A relatively new phenomenon discovered is called **double descent** and it
occurs when the model is **overparametrized**, in other words when the model has
more weights/units than the number of training examples.

This phenomenon seems to go in contrast with what the basics of ML teaches:
usually a very complex and flexible model (w.r.t to the task) is bound to
overfitting.

![Double Descent|500](/files/double_descent.png)

This is not a thing strictly related to neural networks and can be observed also
for simple linear models for a regression task.

## References

- [[deep_learning]]
