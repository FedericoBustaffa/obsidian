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

## Classification

In the classical **binary classification problem** we want to find an
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

---

Typically, when working with SVMs, we are interested in solving the **dual
form**, that in this case is defined with the **lagrangian equation**.

$$
J(w, b, \alpha) = \frac{1}{2} \| w \|^2 -
\sum_{p=1}^N \alpha_p (y_p (w^\top x_p + b) - 1)
$$

with $\alpha_p \geq 0$ for every $p = 1, \dots, N$ that are the **lagrangian
multipliers**.

This gives us an optimization problem were we have to optimize only one function
in which the constraints are included and handled by the multipliers, in fact
the constraints were defined for each point as

$$y_p \cdot (w^\top x + b) \geq 1$$

So to achieve the dual form we need to impose

$$y_p \cdot (w^\top x + b) - 1 \geq 0$$

And so now each term of the sum corresponds to a constraint of the primal
problem.

The aim is minimize $J$ with respect to $w$ and $b$:

$$
\frac{\partial J}{\partial w} = 0 \quad
\frac{\partial J}{\partial b} = 0
$$

from which we obtain that

$$
w = \sum_{p=1}^N \alpha_p \cdot y_p \cdot x_p \quad
\sum_{p=1}^N \alpha_p \cdot y_p = 0
$$

So now we can substitute this two terms in $J$ and obtain

$$
J(\alpha) = \sum_p \alpha_p - \frac{1}{2} \sum_{p} \sum_{t} \alpha_p \alpha_t
\cdot y_p y_t \cdot x_p^\top x_t
$$

that we can maximize with a quadratic programming solver in order to find the
optimal $\alpha$ vector. Once we have it we can substitute the actual $\alpha$
values in the formulation for optimal $w$ previously defined, to find the
**optimal hyper-plane**:

$$\sum_{p=1}^N \alpha_p y_p (x_p^\top x) + b = 0$$

and also find the optimal $b$ corresponding to a positive support vector
$x^{(s)}$:

$$b = 1 - \sum_{p=1}^N \alpha_p y_p x_p^\top x^{(s)}$$

and from **Kuhn-Tucker conditions** it follows that

- If $\alpha_p > 0$, then the $y_p (w^\top x_p + b) = 1$ and $x_p$ is a support
  vector.
- If $x_p$ is not a support vector then $\alpha_p = 0$.

Note now that there is no need to know the optimal $w$ explicitly, because it's
defined in terms of lagrangian multipliers and so, given an input pattern $x$ we
can compute

$$g(x) = \sum_{p=1}^N \alpha_p y_p x_p^\top x + b$$

and classify $x$ as the sign of $g(x)$, but we can restrict the sum to the
support vectors $N_s$

$$g(x) = \sum_{p=1}^{N_s} \alpha_p y_p x_p^\top x + b$$

> [!IMPORTANT] Vapnik Theorem
> Let $D$ be diameter of the smallest ball around the data points $x_1, \dots,
> x_n$. For the class of separating hyper-planes described by the equation
> $$w^\top x + b = 0$$
> the upper bound to the VC-dimension is
>
> $$
> VC \leq \min \left( \left\lceil \frac{D^2}{\rho^2} \right\rceil,
> m \right) + 1
> $$
>
> where $m$ is the dimension of the input vector.

### Soft Margin

The _hard margin_ conditions can be to restrictive and for some cases the margin
can be very small or, in case the problem is not linearly separable, the solver
will not provide a solution.

The **soft margin** SVM relaxes the constraints and let at least one point to be
inside the margin (with or without classification error).

![SVM Soft Margin|350](/files/soft_margin.png)

So now we want to allow some points into the margin in order to have a larger
margin. This is done by the introduction of **slack variables**

$$\xi_p \geq 0 \quad \forall p = 1, \dots, N$$

So now also the definition of support vectors changes.

> [!NOTE] Support Vector (Soft Margin)
> A support vector in the soft margin SVM is a point $x_p$ that satisfies
> exactly the constraint
> $$y_p (w^\top x_p + b) = 1 - \xi_p$$

So now the Vapnik theorem does not hold anymore because is defined only for hard
margin SVMs.

Now the **primal form** of the problem is to find $w$ and $b$ that minimize

$$\Psi (w, \xi) = \frac12 w^\top w + C \sum_{p=1}^N \xi_p$$

under the constraints:

$$y_p (w^\top x_p) \geq 1 - \xi_p \quad \quad \xi_p \geq 0$$

where $C$ is a **regularization hyper-parameter** to be set in order to find a
trade off between empirical risk minimization and capacity term minimization.
Let's also note that

- Low $C$ allow many training errors, possibly leading to underfitting.
- High value of $C$ lead to a hard margin behavior.

