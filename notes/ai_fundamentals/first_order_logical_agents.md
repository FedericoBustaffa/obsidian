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

However with the introduction of quantifiers, entailment via model checking is
very expensive or impossible. For example if we have a variable $x$ that can be
every real value in $[1, +\infty]$ the computation never stops.

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

In order to build a FOL agent we need to build a **FOL knowledge base** and this
is typically done by

1. Identify questions.
2. Assemble the relevant knowledge.
3. Decide on a vocabulary predicates, functions and constants.
4. Encode general knowledge about the domain.
5. Encode a specific description of the problem instance.
6. Ask queries to the inference algorithm to get answers.
7. Debug and evaluate the knowledge base.

For example in the Wumpus World we want to have an interface _ask_ and _tell_;
the agent perceives a _breeze_ at $t = 5$, so

- _Tell (KB, Percept([Smell, Breeze], 5))_
- _Ask (KB, $\exists a$ Action($a$, 5))_

And the answer should be _$a$ = Shoot_.

A useful construct is the **substitution** operator $\sigma$ and it works by
assigning (or substituting) a term value to a variable. For example we can have
a predicate

$$S = \text{Smarter} (x, y)$$

and a substitution

$$\sigma = \{ x / \text{Mario}, y / \text{Luigi} \}$$

the resulint sentence is

$$S \sigma = \text{Smarter} (\text{Mario}, \text{Luigi})$$

This is used when the agent query the $KB$, for example _Ask($KB$, $S$)_; the
answer will be some or all substitutions $\sigma$ such that $KB \models S
\sigma$.

## Universal and Existential Instantiations

The **universal instantiation (UI)** is defined by any sentence obtained by
replacing a _universally quantified_ variable with a **ground term** that can be
inferred.

$$\frac{\forall v \, a}{\sigma(\{ v / g \}, \, a)}$$

that applies for every variable $v$, sentence $a$ and ground term $g$.

## Questions

- What is an object?
  - An element of the domain like a constant.
- What is a relation?
  - A set of tuples of objects for which a predicate is true.
- What is a referent?
  - A symbol that links to an actual thing in the domain.
- What the interpretation actually does?
  - A map that links FOL syntax to the model semantics. For example assigns a
    objects to every constant or a relation to every predicate.

## References

- [[artificial_intelligence_fundamentals]]
- [[logical_agents]]
