*reference books* :  Lehman math for cs, basic probability - ash, wanskel notes on set theory, steven cook notes for logic 1, 2.

todo : 
- [ ]  proofs problems 
##### *1. Proofs* -

*proof*, A mathematical proof of a proposition is a chain of logical deductions leading to the propositions from a base set of axioms.

*proposition*, is a statement that is either **true** or **false**.

$P ::=$ "4 is a perfect square"  // *true*
*predicate*, can be understood as a proposition whose truth depends on the value of one or more variables.

$P(n) ::=$ "$n$ is a perfect square"  // *depends on n*

if $P$ is a predicate then $P(n)$ is either *true* or *false*.

* Propositions that are simply accepted as true are called ***axioms***.
* Important true propositions are called ***theorems***.
* A ***lemma*** is a preliminary proposition useful for proving later propositions.
* A ***corollary*** is a proposition that follows in just a few logical steps from a theorem.

Logical deductions or *inference rules* , are used to prove new propositions using previously proved ones.

Inference should be *sound* : an assignment of truth values to the letters $P,Q\ldots$ , that makes all the antecedents true must also make the consequent true.

$\frac{\neg P\implies\lnot Q}{Q\implies P}$

###### Proving an implication : 

*method 1* - 

1. Write "Assume $P$".
2. Show that $Q$ logically follows.

$a^2 - b^2 = (a+b)(a-b)$

Proof's typically begin with the word *Proof* and end's with a $\square$ or "QED".

###### *prove the contrapositive -

1. Write "We prove the contrapositve:" and then state the contrapositive.
2. Proceed as in Method 1.

***Theorem 1.5.2*** - *if $r$ is irrational, then $\sqrt{r}$ is also irrational*.

###### *if and only if* - 

$P\iff Q  \ \equiv \   P\implies Q \ and \ Q\implies P$

*method 1* : 

1.  Write, "We prove $P$ implies $Q$ and vice-versa".
2.  Write, "First we show $P$ implies $Q$ ".
3.  Write, "Now we show $Q$ implies $P$ ".

method 2 :

1. Write, "We construct a chain of if-and-only-if implications".
2. Prove $P$ is equivalent to a second statement which is equivalent to a third statement and so forth untill you reach $Q$.

The *standard deviation* of a sequence of values $x_1,x_2,\ldots$$,x_n$ is defined to be :

$\sqrt{\frac{(x_1-\mu)^2 + (x_2-\mu)^2 +\ldots+(x_n-\mu)^2}{n}}$

where $\mu$ is the average/mean of the values:

$\mu ::= \frac{x_1 + x_2 +\ldots+ x_n}{n}$

***Theorem 1.6.1*** - *the standard deviation of a sequence of values $x_1\ldots x_n$ is zero iff all the values are equal to the mean*.

square of real numbers are always non-negative.

###### *proof by cases* - 

***Theorem*** - *Every collection of 6 people includes a club of 3 people or a group of 3 strangers*

###### *proof by contradiction* -

In a *proof by contradiction,* or *indirect proof*, you show that if a proposition were false, then some false fact would be true. Since a false fact by definition can't be true, the proposition must be true.

*method* : 

1. Write, "We use proof by contradiction".
2. Write, "Suppose $P$ is false".
3. Deduce something known to be false ( a logical contradiction)
4. Write "This is a contradiction. Therefore, $P$ must be true".

***Theorem 1.8.1*** - *$\sqrt{2}$* is irrational.

- square of a even number is even and square of a odd number is odd.
- every rational number can be represented in a reduced fraction form i.e numerator and denominator are coprime.

$\sqrt{rs} = \sqrt{r}*\sqrt{s}$

this only hold's for non-negative real numbers. you need to use complex numbers in order to find square root of negative numbers. take the imaginary part out and do the operation.

##### The Well Ordering Principle - 

*Every nonempty set of nonnegative integers has a smallest element*.

*there are no fractions which cannot be written in lowest terms*

