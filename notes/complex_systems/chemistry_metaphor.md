---
id: chemistry
aliases: []
tags:
  - complex_systems
  - master
---

# Chemical Reaction Metaphor

Another, and in many cases, simpler way to deal with continuous models is by
think about them like **chemical reactions**. This can be done because for
example we deal with quantities like population densities that increase or
decrease, just like **reactants and products** of a chemical reaction.

## Notation

Chemical reactions describe transformation of molecules: a group of
**reactants** is transformed in a group of **products**.

Every molecule is assumed to float in a _fluid medium_ and the quantity of each
molecule is usually expressed in terms of **concentration**, the concentration
of molecule $A$ is denoted as $[A]$ and it is usually expressed in
$\text{mol} / \text{L}$.

A general notation for chemical reactions is the following

$$
l_1 S_1 + \dots + l_\rho S_\rho \xrightarrow{k}
l'_1 P_1 + \dots + l'_\gamma P_\gamma
$$

where

- $S_i$ and $P_i$ are respectively _reactants_ and _products_.
- $l_i, \; l'_i \in \mathbb{N}$ are **stoichiometric coefficients** which
  express the number of reactants or products of each type that are consumed or
  produced by the reaction.
- $k \in \mathbb{R}_{\geq 0}$ is the **kinetic constant** which is a coefficient
  used to compute the **rate of occurrence** of the reaction. This constant can be
  seen as the speed at which the reactants become products.

Chemical reactions are often **reversible** and they can occur in both
directions

$$
l_1 S_1 + \dots + l_\rho S_\rho
\underset{k_{-1}}{\stackrel{k}{\rightleftharpoons}}
l'_1 P_1 + \dots + l'_\gamma P_\gamma
$$

where $k_{-1}$ is the kinetic constant of the inverse reaction and that is not
related to $k$.

## Law of Mass Action

The assumption we take for modeling chemical reactions is that all the molecules
float in a fluid medium, where they are free to move and randomly meet with each
other.

When a group of molecules meet, they _can_ react and the dynamics of the
possible chemical reaction are modeled by the **law of mass action**, used to
compute the **rate** of a chemical reaction in a given chemical solution. In
other words, the number of occurrences of such reaction in a given chemical
solution in a time unit.

> [!note] Law of Mass Action
> The rate of a chemical reaction is proportional to the product of the
> concentrations of its reactants
> $$k[S_1]^{l_1} \cdots [S_\rho]^{l_\rho}$$
> The same is also true for the reverse reaction and in both cases the kinetic
> constant is the **proportionality ratio** of the reaction.

Let's also notice that the _kinetic constant_ is a intrinsic property of the
chemical reaction, independent of the amount of reactants.

The rate of a chemical reaction, given a chemical solution and a time span, is
the number of reactions occurred (coputed in function of the kinetic constant
and the reactants concentrations).

### Dynamic Equilibrium

The **dynamic equilibrium** is reached when, in a reversible reaction, the two
reactions constantly compensate each other. This happen when the rate of the two
is the same

$$
k[S_1]^{l_1} \cdots [S_\rho]^{l_\rho} = k_{-1}[P_1]^{l'_1} \cdots
[P_\gamma]^{l'_\gamma}
$$

It is easy to see that at the equilibrium we have

$$
\frac{k}{k_{-1}} = \frac{[P_1]^{l'_1} \cdots
[P_\gamma]^{l'_\gamma}}{[S_1]^{l_1} \cdots [S_\rho]^{l_\rho}}
$$

and in this state some _conservation properties_ are exploited.

## From Reactions to ODEs

Reactions rates can be used to build a differential equation model that
describes the dynamics of a set of chemical reactions. This model will contain
one differential equation for each molecular species. For each molecule $S$, an
ODE is constructed by applying these two rules:

- In a reaction where $S$ occurs as a reactant, its ODE contains a negative term
  $-lr$ where $l$ is stoichiometric coefficient of $S$ in the reaction and $r$ is
  the rate of the reaction.
- The same rule is applied when $S$ occurs in the products, but this time the
  term will be $+lr$.

From these two generic reactions

$$
l_1 S_1 + \dots + l_\rho S_\rho
\underset{k_{-1}}{\stackrel{k}{\rightleftharpoons}}
l'_1 P_1 + \dots + l'_\gamma P_\gamma
$$

we obtain the following ODEs

$$
\begin{cases}
\frac{d[S_1]}{dt} = l_1 k_{-1} [P_1]^{l'_1} \cdots [P_\gamma]^{l'_\gamma} -
l_1 k [S_1]^{l_1} \cdots [S_\rho]^{l_\rho} \\
\vdots \\
\frac{d[S_\rho]}{dt} = \dots \\
\frac{d[P_1]}{dt} = l'_1 k [S_1]^{l_1} \cdots [S_\rho]^{l_\rho} -
l'_1 k_{-1} [P_1]^{l'_1} \cdots [P_\gamma]^{l'_\gamma} \\
\vdots \\
\frac{d[P_\gamma]}{dt} = \dots
\end{cases}
$$

This is quite mechanical but it works and in the end we obtain a system of ODEs
where the concentrations have become state variables values and a combination of
kinetic constants and soichiometric coefficients have become the ODE's
coefficients.

> [!example]
> Let's now consider the following chemical reaction
> $$2H_2 + O_2 \underset{0.5}{\stackrel{5}{\rightleftharpoons}} 2 H_2 O$$
> The ODE that we can obtain for the $H_2$ molecule is
> $$\frac{d [H_2]}{dt} = 2 \cdot 0.5 [H_2 O]^2 - 2 \cdot 5 [H_2]^2 [O_2]$$

### From ODEs to Reactions

Often is also possible to pass from a ODE to a chemical reaction but with some
differences. Starting from the ODE we can translate every term in a chemical
reaction but this time we can have an infinite number of chemical reactions that
can be mapped from the initial ODE.

This is because a generic ODE can be generated from a chemical reaction by
adding and subtracting terms that are the product of all the reactants/products
and, more important the stoichiometric coefficient of the molecule and the
kinetic constant.

In other words when we translate from chemical reactions to ODEs we know the
value of every term involved, but when we do inverse translation we only have a
coefficient that is the result of a product among $k$, $l$ and various
concentrations.

> [!example]
> Let's consider the previous example ODE obtained from a chemical reaction
> $$\frac{d X}{dt} = 6X - 0.2XY$$
> If we consider only the first term ($6X$) we can say that the reaction has $X$
> as a reactant and produces $X$:
> $$X \xrightarrow{k} 2X$$
> Now we need the kinetic constant but, as we can see the exponent of $X$ is
> $1$, so it is its stoichiometric coefficient and so we have that
> $$1 * k = 6 \iff k = 6$$
> the final reaction obtained is
> $$X \xrightarrow{6} 2X$$
> if now repeat the same process for all the terms we obtain a set of chemical
> reactions that can be used in a numerical simulator.

There are cases where the translation is not possible, for example when the ODE
implies that a reaction reduces the concentration of some term without having
among its reactant:

$$\frac{dX}{dt} = 6X - 0.2XY - Y$$

In this case we have the negative term $-Y$ in which $X$ does not appear, but
the fact that it is negative means that some $X$ must be consumed as a reactant.

## References

Links: [[complex_systems]], [[continuous_systems]]
