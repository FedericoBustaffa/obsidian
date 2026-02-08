---
id: first_order_logical_agents
aliases: []
tags:
  - master
  - ai_fundamentals
---

# First Order Logical Agents

A more powerfull type of logical agent, with respect to propositional logical
agents, are **first order logical agents**, that exploit **first order logic**
(**FOL**) constructs in order to be more general.

Propositional logic is only able to model _facts_, while in the FOL, is
possible to model objects and relations or properties of them. If a property has
only one value for each input, then it can be seen has a function.

In FOL the syntax is composed by

- **Constants**: $5$, _John_
- **Predicates**: _brotherOf_
- **Functions**: _sqrt_
- **Variables**: $x$, $y$
- **Connectives**: $\lnot$, $\land$, $\lor$
- **Equality**: $=$
- **Quantifiers**: $\exists$, $\forall$

Typically in the FOL we have **sentences**, formed by

- **Terms**: logical expressions referring to an object, like functions,
  constants o variables.
- **Atomic sentence**: defined by a predicate $p$ or a term equality.
- **Complex sentences**: atomic sentences linked by connectives.

A sentence is true with respect to a given **model** and **interpretation**,
where

- A **model**, that contains one or more domain elements (objects) and their
  relations.
- An **interpretation** associates a concrete referent to each constant
  (object), predicate and function (relations).

A model with an interpretation is a model in the propositional logic.

## Computational Issues

A common assumption for FOL agents is the _closed world assumption_: atomic
sentences that are not known to be true or false are assumed false. This allows
to get rid of boilerplate stuff that handles incomplete information. In this
kind of agents we assume one unique referent of each constant, predicate or
function.

However with the introduction of quantifiers, entailment via model checking is
very expensive or impossible. For example if we have a variable $x$ that can be
every real value in $[1, +\infty]$ the computation never stops.

In case of _universal quantification_, a statement in the form

$$\forall x \, P$$

is true in a model if and only if the sentence $P$ is true for every possible
object associated with $x$ in the model. In propositional logic it is a possibly
infinite conjunction of instances of $x$.

In case of _existential quantifiers_, a statement in the form

$$\exists x \, P$$

is true in a model if and only if the sentnces $P$ is true for at least one
object associated with $x$ in the model. In propositional logic is a possibly
infinite disjunction of instances of $x$.

## Knowledge Base

In order to build a FOL agent we need to build a **FOL knowledge base** and this
is typically done by

1. Identify questions.
2. Assemble the relevant knowledge.
3. Decide on a vocabulary predicates, functions and constants.
4. Encode general knowledge about the domain.
5. Encode a specific description of the problem instance.
6. Ask queries to the inference algorithm to get answers.
7. Debug and evaluate the knowledge base.

For example in the Wumpus World we want to have an interface _ask_ and _tell_;
the agent perceives a _breeze_ at $t = 5$, so

- _Tell (KB, Percept([Smell, Breeze], 5))_
- _Ask (KB, $\exists a$ Action($a$, 5))_

And the answer should be _$a$ = Shoot_.

A useful construct is the **substitution** operator $\sigma$ and it works by
assigning (or substituting) a term value to a variable. For example we can have
a predicate

$$S = \text{Smarter} (x, y)$$

and a substitution

$$\sigma = \{ x / \text{Mario}, y / \text{Luigi} \}$$

the resulting sentence is

$$S \sigma = \text{Smarter} (\text{Mario}, \text{Luigi})$$

This is used when the agent query the KB, for example _Ask(KB, $S$)_; the
answer will be some or all substitutions $\sigma$ such that $KB \models S
\sigma$.

## Quantifiers Instantiations

The **universal instantiation (UI)** tells us that any sentence obtained by
replacing a _universally quantified_ variable with a **ground term** can be
inferred.

$$\frac{\forall v \; a}{\sigma(\{ v / g \}, \, a)}$$

that applies for every variable $v$, sentence $a$ and ground term $g$.

