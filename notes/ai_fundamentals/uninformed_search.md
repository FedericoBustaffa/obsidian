---
id: uninformed_search
aliases: []
tags:
  - ai_fundamentals
  - master
---

# Uninformed Search

An **uninformed search** is performed when we don't know the _distance_ from the
current state to the goal state.

In order to perform a search we need a **strategy** that defines how nodes are
expanded and so how much the search will cost, and things like

- **Completeness**: the search always finds a solution if one exists and always
  fails if it does not.
- **Optimality**: the search always find the least cost solution.

We are also interested in **time** and **space complexity** and in this case
they are usually measured in

- **Maximum branching factor** of the tree ($b$).
- **Depth** of the least cost solution ($d$).
- **Maximum depth** of the state space ($m$) that can be infinite.

Strategies for uninformed search use only information available with the
problem definition, so there is no information about how far we are from the
goal state. The most popular strategies for uninformed search are

- **Breadth first search**
- **Uniform cost search**
- **Depth first search**
- **Depth limited search**
- **Iterative deepening search**

Some of these strategies handle naturally the **repeated states** issue that
presents when for example there are cycles in the search graph or there are
redundant paths (differents paths that lead to the same state). In this case,
detection of cycles and redundant paths can prevent a possible exponential
complexity.

## References

Links: [[search_problems]]
