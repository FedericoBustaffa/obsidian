---
id: decision_tree
aliases: []
tags:
  - master
  - machine_learning
---

# Decision Tree

The **decision tree** model (**DT**) can be used for both classification and
regression tasks; however its structure suggests a propensity for
classification.

Their intrinsic tree representation also makes them easily interpretable,
providing critical points where their classification changes; that's why they
are also widely used in Explainable AI.

More formally we can see decision trees as a disjunction of conjunctions, or a
chain of _if-then-else_ rules. This is because the decision tree _splits_ the
feature space at each iteration, considering one feature/dimension at a time.

![Decision Tree Regions|600](/files/decision_tree_regions.png)

A general decision tree algorithm is the following

1. Select the **best feature** to perform a split.
2. Split the dataset in as many subsets as the number of possible values for the
   selected feature.
3. Repeat until all the examples are correctly classified or there are no
   examples left.

In this algorithm is mentioned _the best feature_, but the way to choose the
best feature can discriminate between different possible DT algorithms.

## Entropy

The first type of DT algorithm (like ID3) uses **entropy** to measure the
**impurity** of a subset of examples. This is a measure, going from $0$ to $1$,
of how much the set is _bad_ classified.

In other words a set of patterns with half of the patterns classified as $-1$
and the other hald as $+1$ has maximum entropy (high impurity), while a set with
all the examples classified as $+1$ has $0$ entropy (low impurity).

In order to compute the entropy of a set of examples $S$ we can use this
formula:

$$E(S) = -p_+ \log_2(p_+) - p_- \log_2(p_-)$$

where $p_+$ and $p_-$ are respectively the proportions of positive and negative
patterns ($0 \leq p_-, \, p_+ \leq 1$).

So for two classes we have that the entropy has a maximum where the proportion
is $0.5$ and minimum when all the patterns are classified the same.

![Entropy|600](/files/entropy.png)

The entropy alone cannot be used to select the best feature for the split
because it doesn't consider any feature in the formula, it's just a measure of
the impurity of the subset.

## Information Gain

## References

- [[supervised_learning]]
- [[random_forest]]
