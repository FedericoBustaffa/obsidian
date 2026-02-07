---
id: games
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Search in Games

Intelligent agents playing **games** is another field where **search
algorithms** are commonly used. Of course the paradigm is a little bit different
because this time

- Two or more agents playing against each other; so this is an **adversarial
  search** where the agent wants to make the move the maximize its chance to win.
- The two players play in **turns**.
- The two players play a **zero-sum** game.
- A **fully observable** and **deterministic** game.

Again we have a state space in which each state $s$ contains some informations
like

- `TO-MOVE(s)`: player to move in the current state.
- `ACTIONS(s)`: legal moves from the current state.
- `RESULT(s, a)`: transition model, resulting in a new state.
- `IS-TERMINAL(s)`: is the game over or not.
- `UTILITY(s, p)`: **utility** function gained on a **terminal** state $s$ by
  player $p$.

The _utility_ value is one of the most important thing listed because is like a
_reward_ value on which the search is based.

## Game Tree

Usually in games context there are two players MIN and MAX; MAX plays first and
the _utility_ value is measured from MAX's perspective.

The problem is usually formalized as a **game tree** whose root is the game
initial state, in which MAX can make a move; the second level is composed by all
the possible moves MAX can do starting from the root, and the third level is
composed by all the possible moves MIN can do after MAX's move.

So in this sense, starting from the root, with MAX moving first, every level
alternates MAX and MIN plays. The tree has of course all the properties of a
normal search tree and usually for reasonable sized games is not possible to
build a complete game tree.

Usually for finite trees, the **leaves** represent the **terminal states**, to
which a utility value is associated.

## Minimax

The strategy for each player is a **conditional plan** because they reason in
terms of what should MAX do if MIN does a particular move.

In games where the end state can have multiple values like win, lose, draw or
points, the **minimax** algorithm make MAX play always the optimal move,
assuming that also MIN plays optimally.

In order to do that the minimax algorithm

1. Compute the **minimax values** for the **leaves**, that, in this case are the
   _utility values_.
2. Compute the **minimax values** for the **intermediate nodes**, that are
   chosen differently depending on whose turn it is:
   - It's the **maximum** among the minimax values of next level if it's MAX's
     turn.
     $$\max_{a \in \text{ACTIONS}(s)} \text{MINIMAX}(\text{RESULT}(s, a))$$
   - It's the **minimum** among the minimax values of next level if it's MIN's
     turn.
     $$\min_{a \in \text{ACTIONS}(s)} \text{MINIMAX}(\text{RESULT}(s, a))$$

Now from the root it's possible to perform a complete depth-first search to

1. Find all the terminal states.
2. Get the utility of the leaves.
3. Backtrack to compute minimax value of all intermediate nodes
   - Take the minimum value among the children if the parent is a MIN node,
     take the maximum if it is a MAX node.

The Minimax algorithm is complete on finite trees and is optimal if MIN actually
plays optimally. Anyway it has $O(b^m)$ time complexity and $O(bm)$ space
complexity due to DFS.

### Suboptimal Opponents

In case of **suboptimal opponents** the minimax algorithm can encounter some
inconvenience. In fact, there are many games or scenarios where if both players
always play optimal, MAX can't win.

The trick some algorithm does is to play suboptimal moves, in order to narrow
the path to the victory, _hiding_ it from an optimal opponent's perspective.

In other words we can say that the assumption of minimax make the algorithm lose
some moves derived from an opponent's error, giving an easier win.

### $\alpha$-$\beta$ pruning

In order to mainly reduce time complexity, there is a slight improvement of
Minimax algorithm, called **$\alpha$-$\beta$ pruning**, which aim to _prune_
branches that are not leading to any best moves for either MAX or MIN. In
particular with

- $\alpha$ is the **largest** minimax value so far.
- $\beta$ is the **smallest** minimax value so far.

