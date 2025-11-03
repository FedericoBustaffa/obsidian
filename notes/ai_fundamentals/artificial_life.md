---
id: artificial_life
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Artificial Life

An interesting field, that is a branch of complex systems, is **artificial
life**. In general, in complex systems there are simple parts or components that
interact, leading to an emergent _complex_ behavior.

For _artificial life_ the aim is to study **adaptive complex systems** that
evolve their complexity, leading to an _open-ended_ evolution system that also
_self organize_.

In the field of complex systems we can ask ourselves if they are really complex
or there is some regularity. An example is given by studying the logistic map
for the following recurrence

$$x_{t+1} = r \cdot x_t \cdot (1 - x_t)$$

that as we can see is pretty simple, but for increasing values of $r$ we observe
very chaotic behaviors and oscillations:

![Logistic Map|400](/files/logistic_map.png)

As we can see, from values between $2.4$ and $3.0$ we have a straight line,
meaning that the system converges into an equilibrium state. For $3.0$ to $3.4$
we have a _biforcation_, meaning that the system oscillates between two states.
For $r = 4.0$ we have that the system oscillates in basically every possible
state between $0.0$ and $1.0$, leading to a chaotic behavior that is not more
predictable.

## References

- [[artificial_intelligence_fundamentals]]
- [[complex_systems]]