The **existential instantiation (EI)** tells us that from a sentence that
confirms the existance of something we can infer a sentence with a new constant
$k$, called **Skolem constant**.

$$\frac{\exists v \; a}{\sigma (\{ v / k \}, \, a)}$$

that applies for every variable $v$, sentence $a$ and constant symbol $k$ that
does not belong to any other object.

Applying UI repeatedly means **adding** new sentences to the KB, but the
resulting KB is logically equivalent to the initial KB. If we apply the UI for
every possible sentence in order to remove every $\forall$, we
_propositionalize_ the KB.

Applying EI repeatedly means **replacing** the existential quantification with a
new sentence. In this case the resulting KB is not logically equivalent to the
initial and it is satisfiable if and only if the initial was.

## Unification

Usually the process of propositionalization generates a lot of useless stuff.
Taking also into account that if we have $p$ predicates that are $k$-ary and $n$
constants, we can generate $p n^k$ possible instances.

The trick is to find a way to make different logical expression identical, such
that we reduce the number of possible instances. For this purpose,
**unification** can be use to _unify_ two different expressions:

$$\text{unify} (\alpha, \beta) = \theta)$$

if $\alpha \theta = \beta \theta$, with $\alpha$ and $\beta$ sentences and
$\theta$ substitution.

If for example we have

$$
\begin{align*}
\alpha &= \text{Knows} (\text{John}, x) \\
\alpha &= \text{Knows} (\text{John}, \text{Jane})
\end{align*}
$$

the unifier will be

$$\theta = \{ x / \text{Jane} \}$$

because $\alpha \theta = \text{Knows} (\text{John}, \text{Jane})$ that is
equivalent to $\beta$.

### Generalized Modus Ponens

The unification let us define the **generalized Modus Ponens (GMP)** as follows

$$\frac{p_1', \dots, p_2', \; (p_1 \land \cdots p_n \to q)}{q \theta}$$

where $\forall i p_i' \theta = p_i \theta$ and $p_i'$ and $p_i$ are _atomic_.

The GMP lifts the Modus Ponens, adding the minimal amount of complexity needed
to make it work in FOL, obtaining a _sound_ algorithm.

## Chaining

Since we are dealing with definite clauses we are not covering the entire FOL,
but this is often suffcient to most of the real world cases.

The **forward chaining**, starting from the known facts, triggers all the
possible applicable rules and add the conclusions to the facts. When the query
is generated, it returns true, otherwise, if no new facts are generated, returns
false. The _forward chaining_ is sound and complete.

The **backward chaining** starts from the query $q$, generates all the terms
connected to $q$ until it generates the premise to infer $q$. The _backward
chaining_ is sound but not complete in case of infinite space or loops.

## Resolution in FOL

What is needed for an inference algorithm with FOL is **resolution rule**:

$$
\frac{p_i \lor \cdots \lor p_k, \; m_1 \lor \cdots \lor m_n}
{p_1 \lor \cdots \lor p_{i-1} \lor p_{i+1} \lor \cdots \lor p_k \lor
m_1 \lor \cdots \lor m_{j-1} \lor m_{j+1} \lor \cdots \lor m_n} \theta'
$$

where

$$\theta = \text{unify}(p_i, \lnot m_j)$$

we can then apply resolution steps to $\text{CNF}(KB \land \lnot \alpha)$ to
prove it is unsatisfiable.

Resolution in FOL is complete and if $KB \models \alpha$ then resolution
inference will derive $\alpha$ by contraddiction. It is also _semidecidable_
because it will find proof for a true statement, but for a false statement might
never stop.

## Questions

- What is an object?
  - An element of the domain like a constant.
- What is a relation?
  - A set of tuples of objects for which a predicate is true.
- What is a referent?
  - A symbol that links to an actual thing in the domain.
- What the interpretation actually does?
  - A map that links FOL syntax to the model semantics. For example assigns a
    objects to every constant or a relation to every predicate.

## References

- [[artificial_intelligence_fundamentals]]
- [[logical_agents]]
