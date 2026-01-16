---
id: vector_space_basis
aliases: []
tags:
  - master
  - linear_algebra
---

# Vector Space Basis

Before introducing the concept of **basis** we need to define what is a
generating set of a vector space.

> [!NOTE] Generating Set
> Given a set $A = \{ x_1, \dots, x_n \} \subseteq V$ of vectors, where $V$ is
> a vector space, if every vector in $V$ can be expressed as a linear
> combinations of the vectors in $A$, then $A$ is called a **generating set of**
> $V$.

A direct implication of the generating set is the _span_ of a set of vectors.

> [!NOTE] Span
> The **span** is the set of all possible linear combinations of a set of
> vectors.

In fact, if the span of a set $A$ of vectors is equal to a vector space $V$, we
say that $A$ _spans_ $V$. So of course a generating set $A$ of a vector space
$V$ _spans_ $V$.

Now we can define also a special case of generating set, the _minimal generating
set_ of $V$.

> [!NOTE] Minimal Generating Set
> A set of vector $A$ is a **minimal generating set** for $V$ if and only if
> there is no other $\hat{A}$ such that
> $$\hat{A} \subseteq A \subseteq V$$
> that _spans_ $V$.

So in other words $A$ has the minimum number of vectors to represent every
vector in $V$ as linear combinations of its vectors.

> [!NOTE] Basis
> If a set $B$ of linearly independet vectors is also a minimal generating set
> of $V$, then it's called a **basis** of $V$.

This has some interesting implications like

- Every linear combination of $B$ is unique.
- $B$ is both a minimal generating set and also a maximal linearly independent
  set of vectors in $V$. In other words, adding a vector to this set will break
  the linear independence.

Even though a basis seems to have some unique properties, a vector space can
have multiple basis but all of the same cardinality.

Another interesting fact is that the **dimension** of $V$, when $V$ is a finite
dimensional space, is the number of basis vectors of $V$.

## Change of Basis

> [!example] Change of coordinates system
> Let's take the vector $v = (2, 1)$ and let's consider the canonical basis
>
> $$
> \begin{bmatrix} 1 \\ 0 \end{bmatrix}
> \begin{bmatrix} 0 \\ 1 \end{bmatrix}
> $$
>
> as coordinates system. If we consider another basis, for example
>
> $$
> \begin{bmatrix} 1 \\ -1 \end{bmatrix}
> \begin{bmatrix} 1 \\ 1 \end{bmatrix}
> $$
>
> we need to solve the following system
>
> $$
> \begin{bmatrix} 1 & 1 \\ -1 & 1 \end{bmatrix}
> \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}  =
> \begin{bmatrix} 2 \\ 1 \end{bmatrix}
> $$
>
> in order to find the new coordinates of $v$.
>
> $$
> \begin{cases}
> x_1 + x_2 = 2 \\
> -x_1 + x_2 = 1
> \end{cases} \implies
> \begin{cases}
> x_1 = 2 - x_2 \\
> x_2 = 3/2
> \end{cases} \implies
> \begin{cases}
> x_1 = 1/2 = 0.5 \\
> x_2 = 3/2 = 1.5
> \end{cases}
> $$
>
> so the new coordinates of $v$ with the new basis are $v = (0.5, 1.5)$.

## References

- [[linear_algebra]]
- [[linear_independence]]
