---
id: uncertainty
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Uncertainty

Real world problems contain uncertainty due to partial observability,
nondeterminism or adversaries.

In large domain contexts we may fail to predict well what to do for three main
reasons:

- **Lazyness**: it is too much work to list the complete set of antecedents or
  consequents needed to ensure an exceptionless rule.
- **Theoretical ignorance**: there is no complete theory for the domain.
- **Practical ignorance**: even if all the rules are known, there's still
  uncertainty due to the fact that all the necessary tests are not o cannot be
  done.

An agent has only a _degree of belief_ in the relevant sentences and the
rational decision depends on both the **relative importance** of various goals
and the **likelihood** that they will be achieved.

## Bayesian Probability

In **probability theory** there is a **degree of belief** of a certain set of
sentences and the _uncertainty_ is summarize and the _uncertainty_ is
summarized.

In the **Bayesian view** of probability there is an initial **belief** of an
event (_prior_) that is updated (_posterior_) based on observations
(_likelihood_).

An AI agent needs a preference among different possible outcomes of various
plans and so we come up with

- **Utility theory**: every state has a **degree of utility** and the agent will
  prefer the higher utility outcomes.
- **Decision theory**: the choice is based on utility theory and probabilities.

An agent is **rational** if and only if it chooses the action with the highest
expected utility, averaged over all the possible outcomes of the action. In
other words the agent will take into account the utility of an outcome and the
probability it will be true in the end.

This is called **principle of maximum expected utility (MEU)** and it is useful
to move from a setting where an agent believes that a proposition is either true
or false, to a setting where it has a **continuous** degree of belief on a given
proposition being true or false.

## References

- [[artificial_intelligence_fundamentals]]
