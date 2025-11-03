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

A technical aspect to take into account is boundary management: cells on the
grid boundary don't have the same number of neighbors as central cells. So often
a _toroidal_ structure is used for the grid that wraps cells at boundary
together, creating a circular structure.

![Toroidal Grid|300](/files/toroidal_grid.png)

The simplest grid we can think about is a $1$-dimensional grid that, is
basically a circular array. Each cell can have $0$ or $1$ value and we define an
update rule of a certain cell, based on its value and the values of the previous
and next cells.

$$s_{t+1}^j = f \left( s_t^{j-1}, s_t^t, s_t^{j+1} \right)$$

An example of rule can be

$$
\frac{1 1 1}{0} \quad
\frac{1 1 0}{1} \quad
\frac{1 0 1}{0} \quad
\frac{1 0 0}{1} \quad
\frac{0 1 1}{1} \quad
\frac{0 1 0}{0} \quad
\frac{0 0 1}{1} \quad
\frac{0 0 0}{0}
$$

that can be expressed as rule $01011010$ (denominators) or as by its decimal
form: rule $90$.

Rules should have a initial state that is mapped to $0$ and have a reflection
symmetry property that ensures that we have the same behavior regardless
direction.

If the rule is too basic we will obtain only replication and reproduction of the
initial pattern

![Simple Cellular Automata Rule|400](/files/simple_ca.png)

But for some more complex rule we can gain complexity and observe structures
like **fractals** and self similarity at different scales

![Complex Cellular Automata Rule|400](/files/complex_ca.png)

We can even start from an initial noisy state and observe how the system evolve
into an ordered system with self organization properties.

Trying different rules can lead to very different behaviors and systems, so we
can classify rules in four main categories:

- **Type 1**: evolution to static state.
- **Type 2**: evolution to periodic structure.
- **Type 3**: evolution to chaotic patterns that lead to pseudo-randomness.
- **Type 4**: evolution to complex patterns that are on the edge of chaos.

Typically _type 4_ rules are the most interesting.

Cellular automatas can have **universal computation** properties (universal
Turing machines). The computational power is defined by the rule; not every rule
leads to a universal computation CA. One example of rule that leads to
universal turing machine is the rule $110$ that has been proven to have this
property, and in fact is a _type 4_ rule. In general, too simple rules cannot
exhibit enough complexity to compute all functions, while too complex rules only
lead to chaos.

Of course the simplest formulation of cellular automata can be to expensive to
implement because, in order to compute an arbitrarily complex function, we need
a composition of very basic operations. So many more complex variations of CA
can be implemented:

- **Non uniform**: different rules for different cells.
- **Non local**: different neighbors structure in order to build something that
  is like an automata network.
- **Asynchronous update**: every cell is updated on the fly, so the next cell is
  updated based on previously updated cell.
- **Continuous time/space**.

### Game of Life

One of the most popular examples of cellular automata is **game of life**, which
is defined by a $2$-D grid with two values per cell: $0$ for dead, $1$ for
alive. Each cell has 8 neighbors (excluding itself) and the update rule is
$2,3/3$ which encode the life/death policy

- If an alive cell touches 2 or 3 alive cells then it stays alive, otherwise it
  dies (solitude or overpopulation).
- If a dead cell touches 3 alive cells it comes back to life.

With this simple system and rule life-like structures already emerge, for
example local attractors.

The game of life can be extended by adding some evolutionary approach, for
example genetic algorithms to perform novelty or evolvability search.

It's also possible to extend the CA by defining global and local rules,
extending the neighborhood, and move to a continuous space time automata.

### Lenia

One of the most famouse extensions of the game of life CA is **Lenia**, that now
add multiple states for every cell, and extends the neighborhood with a sort of
radial function, all with continuous time, space and state.

The first core ingredient for continuous Lenia to work is the **potential
distribution**, a radial function $K$ (called kernel) that defines how much a
cell is influenced by its neighbors. This does not update the cell yet but gives
a set of weights.

The other important thing is the **growth distribution**, that instead describes
how the cell change based on its _potential_ previously computed. Typically this
is a sort of Gaussian distribution that maps the potential to a continuos space
between $0$ and $1$. The Gaussian leads to a behavior that penalizes too tiny or
too large potentials, while instead let _ideal_ potential cells grow.

## References

- Artificial Intelligence Fundamentals
  - [[artificial_intelligence_fundamentals]]
  - [[genetic_algorithms]]
  - [[novelty]]

- Complex Systems
  - [[complex_systems]]
