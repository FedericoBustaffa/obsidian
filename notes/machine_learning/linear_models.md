---
id: linear_models
aliases: []
tags:
  - master
  - machine_learning
---

# Linear Models

**Linear models** are in a sense the baseline for many other machine learning
models, cause it introduces many recurring aspects and most important is quite simple.

Generally speaking is better suited for regression but can be used for
classification as well, but for this specific task there are other models with
different approaches able to provide better results.

In both cases the model have a _inductive bias_ that is a _guess_ on which the
objective function would be. In other words, by observing data or by observing
model results, we need to choose an **hypothesis function** in advance.

Once chosen an hypothesis function, the model will try to fit the data with that
function. In theory, the more complex is the function, the better the model will
fit the data.

## Regression

Let's start from the **regression** task, where the model, basically receives
$l$ patterns and tries to minimize the error between the hypothesis function and
the input patterns.

### Univariate Linear Regression

The simplest case of linear regression is **univariate**, in other words we have
a training set composed by $l$ patterns that are couples of real numbers

$$(x, y)$$

The simplest function we can think of is of course a straight line, that will be
out hypothesis function

$$h_w(x) = w_0 + w_1 x$$

where $w$ is the vector of coefficients of the function to approximate, usually
called **weights**.

We are moving in an infinite hypothesis space, because we have to find the best
$w$ to fit the data, but all elements of $w$ are real numbers.

To move towards the best solution by minimizing the **loss function**, and the
most popular is the **mean square error** (**MSE**).

$$
\text{Loss} (h_\mathbf{w}) = E(\mathbf{w}) =
\frac{1}{l} \sum_{p=1}^l (y_p - h_w(x_p))^2
$$

that in the case of a linear regression is

$$\frac{1}{l} \sum_{p=1}^l (y_p - (w_1 x_p + w_0))^2$$

where $x_p$ is the $p$-th input (or pattern), $y_p$ is the $p$-th output for
$x_p$ and $l$ is the number of examples.

> [!HINT] Why the mean?
> Minimize the **total** error or the **mean** gives the exact same line, but
> usually the mean error is useful for two reasons:
>
> - **Comparison**: it's simpler to compare with the error on a different
>   dataset or different model (or both) whose data are in the "same domain".
>   For example if we consider a dataset of $100$ patterns and another one of
>   $100.000$ patterns, the total error in the second case will be higher but
>   not because of a bad fitting.
> - **Scaling**: as anticipated before, the mean brings the total
>   error back to the problem scale. This is useful for comparison but also
>   because is more understandable from a human perspective. The total error is
>   not directly _usable_ to have an idea on the model performance.

So we need to solve a **least (mean) square** (**LMS**) problem, that is usually
the standard to solve overdetermined systems. To do that we basically need to
compute the **gradient** of the loss function, in order to find its minimum

$$\frac{\partial E(\mathbf{w})}{\partial w_i} = 0$$

For the simple linear regression we have only two free parameters and so we can
find the $\mathbf{w}$ such that

$$
\begin{align*}
\frac{\partial E(\mathbf{w})}{\partial w_0} &=
-2 \cdot (y - (w_1 x + w_0))  &= 0 \\
\frac{\partial E(\mathbf{w})}{\partial w_1} &=
-2 \cdot (y - (w_1 x + w_0)) \cdot x &= 0
\end{align*}
$$

Of course we can then perform a sum over all patterns, obtaining

$$
\begin{align*}
\frac{\partial E(\mathbf{w})}{\partial w_0} &=
-2 \sum_{p=1}^l (y_p - (w_1 x_p + w_0)) \\
\frac{\partial E(w)}{\partial w_1} &=
-2 \sum_{p=1}^l (y_p - (w_1 x_p + w_0)) \cdot x_p
\end{align*}
$$

because the derivative of a sum is the sum of derivatives.

### Multivariate Linear Regression

For the **multivariate** case there isn't much more to say except that we need a
more general notation, in order to be able work with $n$-dimensional inputs.

So from now on $x$ is a vector $\mathbf{x}$ and we want to multiply each
feature for one of the weights in order to build the hypothesis function