$\frac{m_0}{n_0}$ are defined so that $m_0/n_0$ cannot be written in lowest terms. This means that by definition they have a common factor (if they didn't than this means they are already in lowest terms, so they can be written in lowest terms).

To prove "$P(n)$ is true for all $n\in \mathbb{N}$ " using Well Ordering Principle :

*  Define the set *C* of counterexamples to $P$ being true.
	
	*$C ::= \{ \ n\in\mathbb{N}\ |\ NOT(P(n))\ is\ true \}$

*  Assume for proof by Contradiction that $C$ is nonempty.

*  By well ordering principle , there will be a smallest element,$n$, in $C$ .

*  Reach a contradiction somehow --- often by showing that $P(n)$ is actually true or by showing that there is another member of $C$ which is smaller than n.

*  Conclude that $C$ must be empty, that is, no counterexamples exists.

***Theorem 2.2.1*** -  *$1+2+3\ldots +n = n(n+1)/2$  for all non negative integers $n$.*

***Theorem 2.3.1*** -  *Every positive integer greater than 1 can be factored as a product of primes*


***Well Ordered Set's*** - 

A set of numbers is *well ordered* when each of its nonempty subsets has a minimum element.

when checking if a set is well ordered or not, check if the least element property holds for every nonempty subset of the set and not for just the original set.

***Theorem 2.4.1*** -  *For any nonnegative integer, n, the set of integers greater than or equal to n is well ordered.* 

##### 2. Induction - 

*Induction* is a powerful method for showing  a property is true for all nonnegative integers.

If student $n$  gets a candy bar then student $n+1$  gets a candy bar, for all nonnegative integers n.

**The Induction Principle** :

Let $P$ be a predicate on nonnegative integers (*induction hypothesis*) . If 

* $P(0)$ is true, and  //*base case*
* $P(n)\implies P(n+1)$ for all nonnegative integers, $n$,  //*inductive step*
then
* $P(m)$ is true for all nonnegative integers, $m$.

***Theorem 5.1.2*** - *For all $n\geq0$ there exists a tiling of $2^n \times 2^n$ courtyard with bill in a central square.

"If you can't prove something, try to prove something grander".

---

*proposition* - statement that is either true or false

*predicate* - proposition whose truth depends on value of variable(s)

*proof* - is a verification of a proposition using chain of logical deductions and base set of axioms.

$p\implies q$ is true when $p$ is *false* or $q$ is *true* .

*axioms* - is a proposition(s) that we assume to be true.

*proof by contradiction -* to prove $p$ is true, we assume $p$ is not true and then use that hypothesis to derive a falsehood or a contradiction.

*induction* - if for a predicate $p$ , $p(0)$ is *true* and $\forall n \in \mathbb{N}\  p(n) \implies p(n+1)$ then $\forall n \in \mathbb{N}\  p(n)$ is *true*.

base case, inductive step
assume p(n) is true for purposes of induction.

prove by induction , $\forall n \in \mathbb{N}\  p(n) = 3 \mid (n^3 - n)$ is *true* 
prove , all horses are the same color.
hint : assume any set of n horses are the same color. false predicate, fails when horses = 2.

**problem** : fina a sequence of moves to go from 

| A   | B   | C   |
| --- | --- | --- |
| D   | E   | F   |
| H   | G   |     |
to 

| A   | B   | C   |
| --- | --- | --- |
| D   | E   | F   |
| G   | H   |     |
legal move : slide a letter into adjacent black square.

*Theorem* - There is no sequence of legal moves to invert G and H and also return all the other letters to their original position.

use ***invariant*** to show that your system can never reach this special state. if a property called the invariant holds at the initial state of the system and is preserved after each legal move and it not preserved in the special state then that state cannot be reached via legal moves from the initial instance of the system. you can only reach final states that holds the invariant property.

row moves doesn't change order or increase/decrease number of inversions. column moves increases/decreases number of inversions by +2/-2 but they cannot be reduced to zero, they stay odd. at most they can be reduced to one. the desired order of elements has zero inversions and when we try to get to the desired order from a randomly ordered grid we are stuck at the final step of getting from one inversion to zero.

*invariant* - parity of the number of inversions remains odd in every state reachable from the start state.

***strong-induction*** : Let $P(n)$ be any predicate. if $P(0)$ is true & $\forall n \in \mathbb{N} (P(0) \land P(1) \ldots \land P(n)) \implies P(n+1)$ is true, then $P(n)$ is true for $\forall n \in \mathbb{N}$ .

*euclid's lemma* - if a prime *p* divides a product $a\cdot b$ , then *p* must divide at least one of *a* or *b*.

Problem 1.19 -  prove that $\log_{12} 18$ is irrational.

---
$p\implies q$ is true when either $p$ is *false* or $q$ is *true* , that is $\lnot p \lor q$ .

a truth assignment is a map $\tau : {atoms} \to {T,F}$ .

$(\lnot A)^{\tau} = T \iff a^{\tau} = F$ 

a truth assignment $\tau$ satisfies $A$ iff $A^{\tau} = T$. $\tau$ satisfies a set $\Phi$ of formulas iff $\tau$ satisfies $A$ for all $A \in \Phi$ . The set $\Phi$ is satisfiable iff some $\tau$ satisfies $\Phi$, otherwise it is unsatisfiable.

$\Phi \models A$ , A is logical consequence of $\Phi$ iff for every $\tau$ , if $\tau$ satisfies $\Phi$ then $\tau$ satisfies $A$.

$A$ is valid iff $\forall \tau A^{\tau} = T$ , a valid propositional formula is also called a *tautology*.

$A$ and $B$ are *equivalent* , $A\iff B$ , iff $A\models B$ and $B\models A$ .

*disjunction* - $\lor$ of propositional formulas
conjuction - $\land$ of propositional formulas
*literal* - a atom, $P$ or $\lnot P$
*clause* - is a disjunction of literals such that no variable occurs twice (negated or not) in disjunction.

a formula is in *conjunctive normal form* (CNF) , if it is a conjunction of one or more clauses.

a formula is in *disjunctive normal form* (DNF), if it is a disjunction of $\land$-clauses.

**Theorem** : Every formula is equivalent to a formula  in CNF, and to a formula in DNF.

if $A$ is unsatifiable then DNF is the empty disjunction $\lor \emptyset$ .

---

n-ary functions take n arguments from domain and produces a output in co-domain
n-ary predicate take n arguments and produces either true or false.

a first order vocabulary or language $\mathcal{L}$ is specified by following :

1. For each $n\in\mathbb{N}$ a set of n-ary function symbols. we use $f,g,h,...$  and also $+,\cdot,s$ as meta symbols for function symbols.
2. for each $n\geq 0$ , a set of n-ary predicate symbols. we use $P,Q,R,...$ and also $<,\leq,=$ as metasymbols for predicate symbols. a zero-ary predicate symbol is same a prop. atom.

in addition, following symbols are available to build first-order formulas :

1. an infinite set of variables
2. connectives $\lnot,\land,\lor$
3. quantifiers $\forall,\exists$ 
4. $(,)$ parentheses

standard language/vocabulary of arithmetic is $\mathcal{L}_{A} = [0,s,+,.;=]$

$0$ - constant
$s$ - unary function symbol
$+,.$ - binary function symbol
$=$  - binary predicate symbol

###### relations and partial orders -

given sets $A$ and $B$ , a binary $R : A \to B$  from $A$ to $B$ is a subset of $A\times B$. 
$A$ and $B$ are called *domain* and *codomain* of $R$ , respectively.
we commonly use $aRb$ or $a \sim_{R} b$  to denote the $(a,b) \in R$

every function $f : A \to B$ is a relation. a relation might associate multiple elements of $B$ with a single element of $A$ , whereas a function can associate at most one element of $B$ (namely $f(a)$) with each element $a\in A$.

the *inverse* $R^{-1}$ of a relation $R : A\to B$ is the relation from $B$ to $A$ defined by the rule 

$bR^{-1}a \iff aRb$ 

inverse image = pre-image

given two relations $R : A \to B$ and $S : B \to C$ , the *composition* of $R$ with $S$ is the relation $(S\circ R) : A \to C$  defined by the rule 

$a(S\circ R)c \iff \exists b \in B | aRb \land bRc$

composition of two functions , $(f\circ g)(a) = f(g(a))$

*total* function covers entire domain, every element of domain has a image.
*partial* function , some element may not have a image.

Let $A$ and $B$ be finite sets,

1. if there is a injection from $A$ to $B$ then, $\lvert A\rvert \leq \lvert B\rvert$ .
2. if there is a surjection from $A$ to $B$ then, $\lvert A\rvert \geq \lvert B\rvert$ .
3. if there is a bijection from $A$ to $B$ then, $\lvert A\rvert = \lvert B\rvert$ .

A relation on set $A$ can be represented by a digraph, where vertices are elements and edges is relation between them.

A relation is an *equivalence relation* if it is reflexive,symmetric and transitive.
eg. congruence modulo *n*.

given a equivalence relation $R : A\to A$ , the equivalence class of an element $x\in A$ is the set of all elements of $A$ related to $x$ by $R$. 

$[x] = \{\ y\ |\ x\ R\ y\ \}$

all the elements in $[x]$ have the same equivalence class.

A *partition* of a finite set $A$ is a collection of disjoint,nonempty subsets $A_1,A_2,\dots A_n$ whose union is all of $A$ . there's a bijection between *equivalence relations* on $A$ and *partitions* of $A$.

A relation $R$ on a set $A$ is a *weak partial order* if it is transitive, antisymmetric and reflexive. The relation is said to be *strong partial order* if it is transitive, antisymmetric and irreflexive.

$\preceq \ \  \prec$

A *total order* is a partial order in which every pair of distinct elements is comparable.

given a *partial order* $\preceq$ on a set $A$ , the pair $(A,\preceq)$ is called a *partially ordered set* or *poset*.

*a poset has no directed cycles other than self-loops*.

poset $\iff$ DAG

A **topological sort** takes a poset and produces a toset where all the original relationships are preserved and elements that weren't related can be ordered arbitarily.

an element $x\in A$ is *minimal* if there is no other element $y\in A$ such that $y\preceq x$.
an element $x\in A$ is *maximal* if there is no other element $y\in A$ such that $x\preceq y$.

*every finite poset has a topological sort*.
*every finite poset has a minimal element*.

parallel task scheduling. topological sort.

*the total amount of parallel time needed to complete the tasks is the same as the length of the longest chain of dependent tasks.*

*every partial order with n elements has a chain of size greater than $\sqrt{n}$ or an antichain of size at least $\sqrt{n}$*.

**Schroder-Bernstein** - *For any pair of sets A and B, if A surj B and B surj A then A bij B*.

*Let A be a set and b $\notin$ A . Then A is infinite iff A bij A U { b }*.

same as shifting guests from room i to i+1 and inserting new guest in room one.

*a set C is countably infinite iff $\mathbb{N}$ bij C . a set if countable iff it is finite or countably infinite.*

*For any set A, the power set $\mathcal{P}(a)$ is strictly bigger than A.*

proof :  to prove $\lvert X\rvert < \lvert \mathcal{P}(X)\rvert$

consider a function $\phi : X\to \mathcal{P}(X)$ ;   $\phi(x) = \{x\}$ is injection.

$\gamma : X\to \mathcal{P}(X)$ ,

suppose that this function is surjective. consider a set $Y = \{ x \in X | x \notin \gamma(x) \}$ 
(set of elements of X that doesn't belong to their images). 

since $\gamma$ is a surjection there is $x_0 \in X$ such that $\gamma(x_0) = Y$ .

if $x_0 \in Y$ then $x_0 \notin Y$ 
if $x_0 \notin Y$ then $x_0 \in Y$


