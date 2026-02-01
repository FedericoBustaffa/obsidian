---
id: least_squares
aliases: []
tags:
  - master
  - computational_mathematics
  - machine_learning
---

# Least Squares

Some linear system cannot be solved exactly, for example in a system $Ax = y$,
where $A$ is not invertible, has $0$ or infinite solutions.

Another case is when the system is _overdetermined_, so that we need to satisfy
more equations than the number of unknow values.

In both cases is possible to find _"the best"_ possible solution, that is, the
solution that is the **closest** to the wanted solution.

> [!EXAMPLE]
> We have two foods, each with their own carbohydrates and protein intakes
> $$A = \begin{bmatrix} 40 & 80 \\ 10 & 20 \end{bmatrix}$$
> For our ideal diet we want a certain amount of carbohydrates and proteins.
> $$y = \begin{bmatrix} 100 \\ 50 \end{bmatrix}$$
> So the amount of each food can be found by solving the following linear system
>
> $$
> \begin{bmatrix} 40 & 80 \\ 10 & 20 \end{bmatrix}
> \begin{bmatrix} x_1 \\ x_2 \end{bmatrix} =
> \begin{bmatrix} 100 \\ 50 \end{bmatrix}
> $$
>
> Unfortunately $A$ is not invertible and so we can't have an exact solution,
> but we can find the closest $x$ vector, such that
> $$\| Ax - y \|$$
> is minimized. For the moment let's take a solution given by an oracle
> $$x = \begin{bmatrix} 0.5294 \\ 1.0588 \end{bmatrix}$$
> that gives us the following vector $Ax$
> $$Ax = \begin{bmatrix} 105.882 \\ 26.471 \end{bmatrix}$$
> As we can see is different from $y$ but it's the closest solution we can get.

A way to find the best possible $x$ is by solving the **least squares** problem,
that aims to minimize the quadratic distance between the wanted solution and the
one we found.

$$\min_x \| Ax - y \|^2$$

The quadratic distance (or error) is a common choice to get a _differentiable_
function, that can be minimized with _gradient-based_ methods. But for now what
we are interested in is what we can do starting from that formula:

$$
\begin{align*}
\| Ax - y \|^2 &= (Ax - y)^\top (Ax - y) \\
    &= ((Ax)^\top - y^\top) (Ax - y) \\
    &= (x^\top A^\top - y^\top) (Ax - y) \\
    &= x^\top A^\top A x - x^\top A^\top y - y^\top A x + y^\top y \\
    &= x^\top A^\top A x - x^\top A^\top y - (A x)^\top y + y^\top y \\
    &= x^\top A^\top A x - x^\top A^\top y - x^\top A^\top y + y^\top y \\
    &= x^\top A^\top A x - 2 \langle x, A^\top y \rangle + \langle y, y \rangle \\
\end{align*}
$$

So minimize $\| Ax - y \|^2$ is equivalent to minimize that equation. Let's make
one more step that will make sense in a while and let's divide everything by
$2$, so that the problem becomes

$$
\min_x \frac{1}{2} \| Ax - y \|^2 =
\min_x \left( \frac{1}{2} x^\top A^\top A x - \langle x, A^\top y \rangle +
\frac{1}{2} \langle y, y \rangle \right)
$$

Let now $Q = A^\top A$ and $q = -A^\top y$, the problem can be rewritten as

$$
\min_x \frac{1}{2} \| Ax - y \|^2 =
\min_x \left( \frac{1}{2} x^\top Q x + \langle q, x \rangle +
\frac{1}{2} \langle y, y \rangle \right)
$$

This problem **has a unique solution** when $Q$ is SPD, but most important the
**gradient** of that equation is

$$Qx + q = A^\top A x - A^\top y$$

To minimize the objective function it's necessary to let the gradient be $0$,
and so we obtain that

$$A^\top A x - A^\top y = 0$$

So now we have a strategy to solve the _least squares_ problem made of $3$ key
points:

1. Compute $A^\top A$
2. Compute $A^\top y$
3. Solve $(A^\top A) x = A^\top y$ (**normal equations**)

We end up with a problem whose solution gives us the closest possible vector to
$y$.

> [!NOTE] Theorem
> The least squares problem $\min_x \| Ax - y \|^2$ has a unique solution if and
> only if $A$ has full column rank.

To solve the _normal equations_ we need the **pseudoinverse** of $A$, that is
defined as $(A^\top A)^{-1} A^\top$, and that let us generalize the concept of
inverse for rectangular (overdetermined) linear systems.

In this way is possible to solve the system by computing

$$x = (A^\top A)^{-1} A^\top y$$

or, a more compact form

$$x = A^+ y$$

where $A^+ = (A^\top A)^{-1} A^\top$ is the pseudoinverse of $A$.

## References

- [[computational_mathematics]]
- [[eigenvalues_eigenvectors]]
- [[pseudoinverse]]
- [[gradient_descent]]
- [[conjugate_gradient]]
