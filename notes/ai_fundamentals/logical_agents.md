---
id: logical_agents
aliases: []
tags:
  - master
  - ai_fundamentals
---

# Logical Agents

A popular type of agents are **logical agents**, also called **knowledge-based**
agents, which, as the name suggest, maintain a _knowledge base_ (KB) on the
environment under the form of **logic sentences**.

Typically they start with a general knowledge of the rules of the environment,
but through perceptions they can add new knowledge and, most important, they can
generate new knowledge by **inference**.

This time we have a **declarative approach**, were the agent has a
representation of the world which says what it knows, not what it does.

## Entailment

To understand how logical agents are able to infer new knowledge is necessary
to define what a **possible model** (**world**) is. A possible model is a
mathematical abstraction with a fixed trruth value for each relevant sentence.

If a sentence $\alpha$ is true in a model $m$ we say that $m$ **satisfies**
$\alpha$ and $M(\alpha)$ is the set of all models of $\alpha$.

The first piece for inference is the concept of **entailment**, that is defined
as a relationship between sentences that is based semantically when a sentence
logically follows another.

More formally we say that $\alpha$ **entails** $\beta$:

$$\alpha \models \beta$$

that is equivalent to say that for every model where $\alpha$ is true then also
$\beta$ is true. In this sense $\alpha$ is a stronger assertion than $\beta$ as
it rules out more possible models.

![Entailment](/files/entailment.png)

This let us avoid considering all the possible models by simply enumerating
them. In other words, the entailment let the agent use its knowledge in order to
exclude some models and so, restricting the possible choices to the ones that
can be true.

### Model Checking

The simplest way to verify entailment is via **model checking**, that consists
in enumerating all the possible models and check whether $\alpha$ is true in all
models in which KB is true.

We enumerate all the possible models where KB is true, and all the possible
model where $\alpha$ is true.

![Entailment on Wumpus World|400](/files/wumpus_entailment1.png)

If the knowledge base rules out the sentence $\alpha$, then we obtain the
possible models where the sentence is true and where all the knowledge is still
true. In other words the intersection between the two represents all the
possible realizations of sentences in the actual environment (based on the KB);
what is outside the intersection cannot be true with respect to the KB.

Let's notice that the KB models where $\alpha$ is true must be a susbet of
$\alpha$ in order to conclude that the sentence is true also for KB.

![Entailment on Wumpus World|400](/files/wumpus_entailment2.png)

If the models of KB are not contained in the models of $\alpha$, the entailment
does not hold. This is like saying that there are possible models where the
sentence is true but, based on the KB and the perceptions, there are no
guarantees that they will realize concretely.

## Inference

Now we want an **inference algorithm** capable of use entailment and classical
logic laws, in order to generate new knowledge automatically. But first some
definitions

- **Equivalence**: $\alpha \equiv \beta$ if and only if $\alpha \models \beta$
  and $\beta \models \alpha$.
- **Validity**: a sentence is valid if it is true in all models.
- **Satisfiability**: a sentence is **satisfiable** if is true in some model. It
  is unsatisfiable if it is never true.

If a sentence $\alpha$ is _derived_ from KB by the the inference algorithm $i$,
than we can say that KB _infers_ $\alpha$:

$$\text{KB} \vdash_i \alpha$$

An inference algorithm can be

- **Sound**: $\text{KB} \vdash_i \alpha \implies \text{KB} \models \alpha$
- **Complete**: $\text{KB} \models \alpha \implies \text{KB} \vdash_i \alpha$

## References

- [[artificial_intelligence_fundamentals]]
