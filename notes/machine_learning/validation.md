---
id: validation
aliases: []
tags:
  - master
  - machine_learning
---

# Validation

A very important step for the deployment of reliable machine learning models is
the **validation** phase, in which the model is _tested_ on unseen data in order to

- Estimate the prediction error and generalization capability: can be done
  analytically or by resampling.
- Tune hyper-parameters

The aim of this is to find a nice trade off between training and validation (or
test). In other words the the main purpose of training is not to perfectly fit
the training data, but to find patterns in order to generalize unseen data.

So the training error is not a good metric to measure the model performance,
because a low training error could be an hint that the model is in
**over-fitting**. Also an high error in the training is not always a proof of
under-fitting because maybe we're dealing with extremly noisy data.

In order to understand better the validation issue, a useful framework is
provided by the statistical learning theory, that introduces the concept of
_bias-variance_ decomposition.

So in practice the validation has two phases with two different aims, each done
after training:

- **Model selection**: estimation of the model performance (generalization
  capability) of different learning models in order to choose the best one.
  This also includes the search of the best hyper-parameters of one specific
  model.
- **Model assessment**: once chosen a model from the previous phase there is an
  evaluation of its prediction risk on new unseen _test_ data.

Model selection returns a model, while model assessment returns an estimation
value.

The two phases must be separate and not mix together in order to have true
measures of the model capabilities. The usage of test results to make model
selection introduces a bias that can lead to situation where the model is
_trained_ on the test. So it will be good in the test we have but maybe but in
real world unseen application and scenarios.

## Grid Search

The first phase of the model selection phase we are interested in finding the
best values for the hyper-parameters of the model. One popular algorithm that
does that is the **grid search**.

The simplest version is an **exhaustive search** where we put candidate
hyper-parameters values in multi-dimensional grid, then we basically train the
model with every possible combination to get the one that have the best score on
the validation set.

This can be expensive but easily parallelizable because each combination of
hyper-parameter lead to an independent result with respect to the others.

### Nested Grid Search

The cost of an exhaustive search can be high depending on the number of
hyper-parameters and on the number of values for each of them

$$O\left(\# (\text{values})^{\# \text{parameters}} \right)$$

A useful trick to speed up the computation is to fix some hyper-parameters
values in a preliminary experimental phase in order to identify which one is not
relevant.

A more robust technique is the **nested grid search**, where we search for good
values ranges by performing first _coarse grid search_ on some values. In this
way is possible to identify regions of interesting values to explore and other
regions to discard.

We can repeat this corse grained search until we identify a good region that is
also small enough to perform a _fine grid search_, that is basically an
exhaustive search on reduced search space.

## Hold Out

Once the grid search produced a candidate set of hyper-parameters the next step
is to evaluate the resulting model.

The simplest technique for validation is the **hold out**, that split the
dataset in three parts: training, validation and test (often referred with TR,
VL and TS respectively).

![Hold Out|700](/files/hold_out.png)

The validation set purpose is model selection and, jointly with TR, are often
called development set; the test set is only used for model assessment.

Once the grid search produce a set of hyper-parameter, the model is trained on
TR with them, then is evaluated with the validation set in order to get a score.

This process is repeat for each set of hyper-parameters produced by the grid
search. Once all the models are evaluated we choose the one the best score on
the validation set.

The last step of the pipeline is to estimate the generalization capability of
the model on TS, obtaining another score.

Satisfied or not, this is the best we can get but a **re-training** is always
possible because having more data is always better and leads to and implicit
regularization of the model (better generalization).

## Cross Validation

The **cross validation** (**CV**) is a way to exploit better all the data at our
disposal, in order to have more robust and reliable estimations on the model
performances.

The basic idea is to perform training, validation and test multiple times but
every time partitioning the dataset in different ways.

This is more robust at particular dataset partitioning because now the model is
selected on the base of multiple training and the validation scores average. The
same holds for model assessment: we can now provide an average risk estimation.

### K-Fold

The problem with hold out is that we have to _guess_ well the partition
dimensions and also we have to ensure that every partition is a small
representation of the total distribution of the dataset, in order don't be
sensible to particular partitioning.

The **K-fold**, that is an implementation of cross validation try to address
these problems by partitioning the dataset in $k$ mutually exclusive subsets:
$D_1, \dots, D_k$.

![4-Fold Cross Validation|250](/files/k-fold.png)

Iteratively the model is trained on the training partitions and validated or
tested on the remaining one. In this way we produce $k$ estimations of which can
calculate the average to use for model selection or model assessment.

## Dataset Partitioning

Another possible problem comes from the fact that the model training, selection
and test results can be sensible to particular partitioning of examples. In
other words if the test set is full of outlier, while training and validation
are not, the test result will be bad but it's not a good estimation.

In order to not be sensible to particular data partitioning there are some
procedure to better partion the data in the first place.

- **Stratification**: group members of the population into relatively
  homogeneous subgroups begore sampling
- **Classification**: for each partition each class is represented in
  approximately the same proportions as in the full dataset.

Also a repeated hold out with different sampling can bring the partitioning
closer to an _expected value_ that is more reliable.

## References

- [[machine_learning]]
- [[statistical_learning_theory]]
