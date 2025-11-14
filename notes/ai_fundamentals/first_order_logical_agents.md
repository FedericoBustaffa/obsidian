---
id: first_order_logical_agents
aliases: []
tags:
  - master
  - ai_fundamentals
---

# First Order Logical Agents

A more powerfull type of logical agent, with respect to propositional logical
agents, are **first order logical agents**, that exploit **first order logic**
(**FOL**) constructs in order to be more general.

Propositional logic is only able to model _facts_, while in the FOL, is
possible to model objects and relations or properties of them. If a property has
only one value for each input, then it can be seen has a function.

In FOL the syntax is composed by

- **Constants**: $5$, _John_
- **Predicates**: _brotherOf_
- **Functions**: _sqrt_
- **Variables**: $x$, $y$
- **Connectives**: $\lnot$, $\land$, $\lor$
- **Equality**: $=$
- **Quantifiers**: $\exists$, $\forall$

Typically in the FOL we have **sentences**, formed by **terms**, that are
logical expressions referring to an object:

- $f(t_1, \dots, t_n)$ with $f$ that is a function.
- Constants
- Variables

An **atomic sentence** is defined by a predicate $p$ or a term equality:

- $p$, $p(t_1, \dots, t_n)$
- $t_1 = t_2$

Atomic sentences linked by connectives are called **complex sentences**.

A sentence is true with respect to a given **model** and **interpretation**,
where

- A **model**, that contains one or more domain elements (objects) and their
  relations.
- An **interpretation** associates a concrete referent to each constant
  (object), predicate and function (relations).

A model with an interpretation is a model in the propositional logic.

## Computational Issues

A common assumption for FOL agents is the _closed world assumption_: atomic
sentences that are not known to be true or false are assumed false. This allows
to get rid of boilerplate stuff that handles incomplete information. In this
kinf of agents we assume one unique referent of each constant, predicate or
function.

Due to the introduction of quantifiers, entailment via model checking is very
expensive or impossible. For example if we have a variable $x$ that can be every
real value in $[1, +\infty]$ the computation never stops.

In case of _universal quantification_, a statement in the form

$$\forall x \, P$$

is true in a model if and only if the sentence $P$ is true for every possible
object associated with $x$ in the model. In propositional logic it is a possibly
infinite conjunction of instances of $x$.

In case of _existential quantifiers_, a statement in the form

$$\exists x \, P$$

is true in a model if and only if the sentnces $P$ is true for at least one
object associated with $x$ in the model. In propositional logic is a possibly
infinite disjunction of instances of $x$.

## First Order Logic Knowledge Base

## Questions

- What is an object?
- What is a relation?
- What is a referent?
- What the interpretation actually does?

## References

- [[artificial_intelligence_fundamentals]]
- [[logical_agents]]
