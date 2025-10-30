---
id: support_vector_machine
aliases: []
tags:
  - master
  - machine_learning
---

# Support Vector Machine

The **support vector machine** (**SVM**) is a model whose development is driven
by the concept of **structural risk minimization** from the statistical learning
theory.

It can be used for classification and regression and it can adapt well to both
linear and non linear task; in particular it tries to find separation
hyper-planes also for classification problems that are not linearly separable.

## Binary Classification

In the classifical **binary classification problem** we want to find an
hyper-plane of equation

$$w^\top x + b = 0$$

that separates the examples in this way

$$
\begin{align*}
w^\top x + b &\geq 0 & \text{for } y = +1\\
w^\top x + b &< 0 & \text{for } y = -1
\end{align*}
$$

Of course the equation of the hyper-plane is just a _discrimant_ function, while
the actual hypothesis function is the sign of the discriminant.

What an SVM does is try to find a **margin** $\rho$ that tries to maximize the
distance between the linear hyper-plane and the closest data points.

![SVM Margin|300](/files/svm_margin.png)

Of course we can move the hyper-plane and the margin will change as well; anyway
the SVM aims to find the hyper-plane that maximizes the margin $\rho$ in order
to have a larger confidence on unseen points.

## References

- [[supervised_learning]]
- [[statistical_learning_theory]]
