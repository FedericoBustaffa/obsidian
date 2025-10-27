---
id: low_rank_approximation
aliases: []
tags: []
---

# Low-Rank Approximation

Sometimes we have to deal with rectangular matrices that are _big_ and _complex_
but its information is contained only in few directions. In that case is
possible to approximate it in a simpler, smaller and so, with a lower rank
matrix, mantaining as much as possible its information.

Given $A \in \mathbb{R}^{m \times n}$ and an integer $k < \text{rank}(A)$, the
**low-rank approximation** of $A$ of rank $k$ is a matrix $\tilde{A}$ such that

$$\tilde{A} = \arg \min_{\text{rank}(B) = k} \| A - B \|_F$$

that is, we're searching the, among all the matrices with rank $k$, the closest
to $A$ with respect to Frobenius (or spectral) norm.

> [!NOTE] Eckart-Young Theorem
> Given $A \in \mathbb{R}^{m \times n}$, the best approximation of rank $k$ is
> given by the _truncated_ SVD of $A$.

In other words if

$$A = U \Sigma V^\top = \sigma_1 u_1 v_1^\top + \cdots + \sigma_r u_r v_r^\top$$

then a minimizer of

$$\min_{\text{rank}(B) \leq k} \| A - B \|_F$$

is given by

$$A = U \Sigma V^\top = \sigma_1 u_1 v_1^\top + \cdots + \sigma_k u_k v_k^\top$$

## References

- [[linear_algebra]]
- [[singular_value_decomposition]]
- [[matrix_norms]]
- [[computational_mathematics]]
