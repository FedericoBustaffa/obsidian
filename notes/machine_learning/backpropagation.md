---
id: backpropagation
aliases: []
tags:
  - master
  - machine_learning
---

# Backpropagation

The **backpropagation** is basically the most important algorithm in deep
learning, because let a neural network learn through multiple layers.

<!-- A common misconception (at least for me it was) is that there is an information -->
<!-- flow from the input layer towards the ouput layer and then back. But new -->
<!-- information flows only forward: when the input goes through every layer and -->
<!-- arrives to the last, there is a comparison with the pattern $p$ correct answer -->
<!-- and an error is computed. Then the algorithm change the weights layer by layer -->
<!-- taking into account the error, but the only new information is the error, that -->
<!-- once computed remains constant. -->

## Base Case

In order to understand the backpropagation is sufficient to build a very simple
network with one neuron per layer and consider only one training example.

![Linear Neural Network](/files/linear_nn.svg)

In this way the notation is simpler to and is also clearer how every piece
contributes. Scale the computation to general case is pretty straightforward and
it only needs to add sums and means instead of single values.

The network at the end produces $o_k$ that is our guess on the input $x$.
Assuming $y$ is the correct answer, we can now compute the error

$$E(w) = (o_k - y)^2$$

in order to minimize it we compute the gradient with respect to our output,
because we want modify $o_k$ in order to reduce the total error

$$\frac{\partial E}{\partial o_k} = 2 \cdot (o_k - y)$$

but in order to make the output change we cannot directly modify $o_k$ and so we
have to unpack it

$$o_k = \sigma (\text{net}_k)$$

where $\sigma$ is a generic sigmoidal activation function and $\text{net}_k$ is
the input of the node $k$

$$\text{net}_k = w_k \cdot o_j + b_k$$

and so we obtain

$$\frac{\partial o_k}{\partial \text{net}_k} = \sigma' (\text{net}_k)$$

So now we have to compute the gradient of $\text{net}_k$ in order to have access
to $w_k$ and $b_k$ that are the free parameters we want to modify.

$$
\frac{\partial \text{net}_k}{\partial w_k} = o_j \quad
\frac{\partial \text{net}_k}{\partial b_k} = 1
$$

Putting everything together gives us the gradient of $E$ with respect to $w_k$
and $b_k$ that, for the **chain rule** of derivatives are

$$
\frac{\partial E}{\partial w_k} =
\frac{\partial E}{\partial o_k} \cdot
\frac{\partial o_k}{\partial \text{net}_k} \cdot
\frac{\partial \text{net}_k}{\partial w_k}
$$

and

$$
\frac{\partial E}{\partial b_k} =
\frac{\partial E}{\partial o_k} \cdot
\frac{\partial o_k}{\partial \text{net}_k} \cdot
\frac{\partial \text{net}_k}{\partial b_k}
$$

so now we can update $w_k$ and $b_k$ applying the gradient descent learning
rule.

To update also $w_j$ and $b_j$ we apply this exact formula but for the node $j$:

$$
\frac{\partial E}{\partial w_j} =
\frac{\partial E}{\partial o_j} \cdot
\frac{\partial o_j}{\partial \text{net}_j} \cdot
\frac{\partial \text{net}_j}{\partial w_j}
$$

and this process is repeated until the first hidden layer is updated.

## General Case

In order to apply the backpropagation in a classic multi-layer perceptron
topology not much changes.

![Multi-Layer Perceptron](/files/mlp.png)

In this case we have the same exact formula but what truly changes is the
$\text{net}_k$ function for a generic unit/layer $k$, because now the unit $k$
receives in input all the outputs of the previous layers:

$$\text{net}_k = \sum_j w_{kj} \cdot o_j$$

and so we have to update every $w_{kj}$ for a single unit $k$ (and also its bias
$b_k$), but for updating a single weight we can now reuse the previous formula
for a single input unit.

## Common Notation

A common notation that is used for the backpropagation is the following

$$
\Delta_p w_{tu} = \delta_t \cdot o_u =
-\frac{\partial E}{\partial o_t} \cdot
\frac{\partial o_t}{\partial \text{net}_t} \cdot
\frac{\partial \text{net}_t}{\partial w_{tu}}
$$

for two generic units $t$ and $u$ connected via $w_{tu}$, with

$$o_u = \frac{\partial \text{net}_t}{\partial w_{tu}}$$

that is the output of unit $u$ that goes into unit $t$, and

$$
\delta_t = -\frac{\partial E}{\partial o_t} \cdot
\frac{\partial o_t}{\partial \text{net}_t}
$$

that for an output node $k$ is

$$\delta_k = (o_k - y) \cdot \sigma' (\text{net}_k)$$

while for an hidden node $j$ is

$$
\delta_j = \left( \sum_{k=1}^K \delta_k \cdot w_{kj} \right) \cdot
\sigma' (\text{net}_j)
$$

that is the local error of unit $t$.

## References

- [[neural_networks]]
- [[neural_networks_training]]
