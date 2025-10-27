---
id: pseudoinverse
aliases: []
tags: []
---

# Pseudoinverse Matrix

The concept of **pseudoinverse** of a matrix is a generalization of the inverse
for square matrices. The pseudoinverse is used for example to solve
overdetermined systems like a regression problem.

## Least Squares

One definition for the pseudoinverse of a matrix is given by the **least
squares** problem, where we want to solve

$$\min_x \| Ax - y \|^2$$

Expand the formula gives us a quadratic form for the problem, of which we can
calculate the **gradient**, that is

$$(A^\top A)x - A^\top y$$

and by setting it to $0$, in order to minimize the previous quantity, we obtain

$$
\begin{align*}
(A^\top A)x - A^\top y &= 0 \\
(A^\top A)x &= A^\top y \\
x &= (A^\top A)^{-1} A^\top y \\
\end{align*}
$$

If we notice, this form is very similar to the canonical form used to solve
square systems:

$$A x = y \iff x = A^{-1} y$$

For rectangular matrices we can instead define the concept of _pseudoinverse_,
denoted as $A^+$

$$A^+ = (A^\top A)^{-1} A^\top$$

that let us solve the rectangular system by computing

$$x = A^+ y$$

In fact if we have a matrix $A \in \mathbb{R}^{n \times n}$ that is invertible,
its pseudoinverse is equal to its inverse:

$$A^+ = (A^\top A)^{-1} A^\top = A^{-1} A^{-\top} A^\top = A^{-1}$$

It's important to notice that the multiplication for the pseudoinverse it's, in
general, not commutative, for the least squares problem, we usually have a
matrix $A \in \mathbb{R}^{m \times n}$ with $m \geq n$:

- Case $A^+ A$ with $A^\top A$ invertible:
  $$A^+ A = (A^\top A)^{-1} A^\top A = I_n$$
  but if $A^\top A$ is not invertible, it means that $A$ has not full column
  rank, and most important is not possible to solve the system.
- Case $A A^+$:
  $$
  A A^+ = A (A^\top A)^{-1} A^\top = A A^{-1} A^{-\top} A^\top =
  (A A^{-1})^\top = I_m
  $$
  but this works only if $A$ is square and invertible or if $A$ is
  underdetermined with full row rank, so in general $A A^+ \neq I_m$.

To check when $A^\top A$ is invertible we need to know if $A$ has full column
rank.

## Singular Value Decomposition

Given a matrix $A = U \Sigma V^\top$ with rank $r$ and that is _thin_, then
its pseudoinverse is defined as

$$A^+ = V \Sigma^+ U^\top$$

where

$$
\Sigma^+ = \begin{bmatrix}
1/\sigma_1 & & & \\
& \ddots & & \\
& & 1 / \sigma_r & \\
& & & 0
\end{bmatrix}
$$

A special case, is when $A$ has full column rank, then, like we saw before, it
holds

$$A^+ = (A^\top A)^{-1} A^\top$$

so in this case is also possible to write

$$A^+ = V \Sigma^+ U^\top = V \Sigma^{-1} U^\top$$

because $\Sigma$ is invertible if and only if $A$ has full column rank, in fact
we can obtain the formula by noticing that

$$
\begin{align*}
(A^\top A)^{-1} A^\top &= (V \Sigma U^\top U \Sigma V^\top)^{-1} V \Sigma
U^\top) \\
&= (V \Sigma^2 V^\top)^{-1} V \Sigma U^\top \\
&= V \Sigma^{-2} V^\top V \Sigma U^\top = V \Sigma^{-1} U^\top
\end{align*}
$$

that is the same thing we get with the previous definition.

## References

- [[linear_algebra]]
- [[matrices]]
- [[singular_value_decomposition]]
- [[least_squares]]
