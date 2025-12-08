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

---

Brief recap of Bayesian probability...

---

More in general, we want the posterior distribution of the **query variables**
$Y$ given the observed values of the **evidence variables** $E$.

Supposing we have unobserved, hidden variables $H$, we have

$$H = X - Y - E$$

with $X$ being the total number of variables. The probability of $Y$ knowing
that $E = e$ is

$$P(Y \mid E = e) = \alpha P(Y, E = e) = \alpha \sum_h P(Y, E = e, H = h)$$

with $\alpha$ a normalizing constant to ensure valid probabilities. This
probabilistic inference with $n$ variables and $d$ possible values per variable
has $O(d^n)$ complexity in time and space for the worst case scenario (the joint
distribution has $d^n$ entries).

Also reasoning in solving _sub-problems_ first can have a big impact because,
for the Bayesian probability, if $a$ and $b$ are independent the following
properties hold

$$
\begin{align*}
P(a \mid b) &= P(a) \\
P(a \land b) &= P(a) \cdot P(b)
\end{align*}
$$

This greatly reduces the size of the joint probability, expecially if all
variables are mutually independent, it goes from $d^n$ to $d n$.

---

By applying the **Bayes' rule**

$$P(b \mid a) = \frac{P(a \mid b) \cdot P(b)}{P(a)}$$

the _prior_ probability $P(b)$ gets updated by becoming the _posterior_
probability $P(b \mid a)$ after observing the _likelihood_ $P(a \mid b)$ of the
event $a$ when $b$ occurs. We can interpret Bayes' rule in a **causal** sense

$$
P(\text{cause} \mid \text{effect})
= \frac{P(\text{effect} \mid \text{cause}) \cdot P(\text{cause})}
{P(\text{effect})}
$$

where $P(\text{cause} \mid \text{effect})$ quantifies the causal direction and
$P(\text{effect} \mid \text{cause})$ quantifies the _diagnostic_ direction.

Another brick to add is **conditional independence** that is defined like
follows: $A$ and $B$ are **conditionally independent** given $C$ if

$$P(A, B \mid C) = P(A \mid C) \cdot P(B \mid C)$$

or equivalently

$$P(A \mid B, C) = P(A \mid C)$$

that in Bayes' terms can be represented as

$$
P(\text{effect}_1, \dots, \text{effect}_n \mid \text{cause})
= \prod_j P(\text{effect}_j \mid \text{cause})
$$

that means that effects are conditionally independent given the cause. To obtain
the posterior of the cause we plug this into the Bayes theorem:

$$
P(\text{cause} \mid \text{effect}_1, \dots, \text{effect}_n) =
\alpha P(\text{cause}) \cdot \prod_j P(\text{effect}_j \mid \text{cause})
$$

## References

- [[artificial_intelligence_fundamentals]]
