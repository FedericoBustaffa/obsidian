---
id: matrices
aliases: []
tags:
  - master
  - linear_algebra
---

# Matrices

One of the main objects in linear algebra is the **matrix**, that can be seen as
an element of $\mathbb{R}^{m \times n}$, that can have many meanings as
collection of column or row vectors, the representation of a system of linear
equations or a linear mapping.

Before using matrices for some specific task is useful to define some
properties, always valid regardless of context.

- **Sum**: given $A, B \in \mathbb{R}^{m \times n}$
  $$A + B = C \in \mathbb{R}^{m \times n}$$
  that is the element-wise sum of $A$ and $B$ ($c_{ij} = a_{ij} + b_{ij}$).
- **Product**: given $A \in \mathbb{R}^{m \times n}$ and
  $B \in \mathbb{R}^{n \times p}$
  $$A \cdot B = C \in \mathbb{R}^{m \times p}$$
  is the so called **dot product** between $A$ and $B$. This works also for
  $1$-column (and $1$-row) matrices, defining the _matrix by vector_
  multiplication.

> [!NOTE] Matrix-Matrix Multiplication
> Given two matrices $A \in \mathbb{R}^{m \times n}$ and $B \in \mathbb{R}^{n
> \times p}$, the elements of product matrix $C \in \mathbb{R}^{m \times p}$ are
> computed as follows:
> $$c_{ij} = \sum_{k=1}^n a_{ik} \cdot b_{kj}$$
> with $i = 1, \dots, m$ and $j = 1, \dots, p$.

For the dot product the following properties are valid:

- **Associativity**: $(AB)C = A(BC)$
- **Distributivity**: $(A + B) C = AC + BC$ and $A(B + C) = AB + AC$

> [!IMPORTANT]
> The _dot product_ is, in general, not commutative
> $$A B \neq B A$$

Matrices also represent **linear transformations**, for example $Av$ is a
transformation $A$ applied to the vector $v$ (like translation or rotation).

> [!note] Order of Transformations
> Knowing that the order in matrix multiplication counts, if we apply multiple
> transformations to a vector, the order in which we apply them will change the
> outcome.
>
> If for example we apply a rotation before a traslation, first we change the
> coordinates system by rotating and then the vector will be moved along the
> new axis.
>
> On the other hand, if we translate first, the vector will be moved and then
> rotated accordingly to the same center of rotation as before.

To combine multiple transformation is possible to multiply matrices. In order to
do that we need the product between the two to be well defined. Given
$A \in \mathbb{R}^{m \times n}$, the other matrix needs to be
$B \in \mathbb{R}^{n \times p}$ for the product to be well defined. In the end
we have

$$AB \in \mathbb{R}^{m \times p}$$

The classical row by column product has a cost of $O(mnp)$ that in general we
can approximate to $O(n^3)$ if we consider almost squared matrices with
dimension $n \times n$.

An important matrix, that is useful to define other matrices and represents the
neutral element for the dot product, is the **identity** matrix

> [!NOTE] Identity Matrix
> The **identity matrix** is defined as a square matrix with all zeros except on
> the main diagonal, that has only ones.
>
> $$
> I = \begin{bmatrix}
> 1 & 0 & \cdots & 0 \\
> 0 & \ddots & \ddots & \vdots \\
> \vdots & \ddots & \ddots & 0 \\
> 0 & \cdots & 0 & 1
> \end{bmatrix}
> $$

The identity matrix is crucial to define the **inverse** matrix of a matrix $A$,
that is very important in many situations like solving linear systems.

> [!NOTE] Inverse Matrix
> Given a _square_ matrix $A$, its inverse is a matrix $B$ such that
> $$AB = BA = I$$
> and it is denoted as $A^{-1}$.

Unfortunately, not all (square) matrices have a inverse, if so, they are called
**singular** or **non invertible**; if instead a matrix has a inverse is called
**regular**, **non singular** or **invertible**.

A direct consequence of this definition is that, if we have $A$ and $I$ is
possible to find $A^{-1}$ by computing the dot product

$$A A^{-1} = I$$

and solving a linear system. Now should be more clear that not always is
possible to have a inverse.

Another special matrix is the **transpose** that, unlike the inverse, is always
defined for every matrix square and rectangular matrix (so also for vectors).

> [!NOTE] Transpose Matrix
> Given a matrix $A \in \mathbb{R}^{m \times n}$, the matrix $B \in
> \mathbb{R}^{n \times m}$ whose elements are
> $$b_{ij} = a_{ji}$$
> is called the transpose of $A$ and is denoted as $A^\top$.

Let's see some properties of these two matrices

- $A A^{-1} = A^{-1} A = I$
- $(A^\top)^\top = A$
- $(AB)^{-1} = B^{-1} A^{-1}$ and similarly $(AB)^\top = B^\top A^\top$
- $(A + B)^{-1} \neq A^{-1} + B^{-1}$ instead $(A + B)^\top = A^\top + B^\top$

Let's also say that if $A$ is invertible, also $A^\top$ is invertible and this
is true because

$$(A^{-1})^\top = (A^\top)^{-1} = A^{-\top}$$

> [!NOTE] Symmetric Matrix
> Given a matrix $A$ such that
> $$A = A^\top$$
> then is called **symmetric**.

The sum of symmetric matrices is also symmetric, while the product is not
(generally).

There are also properties for matrix multiplication by a scalar, so given
$\lambda, \psi \in \mathbb{R}$ it holds:

- $(\lambda \psi) C = \lambda (\psi C)$
- $\lambda (BC) = (\lambda B) C = B (\lambda C) = (BC) \lambda$
- $(\lambda C)^\top = \lambda C^\top$
- $(\lambda + \psi) C = \lambda C + \psi C$
- $\lambda (B + C) = \lambda B + \lambda C$

These are general properties and useful matrices to start doing things with
linear algebra.

## References

- [[linear_algebra]]
- [[linear_systems]]
