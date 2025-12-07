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

## References

- [[artificial_intelligence_fundamentals]]
