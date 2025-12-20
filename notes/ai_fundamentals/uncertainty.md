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

In **Bayesian probability theory**, probability represents an agent's **degree
of belief** of a certain set of sentences, and the _uncertainty_ is summarized
by a probability distribution.

In the **Bayesian view** of probability there is an initial **belief** of an
event (_prior_) that is updated (_posterior_) based on observations
(_likelihood_).

More formally the _prior_ $P(H)$ is the initial belief on an hypothesis, the
_likelihood_ $P(E \mid H)$ quantifies how much the hypothesis explains the
evidence, and the _posterior_ $P(H \mid E)$ is the updated belief after
observing $E$:

$$P(H \mid E) = \frac{P(E \mid H) \cdot P(H)}{P(E)}$$

In this sense, this models _learn_ by updating their believes on the world, not
by searching for absolute truth.

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

We are considering a sample space $\Omega$ that is the set of all possible
worlds (or models), that are **mutually exclusive** and **exhaustive**. A
probability model associates a numerical probability $P(w)$ with each possible
world.

The basic axioms of probability theory are that $P(w)$ must be between $0$ and
$1$ and that the sum of probabilities of all possible worlds must be exactly
$1$.

The **unconditional** (or **prior**) probability represents the degree of belief
in a proposition in absence of any other information.

Other useful relations are

- $P(\lnot a) = 1 - P(a)$
- $P(a \lor b) = P(a) + P(b) - P(a \land b)$

The **conditional** probability of $a$ given $b$ is defined as

$$P(a \mid b) = \frac{P(a \land b)}{P(b)}$$

meaning that

$$P(a \land b) = P(a \mid b) \cdot P(b)$$

If $a$ and $b$ are **independent** random variables then it holds

$$P(a \mid b) = P(a)$$

Generalizing conditional probability to more events we obtain the **chain rule
of probability**:

$$
P(a_1 \land \cdots \land a_n) =
\prod_{j=1}^n P(a_j \mid a_1 \land \cdots \land a_{j-1})
$$

that will be useful in many cases.

---

So in principle is possible to represent the world by a set of pairs
variable-value, where variables will be random variables and values their
assignment. In this way is possible to define a probability distribution of all
possible values of a random variable.

Now is possible to make inference by **joint probability** of each variable

$$P(A = a \land B = b)$$

or more concisely $P(a, b)$. Another useful things is called
**marginalization**, that defines the probability of a certain outcome $A$ as

$$P(A) = \sum_b P(A, B = b)$$

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

---

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

---

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

## Naive Bayes Model

The **naive Bayes model** is implemented by a graph that factorizes nicely our
world and probabilities if random variables are independent and it scales
linearly with the number $n$ of variables.

![Naive Bayes Graph|400](/files/naive_bayes_graph.png)

There are no horizontal links between effects. For example given a cause and two
effects we have

$$
\begin{align*}
P(\text{effect}_1, \text{effect}_2, \text{cause}) &= \\
P(\text{effect}_1, \text{effect}_2 \mid \text{cause}) P(\text{cause}) &= \\
P(\text{effect}_1 \mid \text{cause}) P(\text{effect}_2 \mid \text{cause})
P(\text{cause}) &= \\
\end{align*}
$$

and this means that we can deal with three small tables instead of one giant
table.

So now we can answer queries by applying marginalization, also based on some
assumption. But still the number of terms can be high due to the fact that we
need to take into account also probabilities of unknown environment states.

Actually we don't have to, and what is usually done is to keep in the $KB$ only
what we know, what is _adjacent_ to it is called **frontier** and we ignore the
rest. This last step can usually be done exploiting independence.

Now in order to answer a query on state in the frontier we have much less
computation to make.

## References

- [[artificial_intelligence_fundamentals]]
