---
id: perceptron
aliases: []
tags: []
---

# Perceptron

Behind the idea of the perceptron there was a more formal intuition given by
McCulloch e Pitts, that proposed a logic model for an artifcial neuron for
computing boolean functions.

The idea was more or less to have something that receives binary inputs, has
binary weights, and an **activation function** that, through a weighted sum of
the previous two, determine if the neuron is active or not. This was a
deterministic model **without learning**.

---

The **perceptron** is a (very simplified) mathematical abstraction of
**biological neuron**, and can be seen as the minimal unit of neural networks
(also called **artificial neuron**).

![Perceptron|550](/files/perceptron.png)

Like biological neurons, artificial ones try to reinforce connections through
continuous and repeated stimulations. In the artificial case we have an input
given by feature vectors (training set) and what increase connections relevance
are **weights** that can be modified by learning.

The very first perceptron was built from Frank Rosenblatt; it was an hardware
implementation for image recognition. With respect to what McCulloch and Pitts
achieved, the Rosenblatt's perceptron has real value input and weights and most
important has a **learning rule**.

## Learning Algorithm

This model, differently from linear models, wants to minimize (reduce to $0$ in
practice) the number of misclassified patterns with a line found through a
**learning algorithm**:

1. Initialize the weights.
2. Pick a learning rate $0 < \eta < 1$.
3. For each training pattern $(\mathbf{x}, y)$:
   - Compute the output activation
     $$out = \text{sign} (w^\top x)$$
   - If $out = y$, don't change weights.
   - If $out \neq y$, update the weights by the following learning rule:
     $$w_\text{new} = w + \eta \cdot y \cdot x$$
     or in a different form
     $$w_\text{new} = w + \frac{1}{2} \eta \cdot (y - out) \cdot x$$

The repeat until the stopping condition is satisfied. Using this algorithm the
perceptron always finds the LTU if the problem is linearly separable.

### Convergence Theorem

For linearly separable tasks, the perceptron algorithm converges after a finite
number of steps. So this theorem guarantees to correctly classify all the input
patterns if the problem is linearly separable.

To prove it let's define some quantities and make some assumptions before:

- All patterns $y_i$ are positive ($+1$).
- The problem is linearly separable: $\exists w^*$ (the solution) such that
  $$y_i ((w^*)^\top x_i) \geq \alpha \iff (w^*)^\top (y_i x_i) \geq \alpha$$
  with $\alpha = \min_i y_i ((w^*)^\top x_i) > 0$.
- Define $x_i' = (y_i x_i)$, then $w^*$ is a solution if and only if is a
  solution of $(x_i', +1)$.
- The learning rate is $\eta = 1$.
- The biggest input of the training set $\beta = \max_i \| x_i \|^2$. Useful to
  limit how much the perceptron can misclassify data.
- After $q$ errors (in our case all false negative) we have that the weights are
  $$w(q) = \sum_{j=1}^q x_{(i_j)}$$
  where $x_{(i_j)}$ is the pattern where the $j$-th error was commited. Note
  also that inside the sum there is only $x_{(i_j)}$ because both $\eta$ and $y_i$
  are $1$. We also assume that $w(0) = 0$.

Now we are ready to prove the theorem by exploiting the idea that we can find a
lower and upper bound to $\| w(q) \|^2$ as a function of $q^2$ and $q$ steps. In
this way is possible to find $q$ such that the algorithm converges.

---

Let's find the **lower bound** first by recalling that $w^*$ is the solution and
$\alpha$ is the ideal separation hyperplane. Then

$$(w^*)^\top w(q) = (w^*)^\top \sum_{j=1}^q x_{(i_j)} \geq q \alpha$$

that can be written by exploiting the definition of linearly separable problem.
Now we can use the Cauchy-Swartz theorem:

$$\| w^* \|^2 \| w(q) \|^2 \geq ((w^*)^\top w(q))^2 \geq (q \alpha)^\top$$

from which we derive

$$\| w(q) \|^2 \geq \frac{(q \alpha)^2}{\| w^* \|^2}$$

that is in fact a lower bound expressed as a function of $q^2$.

---

For the **upper bound** instead we can say that

$$
\| w(q) \|^2 = \| w(q - 1) + x_{i_q} \|^2 =
\| w(q-1) \|^2 + 2 w (q - 1) x_{i_q} + \| x_{i_q} \|^2
$$

obtained by exploiting a property of $\| a + b \|^2$. For the $q$-th error the
term $2 w (q - 1) x_{i_q}$ is negative, hence, deleting a negative term will let
us write

$$\| w(q) \|^2 \leq \| w(q-1) \|^2 + \| x_{i_q} \|^2$$

and by iteration starting with $w(0) = 0$ we can say that

$$\| w(q) \|^2 \leq \sum_{j=1}^q \| x_{i_j} \|^2 \leq q \beta$$

since $\beta = \max_i \| x_i \|^2$.

---

We can conclude the proof by writing

$$\frac{(q \alpha)^2}{\| w^* \|^2} \leq \| w(q) \|^2 \leq q \beta$$

and notice that

$$
q \beta \geq q^2 \alpha' \iff \beta \geq q \alpha'
\iff q \leq \frac{\beta}{\alpha'}
$$

where $q$ is the finite number of steps for convergence.

## Differentiable Activation Functions

Until now we used a threshold activation function that is not differentiable,
but this is not mandatory, in fact there are many activation functions (with
different behaviors) that are instead differentiable (like the sigmoidal
logistic function).

We want now to use these kind of activation functions in order to derive a LMS
algorithm by computing the gradient of the mean square loss function.

If we for example take as activation function the logistic function

$$f_\sigma (x) = \frac{1}{1 + e^{-ax}}$$

The activation output, from $o(x) = x^\top w$ becomes

$$o(x) = f_\sigma (x^\top w)$$

and consequently the problem is not minimizing the misclassified patterns
anymore, but it is to reduce the residual sum of squares:

$$E(w) = \sum_p (y_p - o(x_p))^2 = \sum_p (y_p - f_\sigma (x_p^\top w))^2$$

that of course brings to define a new **delta rule** for a gradient descent
algorithm for one pattern $p$:

$$w_\text{new} = w + \eta \cdot \delta_p \cdot x_p$$

where the $\delta_p$ term is equal to

$$
\delta_p = (y_p - f_\sigma (\text{net} (x_p))) \cdot
f_\sigma' (\text{net} (x_p)) =
(y_p - o(x_p)) \cdot f_\sigma'
$$

So with a differentiable function is now possible to use a gradient based
algorithm in order to minimize the error. This marks in some sense the birth of
an evolution of the Rosenblatt's perceptron for some key aspects.

- With these kind of unit the learning algorithm changes and tries to minimize a
  square loss function, regardless from misclassification or not.
- A LMS algorithm makes an update proportional to the error, while the standard
  perceptron makes an update proportional to the learning rate (that is a
  constant).

This is the foundational unit for neural networks called multi-layer perceptron.

## References

- [[supervised_learning]]
- [[activation_functions]]
- [[gradient_descent]]
- [[neural_networks]]

