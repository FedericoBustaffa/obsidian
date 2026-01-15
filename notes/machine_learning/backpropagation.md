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

The key idea to understand backpropagation is that we want to know how much
every unit contributed to the final output and so to the final error.

## Base Case

In order to understand the backpropagation is sufficient to build a very simple
network with one neuron per layer and consider only one training example.
Scaling the computation to general case is pretty straightforward.

![Linear Neural Network](/files/linear_nn.svg)

Assuming $y$ is the target value, is possible to compute the error (MSE in this
case) between $y$ and $o_k$, that is the network's output for input $x$.

$$E(w) = (o_k - y)^2$$

The only contribution to the final error is given by $o_k$ and so it must be
modified to decrease the error. In order to minimize the error it's necessary to
compute the gradient of the error function with respect to $o_k$.

$$\frac{\partial E}{\partial o_k} = 2 \cdot (o_k - y)$$

Of course is not possible to modify $o_k$ directly, it's necessary to unpack it
until we have some free parameters to work on.

$$o_k = \sigma (\text{net}_k)$$

where $\sigma$ is a generic activation function and $\text{net}_k$ is the input
of the node at layer $k$. So now is possible to see how much $\text{net}_k$
contributes to the error by computing the following partial derivative

$$\frac{\partial o_k}{\partial \text{net}_k} = \sigma' (\text{net}_k)$$

but again $\text{net}_k$ cannot be modified directly; it's necessary to unpack
it again

$$\text{net}_k = w_k \cdot o_j + b_k$$

where $w_k$ and $b_k$ are respectively weights and bias of layer $k$ and $o_j$
is the output of layer $j$ (the previous). What is important is that $w_k$ and
$b_k$ now are actual scalar we can directly access and optimize. For now we
consider $o_j$ like a constant.

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

that in more compact way can be written as

$$
\delta_k = \frac{\partial E}{\partial o_k} \cdot
\frac{\partial o_k}{\partial \text{net}_k} \qquad
o_j = \frac{\partial \text{net}_k}{\partial w_k}
$$

and so

$$\frac{\partial E}{\partial w_k} = \delta_k \cdot o_j$$

The key now is to repeat the same process also for the hidden layer/unit $j$,
updating its free parameters

$$
\frac{\partial E}{\partial w_j} =
\frac{\partial E}{\partial o_j} \cdot
\frac{\partial o_j}{\partial \text{net}_j} \cdot
\frac{\partial \text{net}_j}{\partial w_j}
$$

Again we want to measure the contribution of $o_j$ for the result and so we have
to see how much its weights and bias are involved in order to update them.

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
