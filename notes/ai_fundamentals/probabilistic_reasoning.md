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

For easy problems we can employ a simple **conditional probability table** that
gives the value of $X$ for each combination of values of its parents. The
advantage of bayesian networks is that, once the structure of the network is
defined, information about each conditional distribution is sufficient to
specify the full joint distribution, mititgating the combinatorial explosion of
terms.

## References

- [[artificial_intelligence_fundamentals]]
- [[uncertainty]]