$$h_\mathbf{w} (\mathbf{x}) = w_0 + w_1 x_1 + w_2 x_2 + \cdots + w_n x_n$$

as we can see the only weight without a feature is $w_0$, but if we add a $1$ to
every $x$ vector in order to pair it with $w_0$, we can now use the more compact
form

$$
h_\mathbf{w} (\mathbf{x}) = \mathbf{w}^\top \mathbf{x} =
\mathbf{x}^\top \mathbf{w} =
\langle \mathbf{w}, \mathbf{x} \rangle =
\sum_{i=1}^n w_i x_i
$$

with $w^\top = \begin{bmatrix} w_0, w_1, \dots, w_n \end{bmatrix}$ and
$x^\top = \begin{bmatrix} 1, x_1, \dots, x_n \end{bmatrix}$. And of course the
the result given from out model for a given input $x_p$ is

$$
h_\mathbf{w} (\mathbf{x}_p) = \mathbf{w}^\top \mathbf{x}_p =
\sum_{i=1}^n w_i x_{p,i}
$$

What remains is to compute a general gradient with respect to a generic $w_i$:

$$
\frac{\partial E(\mathbf{w})}{\partial w_i} =
-2 \cdot (y - h_w(x)) \cdot x_i =
-2 \cdot \left(y - \sum_{i=1}^n w_i x_i \right) \cdot x_i
$$

because for every $j \neq i$ the partial derivative considers every term
different from $w_i x_i$ a constant.

## Classification

Linear model can also be used for classification tasks but we this time the
label is something numerical; for a binary classification problem we can use
$-1/+1$.

The problem is still solved by computing a regression line, but this time the
output can only be $-1/+1$ (or any other encoding of the two labels) and so we
will obtain a line that cut the $x$ axes in a certain point.

Of course if we give an $x$ value to this line it will return a real value and
so we simply take the sign (in this specific case), in order to obtain the
label.

So in this case our _loss_ function (the one we want to minimize) remains the
same, while the hypothesis function become something like

$$h(x) = \text{sign} (\mathbf{w}^\top \mathbf{x}) \quad \text{output: -1/+1}$$

And the point where our line changes sign define the so called **linear
threshold unit** (**LTU**), that in general is an hyperplane orthogonal to the
**decision hyperplane**.

![Linear model binary classification](/files/linear_model_binary_classification_with_boundary.png)

This model can make mistakes because it doesn't aim to find the best way to
separate the different labeled data; from the model point of view it tries to
fit data, and so if there is a correlation between $\mathbf{x}_p$ and the
corresponding $y_p$, data with same label should be in some way _clustered_. In
this sense the model describes _gradually_ how the classification changes.

In this case is also possible to compute the error as the number of
misclassified patterns, but this is not a differentiable function, so it can't
be used to train the model (only after training to measure performance).

## Learning Algorithms

In order to solve this two tasks we need a way to minimize the error on the
training set, and this can be done in two main ways:

- **Direct**: analytical method that consists in computing the gradient,
  that, as a derivative for a univariate function, is zero where error function
  has a maxima. Setting the negative gradient to zero and solving the system
  gives us instead a minima.
- **Numerical**: iterative method the uses the negative gradient in order to
  gradually move towards a minimum.

In practice the numerical method is often used because is numerically more
stable, instead the direct method can propagate a big approximation error,
resulting in bad fitting, even though the math is correct.

### Direct Method

The direct method differentiates $E(\mathbf{w})$, obtaining

$$\frac{\partial E(w)}{\partial w_j} -2 \sum_{p=1}^l (y_p - x_p^\top w) x_{p,j}$$

and the try to solve the _normal equations_ given by the system

$$(X^\top X) w = X^\top y$$

but if the matrix $X^\top X$ is not singular the unique solution is given by

$$w = (X^\top X)^{-1} X^\top y = x^+ y$$

where $X^+$ is the pseudoinverse if $X$, that can be computed using the singular
value decomposition

$$X = U \Sigma V^\top \implies X^+ = V \Sigma^+ U^\top$$

in this way we can directly apply the SVD to compute

$$w = X^+ y$$

