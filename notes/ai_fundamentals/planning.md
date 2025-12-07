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
representation, and it's possible to proceed in two ways: _forward_ or
_backward_.

In the **forward** case the start is the initial state, then it's necessary to
**unify** the current state agains the preconditions of each action schema to
find the _applicable_ actions. Then, by applying each resulting substitution, is
possible to get a ground action that we can apply.

In the **backward** the start is the goal state and considering **relevant
actions**, we can say that, their effect _unifies_ with one of the goal's
literals and none of its effects negates any part of the goal. Applying any
relevant until the initial state is reached.

As many backward approaches the branching factor is reduced, because only
relevant actions are considered (DFS-like), while the forward approach goes on
until the goal state is reached (BFS-like).

## Heuristics

Even for planning algorithms is possible to design **heuristics** by thinking
about exact solutions for relaxed problems.

For example we can use the **ignore precondition heuristic**, that assumes that
every action is applicable (drop preconditions). From each action's effect keep
only literals present in the goal. Find the minimum number of actions needed
such that the union of the actions' effects satisfy the goal. This is an
_admissible_ heuristic that leads to a _greedy_ algorithm for an approximate
solution.

Another is the **ignore delete list heuristic** that removes negative literals
from actions' effect. This allows to make monotonic steps towards the goal since
no action can undo previous steps.

### Domain Independent Pruning

The **symmetry reduction heuristic** prunes every symmetric branches of the tree
but one.

The **forward pruning** prunes away branches based on a _preferred action_,
getting a relaxed plan that solves a relaxed version of the problem.

### Reducing State Space Size

Another approach is based on the reduction of state space size, unlike action
based heuristics.

The **state abstraction heuristic** applies a _many to one_ mapping from states
to the abstract representation. It removes some fluents to get an approximate
solution and, if possible, adds back some fluents to recover the exact solution.

The **decomposition heuristic** divides a problem into parts, solving each part
independently, and then combining the parts. For this a **sub-goal independence
assumption** must be held: the cost of solving a conjunction of subgoals is
approximated by the sum of the costs of solving subgoal independently.

## Hierarchical Planning

In order to mitigate _explosion of action_ in the final plan is possible to
abstract away some details and build hierarchical structures.

- **High level action (HLA)**: admits one or more refinements to a sequence of
  actions.
- **High level plan (HLP)**: a sequence of HLAs that, in its implementation
  concatenates the implementation of all HLAs. HLP achieves the goal if at least
  one of its implementation achieves the goal.

An example of this is the **hierarchical forward planning** that, starting from
a HLP with a single HLA, that HLA needs to reach the goal. It uses BFS to find
possible refinements of each HLA in the current plan.

## Sensorless Planning

An extension of planning can be defined also for **sensorless** agents, that
keep a _belief state_ and act under an **open-world assumption** (if a fluent
does not appear, then its value is **unknown**).

The agent acts under a **conditional effect**, where a _condition_ is a logical
formula to be compared against the current belief and the _effect_ is a formula
that describes the resulting state.

In this case can happen that the action's effect might depend on the unknown
state and dealing with all possible ramifications easily make the number of
fluents in a belief state grows exponentially.

## Online Planning

A potentially more flexible apprach is **online planning** where the agents
plans as it explores more the environment. This can be useful for
non-deterministic environments, for example when

- **Execution monitoring**: there is the need for for a new plan.
- There are too many contingencies to prepare for.
- A contingency is not prepared, **replanning** is required.
- The agent's model of the world turns out to be incorrect and so a replanning
  is needed.

The are basically three appraches for this:

- **Action monitoring**: before executing an action, the agent verifies that all
  the preconditions still hold.
- **Plan monitoring**: before executing an action, the agent verifies that the
  remaining plan will still succeed.
- **Goal monitoring**: before executing an action, the agent checks to see if
  there is a better set of goals it could be trying to achieve.

## Constraints

The classical planning takes into account only the actions sequence, but doest
consider the actual time or resources needed for every action, in order to
performed.

In this way we can omit some **constraints** given by the problem, failing to
solve it. A more realistic agent should take into account the type and number of
resources it needs to perform an action and also if the resource is consumable
or reusable.

Typically this problems are addressed by first **plan** and then **schedule**:

- In the **planning phase** actions are selected, with some ordering
  constraints, to meet the goals of the problem.
- In the **scheduling phase** temporal information is added to the plan to
  ensure that it meets resource and deadline constraints.

This approach is common in real world manufacturing and logistical settings.

In order to solve the scheduling problem we can define a **partial order** plan
that is a path in a directed graph that allows multiple actions to be taken in
parallel from initial to goal state.

The **critical path** is the the one with the longest total duration and
determines the duration of the entire plan.

Actions that are off the critical path typically have a _window_ of time, called
**slack**, in which they can be executed. For this type of scheduling greedy
heuristics like _minimum slack heuristic_ work quite well.

## References

- [[artificial_intelligence_fundamentals]]
- [[logical_agents]]
- [[first_order_logical_agents]]
- [[work_span]]
