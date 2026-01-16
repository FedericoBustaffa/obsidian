---
id: vector_norms
aliases: []
tags:
  - master
  - linear_algebra
---

# Vector Norms

In order to measure vectors _length_ there are some metrics that can be used,
called **norms**, functions

$$\| \cdot \| : \mathbb{R}^n \to \mathbb{R}$$

but to be a _norm_ a function must respect these three properties:

1. $\| v \| \geq 0$ for all $v$ and $\| v \| = 0 \iff v = 0$.
2. $\| \alpha \cdot v \| = |\alpha| \cdot \| v \|$ for all
   $\alpha \in \mathbb{R}$ and for all vectors $v$.
3. **Triangular inequality**: $\| v + w \| \leq \| v \| + \| w \|$ for all $v, w$.

There are several norms, used for different purposes, here some of the most
popular:

- **Euclidean**: if we think about it in two or three dimensional space it's the
  direct distance between a point and the origin
  $$\| v \|_2 = \sqrt{\sum_{i=1}^n v_i^2}$$
  where $v_i$ is the $i$-th component of the vector $v$. It can also be written
  as the square root of product between the vector $v$ transposed and $v$ itself
  $$\| v \|_2 = \sqrt{\sum_{i=1}^n v_i^2} = \sqrt{v^\top v}$$
  that is the definition of the square root of the scalar product between $v$
  and itself.
  $$
  \| v \|_2 = \sqrt{\sum_{i=1}^n v_i^2} =
  \sqrt{v^\top v} = \sqrt{\langle v, v \rangle}
  $$
  all these equalities can be useful for calculations.
- **Manhattan**: this represents the Manhattan distance between the point and
  the origin and it is basically the sum of the absolute values of each
  component of the vector:
  $$\| v \|_1 = \sum_{i=1}^n | v_i |$$
- **Infinite**: this norm will take only the maximum among each component's
  absolute value:
  $$\| v \|_\infty = \max_{i = 1, \dots, n} | v_i |$$

Generally the euclidean norm is nice because there are many matrices that
preserve it, for example **orthogonal** matrices.

A useful fact that is indipendent of the norm used is that for any vector
$v \neq 0$ we can write

$$v = \alpha \cdot w$$

where $\alpha = \| v \|$ and $w = v / \| v \|$ is the unit vector.

## References

- [[linear_algebra]]
