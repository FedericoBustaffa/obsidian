---
id: matrix_rank
aliases: []
tags:
  - master
  - linear_algebra
---

# Matrix Rank

The matrix **rank** is a useful value that provides some information about the
matrix itself, that can be also useful check linear systems has solutions and
could be useful for many other purposes.

> [!NOTE] Rank
> The number of linearly independent columns of matrix
> $A \in \mathbb{R}^{m \times n}$ is called the (column) **rank** of $A$ and
> it's denoted with $\text{rank} (A)$.

The rank, as we were saying before has a lot of interesting properties:

- $\text{rank} (A) = \text{rank} \left( A^\top \right)$, that means that the
  column rank is equal to the row rank.
- The columns of $A$ _span_ a subspace $U \subseteq \mathbb{R}^m$ with
  dimension $\dim(U) = \text{rank}(A)$. And of course the same is also true for
  the row rank, except that the subspace is $U \subseteq \mathbb{R}^n$.
- A square matrix $A \in \mathbb{R}^{n \times n}$ is invertible if and only if
  $\text{rank} (A) = n$.
- A linear system $Ax = b$ can be solved if and only if
  $\text{rank} (A) = \text{rank}(A | b)$.
- Given $A \in \mathbb{R}^{m \times n}$, the subspace of the system $Ax = 0$ has
  dimension $n - \text{rank}(A)$.

Another important property for matrices is the _full rank_

> [!NOTE] Full Rank
> A matrix $A \in \mathbb{R}^{m \times n}$ has **full rank** if its rank is
> equal to the largest possible rank for a matrix of the same size:
> $$\text{rank} (A) = \min{(m, n)}$$

In general we can compute the rank of a matrix $A$ by reducing $A$ in
_Row-Echelon_ form and count the number of linearly independent columns.

A slight different definition is about a full column rank matrix.

> [!NOTE] Full Column Rank
> A matrix $A \in \mathbb{R}^{m \times n}$ has **full column rank** if its rank
> is equal to the number of her columns.

It's clear that if the matrix is rectangular and wide ($m < n$) the matrix
cannot be a _full column rank_, because the maximum rank is the minimum between
the number of rows and columns.

Another definition states that a matrix $A \in \mathbb{R}^{m \times n}$ is a
full column rank if $\ker{(A)} = \{ 0 \}$. In other words there is no vectore
$v \neq 0$ such that $A v = 0$.

> [!NOTE] Theorem
> A matrix $A \in \mathbb{R}^{m \times n}$ has full column rank if and only if
> $A^\top A$ is SPD.

## References

- [[linear_algebra]]
- [[linear_independence]]
- [[linear_systems]]
- [[eigenvalues_eigenvectors]]
