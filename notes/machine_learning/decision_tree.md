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

$$\text{Entropy}(S) = -p_+ \log_2(p_+) - p_- \log_2(p_-)$$

where $p_+$ and $p_-$ are respectively the proportions of positive and negative
patterns ($0 \leq p_-, \, p_+ \leq 1$).

So for two classes we have that the entropy has a maximum where the proportion
is $0.5$ and minimum when all the patterns are classified the same.

![Entropy|600](/files/entropy.png)

The entropy alone cannot be used to select the best feature for the split
because it doesn't consider any feature in the formula, it's just a measure of
the impurity of the subset.

## Information Gain

In order to select the best feature for the split we need a measure of how much
splitting the dataset with respect to every feature lowers the entropy. This
measure is called **information gain** and measures the expected reduction in
entropy caused by partitioning the examples on a feature:

$$
\text{Gain}(S, A) = \text{Entropy}(S) -
\sum_{v \in \text{Values}(A)} \frac{\#(Sv)}{\#(S)} \cdot E(Sv)
$$

where

- $\text{Values}(A)$ are all the possible values for the feature $A$ present in
  the subset $S$.
- $\#(S)$ is the cardinality of the set $S$.
- $Sv$ is a subset of $S$ for which the feature $A$ has value $v$.

The higher the information gain the more effective the feature in classifying
training data will be.

![Gain Split|600](/files/dt_gain.png)

The information gain is computed for all the features and the one the highest
gain is selected as the first decision node. When we reach a point where the
entropy is zero for all the branches, it means that the DT classified correctly
every instance.

### Gain Ratio

The problem with information gain is that features with many possible values
(e.g. real numbers) will be selected more often.

Let's say for example that a feature $A$ is a real number and, in the given
dataset, we have $l$ patterns, each with a different values for $A$. For that
features the DT will create $l$ possible branches, each with one pattern;
meaning in a $0$ entropy subset.

This of course perfectly fits the data but does not generalize at all, leading
to overfitting.

In order to generate more informative and general subsets the **gain ratio** is
typically implemented as follows

$$
\text{GainRatio}(S, A) =
\frac{\text{Gain}(S, A)}{\text{SplitInformation}(S, A)}
$$

where $\text{SplitInformation}(S, A)$ is a measure of entropy $S$ with respect
to the values of $A$ and it is defined as

$$
\text{SplitInformation}(S, A) = - \sum_{i=1}^C \frac{\#(S_i)}{\#(S)}
\cdot \log_2 \left( \frac{\#(S_i)}{\#(S)} \right)
$$

where $S_i$ are the sets obtained by partitioning on value $v_i$ of $A$, up to
$C$ values.

The gain ratio penalizes features that split examples in many small classes.

However there are cases were the $\text{SplitInformation}$ can be zero or very
small when $\#(S_i) \approx \#(S)$ for some value $i$. An extreme case is when
there is a feature with the same value for all the examples.

To mitigate this is possible to apply some heuristics, for example a very simple
one consists in compute the gain for each feature and then apply the gain ratio
only for features with gain above average.

## Problems

If we keep the tree train, soon or later it will classify correctly all the
examples, but it will fall inevitably in overfitting.

## References

- [[supervised_learning]]
- [[random_forest]]
