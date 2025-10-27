---
id: eigenvalues_eigenvectors
aliases: []
tags: []
---

# Eigenvalues and Eigenvectors

Given a matrix $A \in \mathbb{R}^{n \times n}$, a scalar $\lambda$ and a vector
$v$ such that

$$A v = \lambda v$$

we call $\lambda$ and $v$ respectively an **eigenvalue** and an **eigenvector**
of $A$. To compute eigenvalues and eigenvectors we need to **diagonalize** the
matrix $A$, but it has to be **diagonalizable** in the first place.

> [!NOTE] Diagonalizable Matrix
> A matrix $A \in \mathbb{R}^{n \times n}$ is **diagonalizable** if exists a
> matrix $V \in \mathbb{R}^{n \times n}$ that is invertible and a **diagonal**
> matrix $D$, such that
>
> $$
> A = V D V^{-1} = \begin{bmatrix}
> & & \\ \mathbf{v}_1 & \cdots & \mathbf{v}_n \\ & &
> \end{bmatrix} \begin{bmatrix}
> \lambda_1 & & \\ & \ddots & \\ & & \lambda_n
> \end{bmatrix} \begin{bmatrix}
> & \mathbf{w}_1^\top & \\ & \vdots & \\ & \mathbf{w}_n^\top &
> \end{bmatrix}
> $$
>
> This is also called **eigendecomposition**.

where $(\lambda_i, v_i)$ are **eigenpairs** of $A$, formed by the eigenvalue
$\lambda_i$ and the eigenvector $v_i$.

The set $\{ v_1, \dots, v_n \}$ is a basis of $\mathbb{R}^n$ made of
eigenvectors of $A$.

Some interesting property of eigenvectors are

- If $v$ is an eigenvector of $A$, then also $\alpha v$ is an eigenvector of
  $A$
  $$A (\alpha v) = \alpha (Av) = \alpha \lambda v$$
- If $v$ and $w$ are eigenvectors of $A$, then $v + w$ is also an eigenvector of
  $A$
  $$A (v + w) = Av + Aw = \lambda v + \lambda w = \lambda (v + w)$$

---

Let's now consider a matrix $A = V D V^{-1}$ and its eigenvalues
$\{ \lambda_1, \dots, \lambda_n \}$, the eigenvalues of $A^k$ can be easily
computed by notice that

$$A^k = \prod_{i=1}^k A = \prod_{i=1}^k V D V^{-1}$$

but if we expand the product like this

$$A^k = V D V^{-1} V D V^{-1} \cdots V D V^{-1} V D V^{-1}$$

we can notice that there are a lot of $V^{-1} V$ products that can simplified,
resulting in

$$
A^k = V D^k V^{-1} = V
\begin{bmatrix} \lambda_1^k & & \\ & \ddots & \\ & & \lambda_n^k \end{bmatrix}
V^{-1}
$$

from which we can deduce that the eigenvalues of $A$ are
$\{ \lambda_1^k, \dots, \lambda_n^k \}$ while the eigenvectors are the same,
because the matrix $V$ is still the same, so we can take their columns as
eigenvectors.

A direct implication of this is with polynomials. Let's consider a classic
polynomial of degree $k$

$$p(x) = c_0 + c_1 x + c_2 x^2 + \cdots + c_k x^k$$

In a similar way is possible to write a matrix polynomial like this

$$p(A) = c_0 I + c_1 A + c_2 A^2 + \cdots + c_k A^k$$

Like before, we can expand every $A^i$ in $V D^i V^{-1}$, for all $k$

$$
p(A) = c_0 I + c_1 V D V^{-1} + c_2 V D^2 V^{-1} + \cdots + c_k V D^k V^{-1}
$$

if we now factor out $V$ on the left and $V^{-1}$ on the right, we get

$$p(A) = V (c_0 I + c_1 D + c_2 D^2 + \cdots + c_k D^k) V^{-1}$$

concluding that

$$p(A) = V p(D) V^{-1}$$

and so the eigenvalues of $p(A)$ are

$$
p(D) = \begin{bmatrix} p(\lambda_1) & & \\ & \ddots & \\ & & p(\lambda_n)
\end{bmatrix}
$$

## Spectral Theorem

One of the most important theorems for diagonalization is the **spectral
theorem**.

> [!NOTE] Spectral Theorem
> If $A \in \mathbb{R}^{n \times n}$ and is symmetric, then exists a matrix
> $U \in \mathbb{R}^{n \times n}$ that is orthogonal and there exists a diagonal
> matrix $D \in \mathbb{R}^{n \times n}$ with the eigenvalues of $A$ on its
> diagonal such that
> $$A = U D U^T$$
> and this is called **spectral decomposition** of $A$.

This theorem implies a lot of interesting properties like the fact that if a
matrix is symmetric then is _always diagonalizable_.

Another interesting fact is that the matrix $U$ containing the eigenvectors is
orthogonal and so the diagonalization is simplified due to the fact that we
don't need to compute its inverse but its transpose.

Two interesting properties, derived by the fact that $U$ is orthogonal are that

- All the eigenvalues are real.
- We have $n$ eigenvectors that are an orthonormal basis of $\mathbb{R}^n$.

> [!NOTE] Theorem
> If $A \in \mathbb{R}^{n \times n}$ is symmetric, then
>
> $$
> \lambda_\text{min} \| x \|^2 \leq x^\top A x \leq
> \lambda_\text{max} \| x \|^2
> $$
>
> for any $x$. Of course the formula can be changed in
>
> $$
> \lambda_\text{min} \leq \frac{x^\top A x}{\| x \|^2} \leq
> \lambda_\text{max}
> $$

Let's notice that when $\| x \| = 1$ the formula is simplified in

$$\lambda_\text{min} \leq x^\top A x \leq \lambda_\text{max}$$

The result given by this theorem is a consequence of the fact that if $v \neq 0$
such that $Av = \lambda_\text{max} v$, then

$$
v^\top A v = v^\top (\lambda_\text{max} v) = \lambda_\text{max} v^\top v =
\lambda_\text{max} \| v \|^2
$$

the same holds for $\lambda_\text{min}$.

## Symmetric Positive (Semi)Definite Matrices

Some interesting matrices are the **symmetric positive definite** matrices
(**SPD**), that are defined as symmetric matrices whose eigenvalue are all
_strictly_ positive and are denoted as

$$A \succ 0$$

It's easy to imagine that **symmetric positive semidefinite** matrices
(**SPSD**) are symmetric matrices with all eigenvalues greater or equal than
$0$ and are denoted as

$$A \succeq 0$$

Let's also notice that

- $A$ is SPD if and only if $x^\top A x > 0$ for all $x \neq 0$.
- $A$ is SPSD if and only if $x^\top A x \geq 0$ for all $x$.

For rectangular matrices we have that, given $A \in \mathbb{R}^{m \times n}$ the
matrices $A^\top A$ and $A A^\top$ are SPSD. They're both symmetric:

$$
\begin{align*}
A^\top A &= (A^\top A)^\top = A^\top A \\
A A^\top &= (AA^\top)^\top = AA^\top
\end{align*}
$$

and their eigenvalues are greater or equal than $0$

$$
\begin{align*}
x^\top A^\top A x &= (A x)^\top (A x) = \| A x \|^2 \geq 0 \\
x^\top A A^\top x &= (A^\top x)^\top (A^\top x) = \| A^\top x \|^2 \geq 0 \\
\end{align*}
$$

so they are both SPSD.

## References

Links: [[linear_algebra]]
