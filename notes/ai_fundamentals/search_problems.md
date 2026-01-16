---
id: search
aliases: []
tags:
  - ai_fundamentals
  - master
---

# Search

A very used paradigm for intelligent agents is the **search**, where the problem
is modelled as a **graph** (or **tree**) **search**. In this structure every
node represents a state and links that _enter_ a node, typically represent the
cost to move in that state.

There are two main ways for search-based agents to operate, depending on the
environment:

- **Open-loop (offline)**: the agent searches for the sequence of actions before
  perform it. This is possible only with a _fully-observable_ and
  _deterministic_ environment.
- **Closed-loop (online)**: the agent continuously evaluates the feedback from
  the environment. This is typically done with _partially observable_ and
  _stochastic_ environments.

There are many possible environments that will lead to different strategies, so
it becomes crucial to know which kind of environment the agent is working.

Another variable to take into account and that can depend on the environment or
can come with some previous knowledge is the type of search the agent will
perform.

## Tree Search

The most common approach for search problem is **tree-search** algorithms; in
general the algorithm start from already visited states and **expand** them,
generating **successor states**. For this kind of algorithm we need

- **Frontier**: set of unexpanded nodes separating expanded nodes from unreached
  nodes.
- **Reached nodes**: a set composed by the frontier and the expanded nodes.

In other words, an _expanded_ node is a node that has been extracted from the
frontier, and on which the _successor function_ has been applied. An
_unexpanded_ node is a _reached_ node but still in the frontier, waiting to be
expanded. An _unreached_ node is a node that is not in the frontier but is in
the graph (or tree) because the problem definition implies it.

Let's also emphasize that a _state_ and a _node_ are not the same thing:

- A _state_ is a representation of a physical configuration.
- A _node_ is a data structure that is part of the _search-tree_, so it
  includes parent, children, depth, path cost etc.

The last piece we need is the **expand function**, that, applied to a node:

- Creates new nodes.
- Fills in the various fields.
- Uses the _successor function_ of the problem to create the corresponding
  states.

> [!important] State space $\neq$ Search Tree
> An important thing to take into account is that the problem's **state space** is
> different from the **search tree**.
>
> This is clear if we remember that a state is different from a node; but we can
> say that a node is a _concrete_ representation of a state in the tree. The
> main difference is that, in the state space, every stati is unique, while in a
> search tree we can have multiple nodes representing the same state.

This is because, depending on the problem and the _search strategy_ we can have
loops that can lead to infinite search.

We already mentioned an _expand_ function, which gives a set of _successor_
nodes; in order to do that it needs a _successor_ function, that takes in input
the problem and the current state and returns a set of nodes and the action
needed to reach that node.

## References

- [[artificial_intelligence_fundamentals]]
- [[intelligent_agents]]
- [[uninformed_search]]
- [[informed_search]]
