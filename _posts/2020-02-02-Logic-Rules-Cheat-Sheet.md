---
layout: post
title: Logic Rules Cheat Sheet
category: Computer Science
tags: [Computer Science, Math]
excerpt_separator: <!--more-->
---

When working with logic in discrete math appliations there are a plethora of rules you can use for working with the *<a href="https://en.wikipedia.org/wiki/Well-formed_formula" target="_blank">well formed formulas</a>*. Remembering them all can be a daunting task, which is why I like to have a cheat sheet available. As such, here's a simple one that I like to use when working with these problems.

<!--more-->

# Equivalence Rules

|  Expression       |  Equivalent To       |  Name of the Rule       |
|:-----------------:|:--------------------:|:-----------------------:|
| $$ P \lor Q $$    | $$ Q \lor P $$       | Commutative - comm.     |
| $$ P \land Q $$   | $$ Q \land P $$      | Commutative - comm.     |
| $$\left(P\lor Q\right)\lor R$$ | $$P \lor\left(Q\lor R\right)$$ | Associative - ass. |
| $$\left(P\land Q\right)\land R$$ | $$P\land\left(Q\land R\right)$$ | Associative - ass. |
| $$\left(P\lor Q\right)'$$ | $$P'\land Q'$$ | DeMorgan's Law |
| $$\left(P\land Q\right)'$$ | $$P'\lor Q'$$ | DeMorgan's Law |
| $$P\rightarrow Q$$ | $$P'\lor Q$$ | Implication - imp. |
| $$P$$ | $$\left(P'\right)'$$ | Double Negation - dn. |
| $$P\iff Q$$ | $$\left(P\rightarrow Q\right)\land\left(Q\rightarrow P\right)$$ | Definition of Equivalence - equ. |

# Inference Rules

|  Expression       |  Can Derive          |  Name of the Rule       |
|:-----------------:|:--------------------:|:-----------------------:|
| $$P,\ P\rightarrow Q$$ | $$Q$$ | Modus Ponens - mp. |
| $$P\rightarrow Q,\ Q'$$ | $$P'$$ | Modus Tollens - mt. |
| $$P,\ Q$$ | $$P\land Q$$ | Conjunction - con. |
| $$P\land Q$$ | $$P,\ Q$$ | Simplification - simp. |
| $$P$$ | $$P\lor Q$$ | Addition - add. |
| $$P\rightarrow Q,\ Q\rightarrow R$$ | $$P\rightarrow R$$ | Hypothetical Syllogism - hs |
| $$P\lor Q,\ P'$$ | $$Q$$ | Disjunctive Syllogism - ds. |
| $$P\rightarrow Q$$ | $$Q'\rightarrow P'$$ | Contraposition - cont. |
| $$Q'\rightarrow P'$$ | $$P\rightarrow Q$$ | Contraposition - cont. |
| $$P$$ | $$P\land P$$ | Self Reference - self. |
| $$P\lor P$$ | $$P$$ | Self Reference - self. |
| $$\left(P\land Q\right)\rightarrow R$$ | $$P\rightarrow\left(Q\rightarrow R\right)$$ | Exportation - exp. |
| $$P,\ P'$$ | $$Q$$ | Inconsistency - inc. |

# Derivation Rules

The general workflow for using derivation rules is:

* Strip off the quantifiers
* Work with the independent well formed formulas
* Insert the quantifiers back in

**Universal Instantiation - ui.**:
From $$\left(\forall x\right)P\left(x\right)$$ we can deduce $$P\left(t\right)$$.

*Note: t must not already appear as a variable in the expression for $$P\left(x\right)$$.*

**Existential Instantiation - ei.**:
From $$\left(\exists x\right)P\left(x\right)$$ we can deduce $$P\left(t\right)$$.

*Note: t must be introduced for the first time. As such, you will want to do these early in proofs.*

**Universal Generalization - ug.**:
From $$P\left(x\right)$$ we can deduce $$\left(\forall x\right)P\left(x\right)$$.

*Notes:*
* *$$P\left(x\right)$$ hasn't been deduced by existential instantiation from any hypothesis in which x was a free variable.*
* *$$P\left(x\right)$$ hasn't been deduced by existential instantiation from another wff in which x was a free variable.*

**Existential Generalization - eg.**:
From $$P\left(a\right)$$ we can deduce $$\left(\exists x\right)P\left(x\right)$$.

*Note: x must not appear in $$P\left(a\right)$$.*
