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

$$g(x) = w^\top x + b = 0$$

that separates the examples in this way

$$
\begin{align*}
w^\top x + b &\geq 0 & \text{for } y = +1\\
w^\top x + b &< 0 & \text{for } y = -1
\end{align*}
$$

The equation of the hyper-plane $g(x)$ is just a _discrimant_ function, while
the actual hypothesis function is the sign of the discriminant.

What an SVM does is try to find a **margin** $\rho$ that tries to maximize the
distance between the linear hyper-plane and the closest data points.

![SVM Margin|300](/files/svm_margin.png)

Of course we can move the hyper-plane and the margin will change as well; anyway
the SVM aims to find the hyper-plane that maximizes the margin $\rho$ in order
to have a larger confidence on unseen points.

In order to maximize the margin we want to rescale $w$ and $b$ so that the
closest points to the hyper-plane satisfy

$$|g(x_p)| = |w^\top x_p + b| = 1$$

and so we can write

$$
\begin{align*}
w^\top x_p + b &\geq 0 & \text{for } y_p = +1\\
w^\top x_p + b &\leq 0 & \text{for } y_p = -1
\end{align*}
$$

that in a compact form becomes

$$y_p \cdot (w^\top x_p + b) \geq 1$$

> [!NOTE] Support Vector
> A **support vector** $x^{(s)}$ satisfies the previous equation exactly:
> $$y^{(s)} \cdot (w^\top x^{(s)} + b) = 1$$

The support vectors are the closest points to the hyper-plane. So it seems
obvious that we need to compute the distance between a point and the
hyper-plane. To do that, the idea is to compute the distance between our point
$x$ and a point $x_p$ on the hyper-plane that minimizes the distance.

So we can rethink $x$ as a sort of translation of $x_p$ in the direction defined
by $w$ (that is orthogonal to the hyper-plane) by the distance $r$ we are
looking for:

$$x = x_p + r \frac{w}{\| w \|}$$

## References

- [[supervised_learning]]
- [[statistical_learning_theory]]
