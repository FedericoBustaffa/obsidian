---
id: gustafson_law
aliases: []
tags:
  - master
  - parallel_computing
---

# Gustafson's Law

The **Gustafson's law** tries to take into account also weak scalability in a
more general formula because is quite common that when we use more processors,
we also want to increase the problem size. In this case we talk about **scaled
speedup**, that incorporates such scenarios when calculating the achievable
speedup.

As before we consider the sequential version as baseline

$$
T_{\alpha \beta}(1) = \alpha \cdot T_\text{ser} + \beta \cdot T_\text{par} =
\alpha \cdot f \cdot T_c (1) + \beta \cdot (1-f) \cdot T_c(1)
$$

where $\alpha$ is a scaling function for the part of the program that cannot be
parallelized and $\beta$ a scaling function for the parallelized part. Similarly
to Amdahl's law we can now compute the speedup as follows

$$
S_{\alpha \beta} (p) =
\frac{T_{\alpha \beta}(1)}{T_{\alpha \beta}(p)} \leq
\frac{\alpha \cdot f \cdot T_c (1) + \beta \cdot (1-f) \cdot T_c(1)}
{\alpha \cdot f \cdot T_c (1) + \frac{\beta \cdot (1-f) \cdot T_c(1)}{p}} =
\frac{\alpha \cdot f + \beta \cdot (1-f)}
{\alpha \cdot f + \frac{\beta \cdot (1-f)}{p}}
$$

As before we can use the ratio $\gamma = \frac{\beta}{\alpha}$ of the problem
complexity scaling between the parallelizable part and the non-parallelizable
part.

$$
S_\gamma (p) \leq \frac{f + \gamma \cdot (1-f)}
{f + \frac{\gamma \cdot (1-f)}{p}}
$$

There are two notable cases for the possible values of $\gamma$:

- $\gamma = 1$ ($\alpha = \beta$): we obtain the Amdahl's law.
- $\gamma = p$: we obtain the Gustafson's law, that means that the
  parallelizable part grows linear in $p$ while the non-parallelizable part
  remains constant.
  $$S(p) \leq f + p \cdot (1 - f) = p + f \cdot (1-p)$$

## Functional Dependency of Scaled Speedup

If we parametrize the speedup with $\gamma = p^\delta$ we can reason in terms of
$\delta$ and we can again notice that for

- $\delta = 0$ we have $\gamma = 1$, so we obtain the Amdahl's law.
- $\delta = 1$ we have $\gamma = p$, so we obtain the Gustafson's law.

We can now use this formula to compute the efficiency as follows

$$E_\gamma (p) = \frac{S_\gamma(p)}{p}$$

that becomes

$$
E_\gamma (p) \leq
\frac{f + (1 - f) \cdot \gamma}{p \cdot f + (1-f) \cdot \gamma}
$$

Computing the limits for $p \to \infty$ and for different values of $\gamma$ we
obtain

$$
\begin{align*}
\lim_{p \to \infty} E_{\gamma=p} (p) = 1 - f &&
\lim_{p \to \infty} E_{\gamma=1} (p) = 0
\end{align*}
$$

**Linear** parametrizations of the arguments that guarantee constant efficiency
are called **iso-efficiency lines** and can help to answer the following questions:

- How much must $\delta$ increase in order to preserve parallel efficiency for
  increasing $p$?
- How much does the parallelizable part of our problem need to grow if we want
  to keep the same efficiency while using more processing units.

In this way is possible to fix the efficiency and try to tune $p$ and $\delta$
and find a nice combination of these two parameters.

## References

- [[parallel_computing]]
- [[amdahl_law]]
