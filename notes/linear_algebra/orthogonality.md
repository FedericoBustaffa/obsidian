---
id: orthogonality
aliases: []
tags:
  - master
  - linear_algebra
---

# Orthogonality

The concept of **orthogonality** basically tells us if two vectors are
**orthogonal**, that in a $2$-dimensional space is equivalent to have two
perpendicular lines. Or in terms of linear algebra, it means that there is a
$90$ degree angle between the two vectors. For example

$$
\begin{bmatrix} 1 \\ 0 \end{bmatrix} \quad
\begin{bmatrix} 0 \\ 1 \end{bmatrix}
$$

are orthogonal vectors.

![[orthogonal_vectors.jpg]]

> [!NOTE] Vectors Orthogonality
> Two vectors $u, v$ are orthogonal if
> $$u^\top v = \langle u, v \rangle = 0$$
> their scalar product is zero.

If two vectors are orthogonal and have both length $1$ (euclidean norm), than
they are called **orthonormal**.

![[orthogonal_orthonormal.jpg]]

We can extend this to $n$-dimensional vectors, and we can also extend the
concept to matrices, except that for matrices the orthogonality is defined for
only one matrix and not between two (like vectors).

> [!NOTE] Matrix Orthogonality
> A matrix $U \in \mathbb{R}^{n \times n}$ is **orthogonal** if
> $$U U^\top = U^\top U = I$$
> A direct implication is that $U^\top = U^{-1}$.

Equivalently we can say that $U$'s columns are an **orthonormal basis** of
$\mathbb{R}^n$. So we can see an orthogonal matrix as a collection of column
vectors that are orthonormal with each other.

If two matrices $U_1$ and $U_2$ of size $n \times n$ are orthogonal, then their
product $U_1 U_2$ is also an orthogonal matrix. And this can be proven by

$$(U_1 U_2)^\top (U_1 U_2) = U_2^\top U_1^\top U_1 U_2 = U_2^\top U_2 = I$$

> [!NOTE] Property
> An interesting property is that if $U \in \mathbb{R}^{n \times n}$ is
> orthogonal and $x \in \mathbb{R}^n$ then it holds
>
> $$\| Ux \|_2 = \| x \|_2$$
>
> And more in general
>
> $$(Ux)^\top (Uy) = x^\top y$$
>
> The geometric idea is that the transformation associated to $U$ is a rotation
> or a mirror symmetry.
>
> > [!note] Proof
> > Let's prove first the first case
> >
> > $$
> > \begin{align*}
> >     \| Ux \|_2 &= \sqrt{(Ux)^\top (Ux)} \\
> > &= \sqrt{x^\top U^\top U x} \\
> > &= \sqrt{x^\top x} = \| x \|_2 \\
> > \end{align*}
> > $$
> >
> > For the second case is even simpler
> > $$(Ux)^\top (Uy) = x^\top U^\top U y = x^\top y$$

Let's also notice that two non-zero and orthogonal vectors are also linearly
independent and this can be easily by observing that, if $u$ and $v$ are
orthogonal, then

$$\langle u, v \rangle = u^\top v = 0$$

and if $u$ and $v$ are linearly independent, it means that they're not null and

$$\alpha u + \beta v = 0$$

with $\alpha = \beta = 0$. So we can set

$$
u^\top (\alpha u) + u^\top (\beta v) = \alpha u^\top u + \beta u^\top v =
\alpha u^\top u = 0
$$

But we know that $u \neq 0$ so $u^\top u \neq 0$, and so $\alpha$ must be $0$.
Same reasoning for $\beta$ but this time let's multiply for $v^\top$

$$
v^\top (\alpha u) + v^\top (\beta v) = \alpha v^\top u + \beta v^\top v =
\beta v^\top v = 0
$$

Again we know that $v \neq 0$, so $\beta$ must be $0$.

---

Let's analyze a special case where we consider an orthogonal matrix
$U \in \mathbb{R}^{n \times k}$ with $n > k$. This matrix, by definition has
orthonormal columns $u_1, \dots, u_k$ and so the product

$$U^\top U = V \in \mathbb{R}^{k \times k}$$

is the identity matrix of size $k \times k$ ($V = I$) because, if we compute the
product we will assign to the element in position $(i, j)$ the value

$$
\langle u_i, u_j \rangle = \begin{cases}
0 & \text{if } i \neq j \\
1 & \text{if } i = j
\end{cases}
$$

because in case $u_i \neq u_j$ it means that they are orthonormal vector, and so
their scalar product is $0$; on the other hand if $u_i = u_j$ it means that
their scalar product is

$$\langle u_i, u_j \rangle = u_i^\top u_j = u^\top u = \| u \| = 1$$

because $u_i = u_j = u$ that is orthonormal.

We cannot say the same for $UU^\top$ because in this case we have

$$UU^\top = \sum_{i=1}^k u_i u_i^\top$$

that must be computed and could be any matrix.

## References

- [[linear_algebra]]
- [[matrices]]
- [[vector_norms]]
