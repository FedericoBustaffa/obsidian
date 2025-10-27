---
id: matrix_norms
aliases: []
tags: []
---

# Matrix Norms

As vectors, also matrices has the concept of _norm_ and it is quite similar; in
fact a **matrix norm** is a function

$$\| \cdot \| : \{ \text{matrices} \} \to \mathbb{R}$$

that needs to satisfy $4$ properties:

- $\| A \| \geq 0$ for all $A$ and $\| A \| = 0$ if and only if $A = 0$.
- $\| \alpha A \| = |\alpha| \cdot \| A \|$ for all scalars $\alpha$ and all
  $A$.
- $\| A + B \| \leq \| A \| + \| B \|$ for all $A, B$ (triangle inequality).
- $\| A B \| \leq \| A \| \cdot \| B \|$ for all $A, B$ (sub-multiplicativity)

Starting from a vector norm is possible to define a matrix norm, defining the
so called **induced matrix norm**, that is defined, given a vector norm
$\| \cdot \|$ as

$$\| A \| = \max_{v \neq 0} \frac{\| A v \|}{\| v \|}$$

in fact we also can say that if $\| \cdot \|$ is an induced matrix norm, then
for any $A$ and for any $v$, we have

$$\| A v \| \leq \| A \| \cdot \| v \|$$

## Spectral Norm

The matrix norm induced by the Euclidean norm is called **spectral norm** that
has an interesting property: if $Q_1$ and $Q_2$ are two orthogonal matrices of
dimension $n \times n$ and $A \in \mathbb{R}^{n \times n}$ then

$$\| Q_1 A Q_2 \| = \| A \|$$

and this is because

$$
\begin{align*}
\| Q_1 A Q_2 \| &= \max_{v \neq 0} \frac{\| Q_1 A Q_2 v \|}{\| v \|} \\
    &= \max_{v \neq 0} \frac{\| A Q_2 v \|}{\| Q_2 v \|} \\
    &= \max_{z \neq 0} \frac{\| A z \|}{\| z \|} = \| A \|
\end{align*}
$$

And this is interesting because in the SVD we have the left and right matrices
that are orthogonal like $Q_1$ and $Q_2$ and so

$$\| A \| = \| U \Sigma V^\top \| = \| \Sigma \| = \sigma_1$$

that is the maximum singular value of $A$.

## Frobenius Norm

Another interesting norm that has a relation with SVD is the **Frobenius norm**

$$\| A \|_F = \sqrt{\sum_{i,j} a_{ij}^2}$$

that is, like the spectral norm, _unitary invariant_, that is, for $Q_1$ and
$Q_2$ orthogonal it holds

$$\| Q_1 A Q_2 \|_F = \| A \|_F$$

and this implies that

$$
\| A \|_F = \| U \Sigma V^\top \|_F = \| \Sigma \|_F =
\sqrt{\sigma_1^2 + \cdots + \sigma_n^2}
$$

## References

- [[linear_algebra]]
- [[vector_norms]]
- [[singular_value_decomposition]]
