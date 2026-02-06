---
id: probabilistic_reasoning
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Probabilistic Reasoning

A direct evolution of the _naive bayes model_ is the **bayesian network**, which
is a **direct acyclic graph**, representing dependency among variables. Each
node of the graph represents a random variable, each depending on its parents:

$$P(X \mid \text{parents}(X))$$

For easy problems we can employ a simple **conditional probability table (CPT)**
that gives the value of $X$ for each combination of values of its parents. The
advantage of bayesian networks is that, once the structure of the network is
defined, information about each conditional distribution is sufficient to
specify the full joint distribution, mititgating the combinatorial explosion of
terms.

A bayesian network defines the full joint distribution as the product of the
local conditional distributions:

$$P(x_1, \dots, x_n) = \prod_i P(x_i \mid \text{Parent}(X_i))$$

More in general, each node $X$ is conditionally independent of its
**_non-descendants_** given its parents and that's why is possible to easily
factorize the joint distribution.

Another concept is the **Markov blanket**, which says that each node $X$ is
conditionally independent of any other node given its Markov blanket
$\text{MB}$:

$$
\text{MB} (X) = \text{Parents}(X) + \text{Children} (X) + \text{Parents}
(\text{Children} (X))
$$

To build a bayesian network using the chain rule of probability we can follow
this algorithm:

1. Identify the variables $X_1, \dots, X_n$.
2. Choose an ordering of the variables. Any order will work but better that
   causes precede effects to have a more compact network.
3. For each variable $X_i$
   1. Add $X_i$ as a node of the network.
   2. Select the minimal set of parents from $X_1, \dots, X_{i-1}$ such that
      $$P(X_i \mid \text{Parents}(X_i)) = P(X_i \mid X_1, \dots, X_{i-1})$$
   3. Insert a link from $X_i$ to all $\text{Parents}(X_i)$ and fill the CPT.

The chain rule is used considering that we need to compute $P(X_1, \dots, X_n)$:

$$P(X_1, \dots, X_n) = \prod_i P(X_i \mid X_1, \dots, X_{i-1}$$

after we exploit the local semantics (conditional independence) to obtain

$$
\prod_i P(X_i \mid X_1, \dots, X_{i-1} =
\prod_i P(X_i \mid \text{Parents} (X_i)
$$

## Compact Conditional Distributions

In case random variables are continuous, CPTs become of infinite size and also
grows exponentially with the number of parents. To compactly define infinite
cases we can exploit the **canonical distributiion** in three ways

- **Deterministic nodes**: let $f$ be a function, the variable $X$ is defined as
  $$X = f(\text{Parents} (X))$$
- **Context specific independence**: $X$ can be conditionally independent of
  some of its parents given certain values of other parents.
- **Noisy-OR** models multiple, non-interacting causes (binary variables),
  reducing the CPTs size from $\mathcal{O} (2^\text{\# parents})$ to
  $\mathcal{O} (\text{\# parents})$

### Noisy OR

The **noisy-OR** is based on two assumptions

1. Parents include all causes. If not is possible to add a **leak node** that
   covers all other causes.
2. The causal relationship between cause and effect can be **inhibited** and the
   **inhibition probability** $q_i$ is independent of inhibition of other
   causes.

The second assumption implies that a given effect is false if and only if its
true parents are inhibited.

Then, the CPT for a given node with parents $U$ under the previous assumptions
can be computed compactly via

$$
P(X \mid U_1, \dots U_j, \lnot U_{j+1}, \dots, \lnot U_k) =
1 - \prod_{i=1}^j q_i
$$

The parents that are false (inhibited) do not contribute to the probability
mass; only the inhibition probability of the true parents counts.

## Hybrid Bayesian Networks

Can happen that some nodes are discrete and some are continuous. Continuous
values can be discretized in ranges, leading to possibly large CPTs and
numerical errors.

In order to implement **hybrid bayesian networks** is necessary to specify

- Conditional distribution of continuous variable given discrete or continuous
  pasrents.
- Conditional distribution of discrete varaible given continuous parents.

If a network contains only continuous variable with linear gaussian
distributions, then the joint distribution is gaussian (multivariate over all
variables).

If there are discrete variables as parent, then, given any assignment of the
discrete variable, the distribution over continuous ones is Gaussian
(multivariate over all continuous variables).

## Inference

### Variable Elimination

## References

- [[artificial_intelligence_fundamentals]]
- [[uncertainty]]
