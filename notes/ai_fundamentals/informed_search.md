---
id: informed_search
aliases: []
tags:
  - ai_fundamentals
  - master
---

# Informed Search

An **informed search** is performed when we have some knowledge on the
_distance_ from the current state to the goal state. This is not part of the
problem definition and it is usually _given_.

This measure of how far we are from the goal state is called **heuristic** and
provides an estimate of the cheapest cost from the state at node $n$ to the
goal.

The most popular category of informed search algorithms are the **best first**,
in which we expand the _most desirable_ unexpanded nodes. In this case how
desirable is a node is defined by the heuristic or some _evaluation function_
that comprehends it.

The main two _best first_ algorithms are

- [[greedy_search]]
- [[a_star_search]]

in which the frontier is now a **weighted queue** sorted by increasing heuristic
value.

If we have two different admissible heuristics $h_1$ and $h_2$, then we can say
that $h_2$ **dominates** $h_1$ if $h_2(n) \geq h_1(n)$ for all $n$.

## Design Heuristics

Designing heuristics is not a mechanical process and it always depends on the
problem.

In general we can try to **relax** the problem and find an exact solution to
that relaxed problem. The cost of transition of the relaxed problem become the
heuristic for the real problem.

To compare heuristic we can measure the **effective branching factor** $b$: the
closer to $1$ the better it is.

Is also possible to _learn_ heuristics because they are just functions, but a
fair amount of data is needed.

## References

Links: [[search_problems]], [[greedy_search]], [[a_star_search]]
