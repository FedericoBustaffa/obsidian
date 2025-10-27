---
id: singular_value_decomposition
aliases: []
tags:
  - master
  - linear_algebra
  - computational_mathematics
---

# Singular Value Decomposition

The **singular value decomposition** (**SVD**) is a way to the decompose every
matrix $A \in \mathbb{R}^{m \times n}$ in a product of three matrices

$$A = U \Sigma V^\top$$

where $U \in \mathbb{R}^{m \times m}$ and $V \in \mathbb{R}^{n \times n}$
orthogonal and $\Sigma \in \mathbb{R}^{m \times n}$ diagonal, where, on the
diagonal we have the so called **singular values**:

$$\{ \sigma_1, \dots, \sigma_{\min(m,n)} \}$$

The singular values have the following interesting property

$$\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_{\min{(m, n)}} \geq 0$$

Singular values are unique, while $U$ and $V$, in general, are not; so in other
words, the singular values depend only on the matrix $A$, while there are
multiple choices of $U$ and $V$ that keep the decomposition valid.

As said, $\{ \sigma_1, \dots, \sigma_{\min{(m,n)}} \}$ are the singular values
of $A$, while the columns of $U$ are called the **left singular vectors** of $A$
and in a similar way, the rows of $V^\top$ (or the columns of $V$) are called
the **right singular vectors** of $A$.

## SVD and Eigenvalues

By taking a look at the SVD formula and, most importantly at the $\Sigma$
matrix, it seems that the SVD is a generalization of the eigen decomposition for
square matrices.

We can in fact take a matrix $A \in \mathbb{R}^{m \times n} = U \Sigma V^\top$
and see that

$$
\begin{align*}
A^\top A &= \left(U \Sigma V^\top\right)^\top \left(U \Sigma V^\top\right) & \\
        &= V \Sigma^\top U^\top U \Sigma V^\top & U^\top U = I \\
        &= V \Sigma^\top \Sigma V^\top = V \Sigma^2 V^\top & \Sigma = \Sigma^\top
\end{align*}
$$

That is the **spectral decomposition** of $A^\top A$ from which we can deduce
that $\sigma_1, \dots, \sigma_n$ are the singular values of $A$ if and only if
$\sigma_1^2, \dots, \sigma_n^2$ are the eigenvalues values of $A^\top A$.

---

If we take a look at the rank of $A$, we can see that it is equal to the number
of strictly positive singular values. In fact is possible to write $A$ as the
sum of $r = \text{rank} (A)$ terms of rank-$1$.

$$A = \sigma_1 u_1 v_1^\top + \cdots + \sigma_r u_r v_r^\top$$

where $u_i$ is a column of $U$ and $v_i^\top$ is a row of $V^\top$. In other
words, to write $A$ we only need the first $r = \text{rank} (A)$ singular
values, left and right singular vectors.

This leads to a more compact representation of $A$ and the SVD in general that
takes the name of **thin** SVD of a rectangular _tall_ matrix
$A \in \mathbb{R}^{m \times n}$ ($m > n$). In this form the columns of $U$ are
orthonormal.

The SVD gives also some information on the $4$ fundamental subspaces of $A$

- $\text{Im}(A) = \text{span} \{ u_1, \dots, u_r \}$
- $\text{Ker}(A) = \text{span} \{ v_{r+1}, \dots, v_n \}$
- $\text{Im}(A^\top) = \text{span} \{ v_1, \dots, v_r \}$
- $\text{Ker}(A^\top) = \text{span} \{ u_{r+1}, \dots, u_m \}$

In fact we can for example test it like this

$$A = \sigma_1 u_1 v_1^\top + \dots + \sigma_r u_r v_r^\top$$

and so

$$
A v_{r+1} = \sigma_1 u_1 v_1^\top v_{r+1} + \dots +
\sigma_r u_r v_r^\top v_{r+1} = 0
$$

because every $v_i^\top \cdot v_{r+1} = 0$.

## References

- [[linear_algebra]]
- [[eigenvalues_eigenvectors]]
- [[computational_mathematics]]
- [[least_squares]]
