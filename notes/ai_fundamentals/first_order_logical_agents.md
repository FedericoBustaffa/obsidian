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

## Questions

- What is an object?
- What is a relation?
- What the interpretation actually does?

## References

- [[artificial_intelligence_fundamentals]]
- [[logical_agents]]