obtaining the minimal norm solution of least squares problem, that in this case
is

$$\min \frac{1}{2} \| X w - y \|^2$$

This is the so called _closed form_ and of course this steps form a learning
algorithm, but to solve the system the are many efficient numerical methods that
can be used.

### Numerical Method

The most common way to solve the least (mean) squares problem is _iterative_,
and the algorithm used is named **gradient descent algorithm**. This is usually
preferred to the direct method because

- Is more efficient (the direct method has cubic complexity with the matrix $X$
  dimension).
- Is better for approximation with noisy data by stopping before the minimum.
- Numerically is more stable.

The key idea is to compute the gradient of the _loss_ function with respect to
the weights. The gradient is basically a vector pointing in the direction of
steepest slope. So basically, a negative gradient shows the direction where the
function decreases. At each step the weights are updated by this formula

$$w_\text{new} = w + \eta \cdot \Delta w$$

where $\eta$ is the **learning rate** and $\Delta w$ is the negative gradient.

This algorithm let us move towards the minimum of our _loss_ function by _small_
steps, proportional to how far we are from the minimum and the learning rate.

## Linear Basis Expansion

Of course the model as it is right now is very limited both for regression and
classification. In the first case, the distribution of data can follow more
complex functions; in the second case all we can do with a line is try to solve
**linearly separable** problems.

In order to add some complexity we can use a technique called **linear basis
expansion** (**LBE**), that now let us work with _non linear_ functions. The
model is still linear with respect to the weights, but now we accept that input
and output data can have some non linear relationship.

So what we do is basically is augment the input vector with additional variables
which are transformations of $x$ according to some function

$$\phi_k : \mathbb{R}^n \to \mathbb{R}$$

So, in general we have that the new hypthesis function becomes

$$h_w(x) = \sum_{k=0}^K w_k \phi_k (x)$$

Where $K$ can be larger than the number $n$ of dimensions of a single input
vector.

Since the model is still linear in the parameters $w$ (not in the input $x$), we
can use the same algorithm as before.

The main advantage in using this technique is more flexibility of the model,
that of course can become also the disadvantage, because an overcomplicated
model risk overfitting. Other two cons are _curse of dimensionality_ and the
fact that $\phi$ is fixed, and not automatically deduce based in data.

### Tikhonov Regularization

In order to control the complexity of the model, a technique called **Tikhonov
regularization** can be used. The application of this method in the case of
regression lead to the so called **Ridge regression**, where the model is
limited by the values of $w$.

In general high value of $w$ are penalized, in order to make some $w_j$ tend to
zero, so that less terms are used.

$$\text{Loss} (w) = \sum_{p=1}^l (y_p - x_p^T w)^2 + \lambda \| w \|^2$$

> [!TIP] Why the square of $\| w \|$?
> Let's notice that the $\| w \|$ term is already positive, so one can think
> that there is no need to square it. In reality this is done because, otherwise
> it is **not differentiable** and so it cannot be used in the gradient descent
> algorithm.

And so the new formula for updating weights now becomes

$$w_\text{new} = w + \eta \cdot \Delta w - 2 \lambda w$$

As we can see we extended the _loss_ function with a new term in which $\lambda$
is a new _small_ positive hyperparameter chosen in model selection phase.

Of course we need to find a trade off between these two terms because

- with a **small** $\lambda$ the focus is on obtaining a small error, but with a
  too complex model the risk is overfitting ($\lambda = 0$) is previous
  _unregolarized_ model).
- with a **high** $\lambda$ the second term can become dominant, increasing
  error on data and moving towards simpler models with the of risk underfit.

The penalty term penalizes high values of the weights and tends to drive all the
weights to smaller values. Some weight can also become $0$, reducing the model
complexity.

#### Direct Approach

As the previous case is possible to solve the problem with a direct approach
given the following closed formula

$$w = (X^T X + \lambda I)^{-1} X^T y$$

where the $(X^T X + \lambda I)$ matrix **is always invertible**.

## References

- Machine Learning
  - [[supervised_learning]]
  - [[gradient_descent]]

- Computational Mathematics
  - [[least_squares]]
  - [[singular_value_decomposition]]
