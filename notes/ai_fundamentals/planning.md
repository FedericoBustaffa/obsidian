---
id: planning
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Planning

In the classical way of **planning** the aim is to find a **sequence of
actions** to accomplish a goal in a discrete, deterministic, static and fully
observable environment.

This is already done by agents via search algorithms or by logical agents via
inference, and there are ad-hoc languages to implement this kind of stuff, like
**planning domain definition language (PDDL)**.

In this kind of languages it is possible represent a combinatorial number of
actions with a single schema, and in the end return a _plan_.

A core component in PDDL are states, that are basically a **conjunction of
_ground atomic fluents_**, where

- **Ground** means that there are no variables.
- **Atomic** means that is a single literal with no structure.
- **Fluents** is an aspect of the world that changes over time.

Another important component is the **action**, for which we can define a
_schema_ composed by:

- _Name_: the name of the action.
- _Variables_: a list of variables involved in the action.
- _Precondition_: conjuctions of literals.
- _Effect_: conjunctions of literals.

We can assign values to variables, resulting in a ground action.

A set of action schemas defines the **planning domain** and for a problem in the
domain is necessary to add the _initial state_ (formed by a conjunction of
ground fluents) and the _goal state_ (formed by a conjunction of literals).

Given an action with precondition $P$, the action is applicable in a state $s$
if and only if $S \models P$. Every positive literal in $P$ must also hold in
$s$ and every negated laiteral in $P$ must not be present in $s$.

The result of an action is a new state with all fluents from the previous state,
from which it's necessary to

- Remove fluents appearing as negated literals in the action's effects.
- Add fluents appearing as positive literals in an action's effects.

## Algorithms for Planning

In order to find an actual plan, typically we can start from a declarative
representation, and it's possible to proceed in two ways:

- **Forward**: the start is the initial state, then it's necessary to **unify**
  the current state agains the preconditions of each action schema to find the
  _applicable_ actions. Then, by applying each resulting substitution, is
  possible to get a ground action that we can apply.

## References

- [[artificial_intelligence_fundamentals]]
- [[logical_agents]]
- [[first_order_logical_agents]]
