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

This does not mean that the system became random: the system is still completely
deterministic. However, its dependecy from the initial state is so strong that a
tiny error in the evaluation of $x_t$ can completely change what we get as
$x_{t+1}$.

In theory, if we have infinite precision and we now the initial conditions, it
would be possible to perfectly predict what $x_{t+1}$ will be. However with
finite precision, after few iterations, the result is completely different from
the real one. This is why we are talking about chaos and apparent randomness,
even in a deterministic system.

## Cellular Automata

A simple mathematical model of _self-replication_ is called **cellular
automata**, that is a system able to produce itself as output. The idea is to
use the system description to replicate it, resembling DNA.

Typically cellular automata are defined in discrete space like a
multidimensional **grid** and the time is also discrete. A cell contains only
one discrete variable, that is updated by some **rule** that dictates the
dynamics based on neighbors' state.

## References

- [[artificial_intelligence_fundamentals]]
- [[complex_systems]]
