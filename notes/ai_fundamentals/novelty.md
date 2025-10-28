---
id: novelty
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Novelty Evolution

The **novelty evolution** is an evolution paradigm that is not driven by some
task but is driven by the discovering of new interesting behaviors and patterns.

This is a kind of _search without objective_, where we want to build **stepping
stones**, that are interesting building blocks without an immediate identifiable
purpose.

## Novelty Search

So now we have a so called **open-ended evolution** where we want to reproduce
the unbounded innovation of natural evolution. The only objective is to survive
and replicate, and the goal is to always create individuals of greater
complexity and diversity.

For example we can build a population that solve a maze but without a fitness
function that indicates the distance from the exit; individuals just reproduce
with the main goal of exploring the space taking different decisions from the
others.

In general we want to keep an **archive** of novel behaviors because they
represents the stepping stones for futures individuals. A behavior is added to
the archive if it exceed a threshold of diversity from other individuals.

## Quality-Diversity

Novelty search can seem a little too extreme because it seems like a random walk
but that tries to cover as fast as possible the search space.

A good trade off between task based and novelty evolution is
**quality-diversity** evolution. With this approach we want to find a maximally
different set of solutions for the same problem where we move away from already
visited regions.

For this kind of algorithms there are two main components

- **Behavioral characterization** (**BC**): a vector describing the path or the
  actions taken by an individual or any other feature is worth adding.
- **Novelty**: a metric measured by comparing BCs accross individuals.

In general the BC should be _aligned_ with the current task in order to succeed.

### Map Elites

The **map elites** is an example of this QD algorithm where we have a
multidimensional arhive of phenotypic elites, where there are stored the best
individuals of a specific _niche_.

The niche is like a partition of the space that contains a portion of possible
individuals with some behavioral characterization.

![Map Elites|400](/files/map_elites.png)

The method divides the behavioral space in a discrete grid where in each bin the
best individual of the niche is stored. Each dimension of the space is a BC, for
example, for a maze solver we can have on one dimension the euclidean distance
from the exit and for another dimension the amount of _exploration_ done (also
with respect to the past individuals).

The algorithm proceeds by selecting
parents from different grid bins, generating offsprings and evaluate them, based
on task fitness.

The offspring will have a BC that indicates its position in the grid; if its
fitness is better than the current occupant of the bin it will take its place,
otherwise it will be discarded.

### Novelty Search with Local Competition

Another approach to QD is the **NSLC**, that uses novelty search for diversity
and then evaluates the individual's task-based fitness.

The best individuals are chosen based on the percentage of $k$-NN with lower
fitness than individuals.

In this case there is no need to discretize the space because the niches are
adaptively computed with the $k$-NN, but of course similar individuals can
belong to slightly different niches.

## Evolvability

The concept of **evolvability** aim to make individuals that as before aim to
develop new features in order to survive and reproduce, but also to have genomes
that produce _good_ offsprings.

Generally having a static fitness function penalize individuals with large
evolvability, so in order to indirectly promote it we can change the fitness
function over time or use multiple fitness functions.

A more direct approach is **evolvability search** that selects offsprings based
a direct measure of evolvability:

1. Generate fake offsprings, measure their behavior distance with respect to the
   archive and then discard them.
2. Select parents the produces the more diverse offsprings.

Then we can perform an **evolvability test** that measures how good the obtained
population performs in a new environment without further adaptation.

### Evolvability Evolution Strategy

An attempt to scaling evolvability search to large problems is given by
**evolvability evolution strategy**, that uses novelty search for to drive
individual to novel behaviors and evolvability search to drive offsprings to
novel behaviors.

The difference lies in the fact that now we use ES to avoid offsprings
evaluation, use a population distribution and maximize evolvability via gradient
descent.

## References

- [[evolutionary_optimization]]