In general when we consider a node with a minimax value $V$, if a node of the
same depth or a higher-up node have a better minimax value, it's not possible to
reach the node with $V$ minimax value, and so it can be _pruned_.

The pruning process does not affect the solution but can reduce time complexity
to $O(b^{m/2})$. It's still quite high and also, it can be obtained only if the
pruning is done as soon as possible.

### Expected Minimax

A possible extension of Minimax for stochastic games is the so called **expected
minimax** which uses a similar tree as before but that now contains also
**chance nodes** representing all possible outcomes.

In this version there is the need to handle chance node by assigning them the
_expected value_ of all possible outcomes of the node.

$$
\text{ExpectedMINIMAX}(s) =
\sum_r P(r) \text{ExpectedMINIMAX}(\text{Result}(s, r))
$$

## Monte-Carlo Tree Search

An alternative to minimax is the **Monte-Carlo tree search** (**MCTS**), that
uses random simulations in order to understand which moves are the best,
building a probabilistic model based on how many times a certain move bringed to
a win.

The MCTS introduces new concepts in order to work well:

- **Rollout/Playout**: from a given state, play out an entire episode until the
  end and get the utility.
- **Playout policy**: a function that decides which move to take at each step.
- **Pure Monte-Carlo Search**: given a state, compute its value by performing
  $N$ playouts without a the need of a search tree.

In MCTS the value of a state is the **average final utility** over a number of
rollouts from that state.

The MCTS method starts with a tree with only one node (the initial state) and
follows these steps:

1. **Selection**: From the root node, use the _selection policy_ to select a
   path down to a leaf.
2. **Expansion**: use the playout policy to generate one new node from the
   current leaf.
3. **Simulation**: perform one rollout from the newly expanded node using the
   playout policy.
4. **Backpropagation**: update the value of the nodes in the search tree from
   the newly added node up to the root, following the selection path.

![MCTS|600](/files/mcts.png)

Return in the end the move with the _highest number of play-outs_.

### Selection Policy

A crucial component of the MCTS is the **selection policy**, which tells which
is the move to follow from a given search tree. What the selection policy aims
to do is _balance_ two values: **exploitation** and **exploration**.

The first one prioritizes moves that we know they are good and well explored
from many previous simulations. The other one prioritizes moves from less
explored states.

In order to balance these two values the right way there is the need to
introduce the concept **upper confidence bounds applied to trees** (**UCT**).
This is a tool that dynamically updates the two values following the idea that

- In the beginning it's worth exploring as much as possible in order to discover
  new paths.
- A "middle" phase has some kind of equilibrium between exploitation and
  exploration.
- After a while we want to stop exploring and follow only the most promising paths.

This can be done by computing the **upper confidence bound** (**UCB1**) for each
node, during the MCTS:

$$
\text{UCB1} (n) = \frac{U(n)}{N(n)} +
C \cdot \sqrt{\frac{\log{N (\text{Parent}(n))}}{N(n)}}
$$

where

- $N(n)$ is the number of playouts through node $n$.
- $U(n)$ is the total utility (sum) of playouts through node $n$.
- $C$ is a constant the balances exploration and exploitation.

And so we can compute the **exploitation term** by calculating the **average
utility**

$$\frac{U(n)}{N(n)}$$

and the **exploration term** by computing

$$\sqrt{\frac{\log{N (\text{Parent}(n))}}{N(n)}}$$

that is high when $N(n)$ is low and so for rarely explored nodes. As we can see,
this second term produces the wanted behavior of progressively decreasing
exploration value and this is because

- $N(\text{Parent}(n)) \geq N(n)$ as every time $n$ is visited
  $\text{Parent}(n)$ is visited as well.
- $\log{(N(\text{Parent}(n)))} \leq N(n)$ from a certain number of rollouts on.

![Exploration Functions|500](mcts_exploration.png)

So, exploration term will go to zero as the number of rollouts increases, in
order to progressively limit exploration.

## References

- [[artificial_intelligence_fundamentals]]
