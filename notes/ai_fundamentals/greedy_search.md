---
id: greedy_search
aliases: []
tags:
  - ai_fundamentals
  - master
---

# Greedy Search

The **greedy search** expands the most desirable node, based only on the
heuristic value. It does not take into account the cost of edges, the only goal
is to minimize the distance from the current state to the goal.

Its completeness depends on _repeated state checking_: if not performed it can
get stuck in loops; viceversa it is a complete option in a finite state space.

It's not optimal because it does not take into account the cost of the
transition to the next state.

It has $O(b^m)$ complexity in both time and space because it keeps all the nodes
in memory but a good heuristic can improve dramatically the execution time.

## References

- [[informed_search]]