Note also that $C = 0$ brings us back to hard margin formulation.

So now we can formulate the problem in its **dual form** as before but now we
have two kinds of constraints: the one on the classification and the one on the
non negativity of slack variables

$$
J(w, b, \xi, \alpha, \mu) = \frac{1}{2} \| w \|^2 + C \sum_{p=1}^N \xi_p -
\sum_{p=1}^N \alpha_p (y_p (w^\top x_p + b) - 1) - \sum_{p=1}^N \mu_p \xi_p
$$

with $\mu_p$ that are the lagrangian multipliers introduced to enforce the non
negativity of slack variables.

Now the **Kuhn-Tucker conditions** say to us that

- if $0 < \alpha_p < C$ and so $\xi_p = 0$ the points is a support vector.
- if $\alpha_p = C$ and so $\xi_p \geq 0$ the point is inside the margin.

The dual problem is solved as before but now we also want to optimize $\xi$.

### Kernel Trick

In general we can address non linearly separable problems with a _soft margin_
approach, but there are problems where even with that the SVM will not perform
good enough.

Usually, mapping the data points to an higher dimensional feature space, can
make them linearly separable in the new space.

![Higher Dimension Mapping|500](/files/svm_kernel.png)

So now we can find a function $\phi$ the maps features in an higher dimension
space. But know we have the problem is choosing the best $\phi$, unless a prior
knowledge on the feature space properties, in particular:

- How to choose the **class of $\phi$**: polynomial, gaussian, logarithmic etc.
- How to choose the **number of dimension to add**.

For example if we have

$$x = \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$$

we can add a third feature by applying a $\phi$ function and obtain

$$\phi(x) = \begin{bmatrix} x_1 \\ x_2 \\ x_1 \cdot x_2 \end{bmatrix}$$

So now the formula in due dual problem for the optimal hyper-plane becomes

$$\sum_{p=1}^N \alpha_p y_p \phi^\top (x_i) \phi(x) = 0$$

The problem here is that for arbitrarly complex $\phi$ and for high dimension
input space, the computation of $\phi$ and the inner products can become
unfeasible.

Fortunately, under certain conditions we do not need to evaluate directly
$\phi(x)$ and we don't even need to know the feature space itself. This is
possible by computing a function directly in the feature space

$$k : \mathbb{R}^m \times \mathbb{R}^m \to \mathbb{R}$$

called **inner product kernel** function

$$k(x_i, x) = \phi^\top (x_i) \cdot \phi(x)$$

that is symmetric: $k(x_i, x) = k(x, x_i)$. The dot product in feature space is
evaluated without considering the feature mapping and the feature space itself.

> [!EXAMPLE]
> Given a certain $\phi$ such that
> $$\phi(x) = \phi((x_1, x_2)^\top) = (x_1^2, \sqrt{2} x_1 x_2, x_2^2)^\top$$
> Given $x = (x_1, x_2)^\top$ and $y = (y_1, y_2)^\top$ in $\mathbb{R}^2$, we
> compute
> $$\phi^\top (x) \phi(y)$$
> in this way:
>
> $$
> \begin{align*}
> (x_1^2, \sqrt{2} x_1 x_2, x_2^2) (y_1^2, \sqrt{2} y_1 y_2, y_2^2)^\top &=
> x_1^2 y_1^2 + 2 x_1 x_2 y_1 y_2 + x_2^2 y_2^2 \\
> &= (x_1 y_1 + x_2 y_2)^2 \\
> &= ((x_1, x_2)(y_1, y_2)^\top)^2 \\
> &= (x^\top y)^2 = k(x, y)
> \end{align*}
> $$
>
> That is out kernel function.

It is possible to arrange the dot products in the feature space between the
images of the input training patterns in a $N \times N$ matrix, called **kernel
matrix**:

$$
K = \begin{bmatrix}
k(x_1, x_1) & \cdots & k(x_1, x_N) \\
\vdots & \ddots & \vdots \\
k(x_N, x_1) & \cdots & k(x_N, x_N) \\
\end{bmatrix}
$$

Which is symmetrical as the inner product kernel is symmetrical. However not
every function compute the kernel in the input space: this property only holds
for kernels gaining _positive semidefinite_ kernel matrices.

So now the **primal form** of the problem is defined as before

$$\Psi (w, \xi) = \frac12 w^\top w + C \sum_{p=1}^N \xi_p$$

but with the constraints defined as

$$y_p (w^\top \phi(x_p)) \geq 1 - \xi_p \quad \quad \xi_p \geq 0$$

and so the **dual form** can be expressed as

$$
J(\alpha) = \sum_p \alpha_p - \frac{1}{2} \sum_{p} \sum_{t} \alpha_p \alpha_t
\cdot y_p y_t \cdot k(x_p, x_t)
$$

