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
$x$ and a point $\hat{x}$ on the hyper-plane that minimizes the distance.

So we can rethink $x$ as a sort of translation of $\hat{x}$ in the direction
defined by $w$ (that is orthogonal to the hyper-plane) by the distance $r$ we
are looking for:

$$x = \hat{x} + r \frac{w}{\| w \|}$$

Let's also remember that $g(x) = w^\top x + b$ and so we can write

$$
\begin{align*}
g(x) &= w^\top \left( \hat{x} + r \frac{w}{\|w\|} \right) + b \\
&= w^\top \hat{x} + w^\top r \frac{w}{\|w\|} + b \\
&= r \frac{\| w \|^2}{\| w \|} = r \| w \|
\end{align*}
$$

thus, the distance can be computed as

$$r = \frac{g(x)}{\| w \|} = \frac{w^\top x + b}{\| w \|}$$

So, assuming an already optimal hyper-plane, the distance from the hyper-plane
and a support vector is $x^{(s)}$

$$\frac{w^\top x^{(s)} + b}{\| w \|} = \frac{1}{\| w \|} = \frac{\rho}{2}$$

Computing also the distance from the hyper-plane to a negative support vector,
that will be $-\frac{1}{\|w\|}$, the total margin will be

$$\rho = \left| -\frac{1}{\|w\|} - \frac{1}{\|w\|} \right| = \frac{2}{\|w\|}$$

The **optimal** hyper-plane maximizes $\rho$ by minimizing $\|w\|$.

### Hard Margin

The first and simplest form of SVM is the **hard margin** that wants to minimize
the weights while having zero classification errors. Of course this is possible
if and only if the problem is linearly separable.

In its **primal form** the problem is formulated as the minimization of

$$\Psi (w) = \frac{1}{2} w^\top w = \frac{1}{2} \| w \|^2$$

satisfying the constraint that

$$y_p \cdot (w^\top x_p + b) \geq 1$$

for each pattern in the training set. But the objective function is quadratic
and convex in $w$ and the constraints are linear in $w$; solving this problem
scales with the size of the input space.

So typically, when working with SVMs, we are intrested in solving the **dual
form**, that in this case is defined with the **Lagriangian multipliers**:

$$
J(w, b, \alpha) =
\frac{1}{2} w^\top w - \sum_{p=1}^N \alpha_p (y_p (w^\top x_p + b) - 1)
$$

with $\alpha_p \geq 0$ for every $p = 1, \dots, N$ that are the lagrangian
multipliers.

Each term of the sum corresponds to a constraint of the primal problem. The aim
is minimize $J$ with respect to $w$ and $b$ and maximize it with respect to
$\alpha$.

We will find that the optimal hyper-plane is expressed as

$$\sum_{p=1}^N \alpha_p y_p x_p^\top x + b = 0$$

and from Kuhn-Tucker conditions it follows that

- If $\alpha_p > 0$, then the $y_p (w^\top x_p + b) = 1$ and $x_p$ is a support
  vector.
- If $x_p$ is not a support vector then $\alpha_p = 0$.

Hence we can restrict the computation to the support vectors, finding the
optimal weights as follows

$$w = \sum_{p=1}^{N_s} \alpha_p y_p x_p$$

with $N_s$ the number of support vectors. Because the hyper-plane depends only
on support vectors.

## References

- [[supervised_learning]]
- [[statistical_learning_theory]]
