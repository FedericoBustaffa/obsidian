---
id: evolutionary_optimization
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Evolutionary Optimization

The field of **evolutionary optimization** takes inspiration by the darwinian
**biological evolution**, where an environment have some **initial conditions**
and then starts evolving in an iterative process mostly guided by **natural
selection**.

This approach lead to what are called **evolutionary algorithms** that are what
is used to solve typically optimization problems; here why the name
_evolutionary optimization_.

## Biological Evolution

In this process of **natural selection** there are some traits that are more
advantageous for some task and so they will survive, while other traits will
disappear. Usually there is some concept of _generation_ in which a population
lives and whose individuals reproduce, combining their genetic traits, and so
passing them to the next generation.

So the aim is to use most interesting aspects of evolution like

- **Punctuated equilibria**: evolution can happen fast.
- **Adaptation**: a trait favored by natural selection becomes increasingly
  important for a given task.
- **Exaptation**: finding a different use for a trait is previously developed
  for a different use.

These are the main points on which evolutionary optimization will focus.

Another interesting aspect of biological evolution is about **diversity**, which
theoretically leads to a population of individuals good in some task but
genetically different.

Usually in nature this is achieved by the combination of

- **Mutation**: random changes in genes.
- **Recombination**: genetic shuffling of genetic material from different
  individuals.
- **Natural selection**: survival of the fittest.

This is in general a _divergent process_ whose aim is to create novelty.

## Digital Evolution

So in order to have an evolution effect to solve some problem we need to
simulate _initial conditions_ of an environment and the _biological evolutionary
algorithm_ the powers the evolution.

The skeleton of a generic **evolutionary algorithm** is the following:

1. **Initialize** a population of individuals with $N$ features.
2. **Evaluate** the _fitness_ of the population.
3. For each step (generational step):
   - **Select parents** based on fitness.
   - **Reproduction**: generate offsprings by recombination of features.
   - **Mutation**: random little variations on the features.
   - **Evaluate** the fitness of new population.
   - **Survival**: select a subset of $K$ individuals to keep.

Of course there are many details and variants for these algorithms that can
produce different effects and solutions.

The same structure have been used to develop two types of evolutionary
algorithms, but specialized for different purposes:

- **Evolutionary strategies**: _distribution-based_ algorithms, generally for
  continuous optimization problems, in which the main operation is the mutation.
- **Genetic algorithms**: _population-based_ algorithms were there is
  representation of a genome for the features, the mutation is rare and
  recombination is central.

These algorithms' type of optimization is also called **black-box optimization**
because their main goal is to maximize a _fitness function_ without knowing
anything on the function's properties or shapes, so there is no such thing as
gradient descent or in general _derivative optimization_.

We can see the process as a random sampling process guided by an heuristic
function.

## References

- [[artificial_intelligence_fundamentals]]
- [[evolutionary_strategies]]
- [[genetic_algorithms]]
- [[novelty]]