satisfying the constraints

$$\sum_{p=1}^N \alpha_p y_p = 0$$

with $0 \leq \alpha_p \leq C$ for all $p$.

There are some popular valid kernels that are often used without the need to
build a kernel from scratch:

- **Polynomial**: given a degree of freedom $d$ is defined as
  $$k(x, x_p) = (x^\top x_p + 1)^d$$
- **Radial Basis Function**: also called **gaussian kernel** that, given
  $\sigma$ is defined as
  $$k(x, x_p) = e^{-\frac{1}{2 \sigma^2} \| x - x_i \|^2}$$
  and leads to a infinite dimensional space.
- **Sigmoidal**: that given $\beta_0 > 0$ and $\beta_1 < 0$ is defined as
  $$k(x, x_p) = \tanh (\beta_0 w^\top x_i + \beta_1)$$

For the first two and for every choice of their hyper-parameters, a valid kernel
is produced, while for the third only some values of $\beta_0$ and $\beta_1$
produce a valid kernel.

## Regression

SVMs can be used also for **regression** tasks but the now they work in the
opposite way of classification.

For a _hard margin_ version, the classification SVM does not allow any point
inside the margin; for regression instead the SVM wants to put every point
inside the margin, minimizing its size.

For a soft margin version instead, some points are allowed to stay outside of
the tube, leading to an higher resistence to outliers than other models like
neural networks.

---

More formally, the model again tries to estimate targets using a linear
expansion of (possibly) non linear functions

$$y \approx h(x) = w^\top \phi(x)$$

In order to perform regression a loss function is needed and in case of SVMs we
have $\epsilon$-insensitive loss:

$$
L_\epsilon (y, h(x)) = \begin{cases}
    | y - h(x) | - \epsilon & \text{if } | y - h(x) | < \epsilon \\
    0 & \text{otherwise}
\end{cases}
$$

where $\epsilon$ defines the tube's _width_.

![SVR|400](/files/svm_regression.jpg)

The problem formulation is very similar to the classification one: as before
also here there are **slack variables** $\xi_i$ and $\xi_i'$ that hold the
following property:

$$-\xi_i' - \epsilon \leq y_i - w^\top \phi(x_i) \leq \epsilon + \xi_i$$

for all $i$ from $1$ to $N$. This leads to the following constraints

$$
\begin{align*}
y_i - w^\top \phi(x_i) & \leq \epsilon + \xi_i \\
w^\top \phi(x_i) - y_i & \leq \epsilon + \xi_i' \\
\xi_i & \geq 0 \\
\xi_i' & \geq 0 \\
\end{align*}
$$

The formulation for the **primal problem** is similar to the classification task
formulation. Given the training set, find the optimal values of $w$ such that
the following objective function is minimized

$$\psi (w, \xi, \xi') = \frac12 w^\top w + C \sum_{i=1}^N (\xi + \xi')$$

under the constraints defined before. The formulation for the dual problem again
needs the lagrangian multipliers. Given the training set, find the optimal
values of $\{ \alpha_i \}_{i=1}^N$ and $\{ \alpha_i' \}_{i=1}^N$ which maximize
the objective function

$$
Q(\alpha, \alpha') = \sum_{i=1}^N y_i (\alpha_i - \alpha_i') - \epsilon
\sum_{i=1}^N (\alpha_i + \alpha_i') - \frac12 \sum_{(i,j)=1}^N (\alpha_i -
\alpha_i') (\alpha_j - \alpha_j') k (x_i, x_j)
$$

under the constraints

$$
\begin{gather*}
\sum_{i=1}^N (\alpha_i - \alpha_i') = 0 \\
0 \leq \alpha_i \leq C
0 \leq \alpha_i' \leq C
\end{gather*}
$$

for all $i$ from $1$ to $N$. Solving the dual problem we obtain the optimal
values of lagrangian multipliers, then we compute the optimal value of vector
$w$ like follows

$$
w = \sum_{i=1}^N (\alpha_i - \alpha_i') \phi(x_i) =
\sum_{i=1}^N \gamma_i \phi(x_i)
$$

and in this case support vectors correspond to non zero values of $\gamma_i$.
The estimated function using the linear expansion is defined as

$$
h(x) = \sum_{i=1}^N \gamma_i \phi^\top (x_i) \phi (x) =
\sum_{i=1}^N \gamma_i k(x_i, x)
$$

Like for classification, only support vectors matter and so as long as they are
in the tube, points that are not support vectors can be disposed in every
possible configuration.

## References

- [[supervised_learning]]
- [[statistical_learning_theory]]
- [[eigenvalues_eigenvectors]]
