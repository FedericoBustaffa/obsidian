---
id: stochastic_simulations
aliases: []
tags:
  - complex_systems
  - master
---

# Stochastic Simulations

Chemical reactions sometimes exhibit random behaviors that lead to the
definition of **stochastic simulation algorithms** to simulate them.

The occurrence of a chemical reaction in a chemical solution is actually
difficult to be predicted in advance because the movement of the molecules in
the chemical solution is related with the interaction with the fluid. If we
ignore this low level interaction, the process appears random because we cannot
predict exactly when a specific combination of reactants will meet.

Another aspect to consider is the concentration of reactants; in fact when we
have high concentrations of reactants, the **law of large numbers** allow us to
ignore random aspects of chemical reactions and justify the use of ODEs. But
with small concentrations the random aspects become crucial and **discrete
variables** become necessary. If we consider for example the reaction

$$A \xrightarrow{k} B + C$$

with only one molecule of $A$ in the solution. The ODEs would describe the
concentration of $A$ to pass slowly and continuously from $1$ to $0$. In the
real system the number of instances of $A$ would pass from $1$ to $0$ with a
discrete instantaneous change.

## Gillespie's Stochastic Simulation Algorithm

The _Gillespie's Stochastic Simulation Algorithm_ (**SSA**) is an exact procedure
for simulating the time evolution of a chemical reacting system by taking proper
account of the randomness inherent in such a system. Given a set of reactions
$\mathcal{R} = \{ R_1, \dots, R_n \}$, the SSA assumes a stochastic reaction
constant $C_\mu$ for each chemical reaction $R_\mu \in \mathcal{R}$ such that
$c_\mu dt$ is the probability that a particular combination of reactants of
$R_\mu$ react in an infinitesimal time interval $dt$.

The constant $c_\mu$ is used to compute the **propensity** (or stochastic rate)
of $R_\mu$ to occur in the whole chemical solution, denoted $a_\mu$, as follows

$$a_\mu = h_\mu c_\mu$$

where $h_\mu$ is the number of distinct molecular reactant combinations. Let $R_\mu$ be

$$
l_1 S_1 + l_\rho S_\rho \xrightarrow{c} l'_1 P_1 + l'_\gamma P_\gamma
$$

In accordance with standard combinatorics, the number of distinct reactant
combinations of $R_\mu$ in a solution with $X_i$ molecules of $S_i$, is given by

$$h_\mu = \prod_{i=1}^\rho \binom{X_i}{l_i}$$

with $1 \leq i \leq \rho$. The propensity $a_\mu$ is used in the algorithm as
the parameter of an **exponential probability distribution** modelling the time
between subsequent occurrences of reaction $R_\mu$. Below the _density_ function

$$
f(x) = \begin{cases}
\lambda e^{-\lambda x} & x \geq 0 \\
0 & x < 0
\end{cases}
$$

and the _cumulative_ function of the distribution

$$
F(x) = \begin{cases}
1 - e^{- \lambda x} & x \geq 0 \\
0 & x < 0
\end{cases}
$$

Remember also that the mean (expected value) of an exponentially distributed
variable with parameter $\lambda$ is $\frac{1}{\lambda}$. Two important
properties of the exponential distribution are:

- **Memory-less**: The simulation "forgets" about the history
  $$P(X > t + s \mid X > s) = P(X > t)$$
- Let $X_1, \dots, X_n$ be independent exponentially distributed random
  variables with parameters $\lambda_1, \dots, \lambda_n$. Then the variable
  $$X = \min (X_1, \dots, X_n)$$
  is also exponentially distributed with parameter
  $\lambda = \lambda_1 + \dots - \lambda_n$. So that the algorithm can use a
  unique exponential distribution for the whole set of reactions.

The algorithm keep the **state** of the simulation which is formed by

- A vector representing the multi-set of molecules (initially
  $X_1, \dots, X_n$).
- A real variable $t$ representing the simulation time (initially $t = 0$).

The algorithm iterates the following steps until $t$ reaches a final value
$t_\text{stop}$.

1. The time $t + \tau$ at which the next reaction will occur is randomly chosen
   with $\tau$ exponentially distributed with parameter $a_0 = \sum_{\nu = 1}^M
   a_\nu$.
2. The reaction $R_\mu$ that has occur at time $t + \tau$ is randomly chosen
   with probability $a_\mu / a_0$.

At each step $t$ is incremented by $\tau$ and the multi-set representing the
chemical solution is updated by subtracting reactants and adding products.

## References

[Stochastic Simulation of Chemical Kinetics](https://scispace.com/papers/stochastic-simulation-of-chemical-kinetics-53h3kqhk7k)

Tags: [[complex_systems]]
