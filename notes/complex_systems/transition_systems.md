---
id: transition_systems
aliases: []
tags:
  - complex_systems
  - master
---

# Transition Systems

Transition systems can be seen as some kind of automata which aim to study
things like _reachability_ of a given _state_. This because we cannot deduce if
some state could be reached only by running simulations.

We can see the transition systems as **models of behavior** (like ODEs) and they
can be analysed by running stochastic simulations or through _model checking_.

> [!note] Transition System
> A transition system is pair $(S, \to)$ where
>
> - $S$ is a set of **states**.
> - $\to \subseteq S \times S$ is the **transition relation**.

With $s \to s'$ we can denote the transition from state $s$ to state $s'$; we
can also use the notation $(s, s')$ with $(s, s') \in S$. The notation $s
\not\to$ denote instead that there is no $s'$ in $S$ such that $s \to s'$.

The transition system is essentially a graph used to model the behavior of a
system. Lets notice that the set of states can be infinite but typically
enumerable. Lets also notice that a transition describe the system state changes
and from a given state, we can have multiple possible new states, capturing
_non-deterministic_ behaviors.

## Traces

Usually in a transition system a state $s_0$ is chosen as _initial state_ and a
possible behavior starting from $s_0$ is called a **trace**.

> [!note] Trace
> A trace $t$ of a transition system $(S, \to)$ with initial state
> $s_0$, is a sequence of states $t=s_0, s_1, \dots$ such that for each
> $s_{i+1}$ with $i \in \mathbb{N}$ in $t$ it holds $s_i \to s_{i+1}$.

Usually the only state $s_0$ is also called **minimal trace** and a generic
trace $t$ is called **maximal** if either $t$ is infinite or there are no
possible transitions from the last state of the sequence to the any other one.

### Reachability

Finally, with traces, we can introduce the notion of _reachability_ of a state.

> [!note] Reachability
> A state $s$ of a transition system $(S, \to)$ with
> initial state $s_0$ is **reachable** (from $s_0$) it there exists $s_1, \dots,
> s_n \in S$ such that $s_1, \dots, s_n, s$ is a trace.

So we basically want to know if there is a path in the graph that can bring from
$s_0$ to $s$.

A particular class of transition systems are **Kripke Structures**, where states
are characterized by a set of **atomic propositions** that can be either true or
false.

> [!note] Kripke Structure
> Given a finite set of atomic propositions $AP$, a Kripke structure $K$ is a
> Transition System $(S, \to)$ where $S = \mathcal{P} (AP)$.

In other words an atomic proposition $a$ is contained in a state if and only if
it is true in that state.

## Transition Systems over a set of variables

Very often the definition of the state of a transition system is based on a set
of variables describing the features of the state.

> [!note] Transition system over a set of variables
> Given a set of variables $X = \{ X_{1}, \dots, X_{n} \}$ and a set of domains
> $\{ D_{1}, \dots, D_{n} \}$ such that $D_i$ is the domain of $X_i$, a
> transition system over $X$ is a transition system $(S, \to)$ with $S = D_1
> \times \cdots \times D_n$.

Each domain should be a recursively enumerable set of values or, better, a
finite set of values. This is because the number of variables impact
significantly on the number of possible states of the transition system,
possibly leading to a _combinatorial explosion_.

### Specifying Transition Systems

A transition system over a set of variables can be _specified_ by giving a set
of **transition rules** having a similar semantic to an `if-then` statement. We
in fact have a **guard**, composed by a conjunction of conditions on the state
variables; and a **update** that is a conjunction of assignments to state
variables.

The idea is that, given $s_1$ and $s_2$, if $s_1$ satisfies the _guard_ $s_2$
can be obtained by applying to $s_1$ the assignments described in the _update_
rule.

## Labeled Transition Systems

We can extend normal transition systems with labels.

> [!note] Labeled Transition Systems
> A labeled transition system (LTS) is a triple $(S,L,\to)$ where
>
> - $S$ is a set of states.
> - $L$ is a set of labels.
> - $\to \subseteq S \times L \times S$ is a labeled transition relation.

This kind of transition systems are usually well suited to model _concurrent
interactive systems_ in compositional way. The idea is to specify the LTS of
each component and combine them by taking synchronizations into account.

The role of transition labels is to describe the the action performed by the
system during the transition and usually the notation for internal, not
synchronized actions is a $\tau$ label; for a potential action the system could
perform by interacting with another component the labels can be arbitrary and
can explain the action.

### Synchronization

Two possible approaches to synchronization are _binary_ and _global_:

- **Binary**: in this case the non-$\tau$ actions are split into two sets
  denoted as $\{a, b, \dots \}$ and $\{\overline{a}, \overline{b}, \dots \}$. The
  idea is that action $a$ has to be performed along with a transition with label
  $\overline{a}$ of another component.
- **Global**: in this case non-$\tau$ actions are synchronized among all system
  components. All components having a transition with label $a$ must perform such
  a transition together.

The synchronization of a number of transitions result in a new $\tau$
transition.

## References

- [[complex_systems]]
