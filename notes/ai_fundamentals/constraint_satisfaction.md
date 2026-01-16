---
id: constraint_satisfaction
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Constraint Satisfaction Problem

There are problems where the goal is to reach a state that satisfies some
_constraints_, these are called **constraint satisfaction problems** (**CSP**)
and in order to solve them, we need to

- Define a **structured state** via **factored representation**. In other words
  we pass from an **atomic** state to a state that has the shape of a vector,
  whose components are in a sense independent.
  $$X = \{ X_1, X_2, \dots, X_n \}$$
- The **domain knowledge** is expressed by a set $C$ of constraints. For example
  $$X_1 \leq 0 \quad X_2 \in [1, 3]$$

A CSP solution so is basically a triple formed by

- $X$: the assignment that satisfies all constraints.
- $D$: the domain space of $X$.
- $C$: the set of constraints.

Let's also define some type of assignment or solution

- **Complete** if all variables have been assigned a value. It is called
  **partial** if only a few variables have been assigned.
- **Consistent** if the assigned variables satisfy the constraints.
- **Partial solution** is a partial assignment that is consistent.

Of course the aim is to find a _complete_ and _consistent_ solution.

## Constraints Types

There are different types of constraints, and based on this strategies and
problem complexity can change:

- **Unary**: only one variable is involved.
- **Binary**: two variables are involved.
- **Global**: any number of variables involved.
- **Preference**: soft constraints that represent a preference that will not
  affect the consistency of a solution if not satisfied, but would be better to.
  They are typically modelled by a cost associated with each assignment.

The first three types are called **absolute constraints**.

## Search in CSPs

Like in other fields, we can search in the factored states space, but instead of
expanding nodes, we can exploit the vectorial structure. If we have $n$
variables that can take $d$ possible values, making an assignment that respects
some constraints reduces the search space (**constraint propagation**).

For the initial state we have an **empty** assignment and a branching factor of
$nd$ at the root. We proceed by assigning one of the possible $d$ value to a
variable that does not conflict with the current assignment. The branching
factor at the second level will be $(n-1) d$, and so all the solutions will be
at level $n$ (we have assigned all the variables).

The problem is that the number of leaves is $n! d^n$, but to fix this we can
exploit the problem structure.

### Backtracking

just by fixing the order of assignments, in this way is possible to remove the
$n!$ complexity. This is because, at each step, we no longer choose which
variable assign, but only which value assign to it because the order is
predefined.

This is basically a DFS that recursively assign variable after variable and if
the assignment is not consistent, it fails and try another value from the
domain. If every value bring to non consistent assignment the there is the need
to change the value of the previous variable.

#### Heuristics

With vectorial state is possible to build heuristics that are independent from
the domain and that can speed up the search.

- **Minimum Remaining Value** (**MRV**): choose variable with fewest legal
  values first, in order to fail fast and pruning earlier.
- **Degree**: choose variable with most constraints first.
- **Least Constraining Values**: choose a value assignment that rules out the
  fewest values in the other variables to be assigned. In other words it let
  more options open in order reduce the probability of going back too much.

There are also possible improvements via _inference_,

- **Forward Checking**: assign a value to $X$ and for each constraint $C_{XY}$,
  remove values from $D_Y$ that break constraints. If there is no value left we
  backtrack
- **Arc-Consistency**: a variable $X$ is **arc-consistent** if and only if for
  every constraint $C_{XY}$, and for all possible values $x \in D_X$, there
  exists a value $y \in D_Y$ such that $y$ satisfies the constraint $C_{XY}$.

  In other words, every possible value of $X$ is supported by at least one
  compatible value in $D_Y$. The opposite must be checked separately. If a value
  in $D_X$ does not have at least one value in $D_Y$ that satisfies the
  constraint $C_{XY}$, it can be removed from the $D_X$.

  It's possible to repeat the process until we remain only with variables that
  are _arc-consistent_ and so, with a reduced search space.

these techniques can be used before and/or during the search in order to reduce
the space of possible assignments.

### Local Search in CSPs

Local search algorithms for CSPs does not partially assign values, they in fact
consider a **complete state formulation** where each state assign a value to
every variables.

So we typically start from a random assignment of all variables and the search
changes only one value at a time.

- **Min Conflicts**: choose the value that breaks the least constraints.

Even though there are many tricks, CSP is still difficult, in fact the worst
case have $d^n$ leaves anyway.

### Exploiting Problem Structure

There are cases where we need to reason on the problem structure in order to be
able to optimize.

For example in a **graph-structure** CSPs, we can compute the **connected
components** in order to be able to solve subproblems, whose union will bring
the main problem solution.

For **tree-structured** CSPs the cost is $O(nd^2)$ if we use the following
algorithm:

1. **Topological sort**: choose any variable as root and order variables such
   that each one is a child of its parents.
2. Make the tree arc-consistence, return failure otherwise.
3. Assign to each node any value consistent with its parents, or fail.

This specific structure let us never backtrack. Is also possible to **force a
CSP in a tree structure** by

1. Assign a variable to a node.
2. Make arcs consistence.
3. Remove that variable.

If the resulting graph can be made a tree, it's possible to check quicky if the
CSP is solvable, otherwise is possible to try out another variable assignment or
node selection.

## Questions

- Whats is an _AllDiff_?
- Graphs and hyper-graphs.

## References

- [[artificial_intelligence_fundamentals]]
