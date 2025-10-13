*reference books* :  Lehman math for cs, basic probability - ash, wanskel notes on set theory, steven cook notes for logic 1, 2.
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

1. Write "Assume $P$ ".
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

infinite set of negative integers is not well-ordered, coz it doesn't have a least element, whereas infinite set of non-negative integers does have a least element. 

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

Problem 1.19 -  prove that $\log_{12} 18$ is irrational. (use prime factorization, powers of primes must be same in equality)

---
$p\implies q$ is true when either $p$ is *false* or $q$ is *true* , that is $\lnot p \lor q$ .

a truth assignment is a map $\tau : {atoms} \to {T,F}$ .

$(\lnot A)^{\tau} = T \iff A^{\tau} = F$ 

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
*every finite poset has a minimal/maximal element*.

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


###### Number Theory - 

number theory is the study of the intergers $\mathbb{Z}$ .

 $a$ divides $b$   *iff*   $b = a\cdot k$ , for some $k$.

1. *if a | b , then a | bc  for all c* .
2. *if a | b and b | c , then a | c*
3. *if a | b and a | c , then a | sb + tc  for all s and t*
4. *for all c $\neq$ 0 , a | b iff  ca | cb* 

*division theorem* - Let *n* and *d* be integers such that *d > 0 . Then there exists a unique pair of integers q and r , such that  $n = q\cdot d + r$  and  $0\leq r < d$ .

*famous conjectures in number theory* - 

- *fermats last theorem* : there are no positive intergers $x,y,z$  such that $x^n + y^n = z^n$  for some interger $n> 2$  .

- *goldback conjecture* : every even integer greater than two is equal to the sum of two primes.

- *there are infinitely many primes p such that p+2 is also a prime*

- *factoring* - given the product of two large primes *n = pq* , there is no efficient way to recover the primes *p and q* .

naive primality test algorithm $\sqrt{n}$ is polynomial in $n$ ,  but is exponential in $\log_2n$ (number of digits in binary representation)

$\sqrt{n} = 2^{m/2}$ ,  $m=\log_2n$ (number of bits in n).


equation of the form $s\cdot a + t\cdot b$ , is called an *integer linear combination* of a and b .

*greatest common divisor of a and b is equal to the smallest positive linear combination of a and b .*

*proof* - 

by well ordering principle, there is a smallest positive linear combination of a and b , call it m. we'll prove that *gcd(a,b) = m* , i.e *gcd(a,b) <= m  &&  m <= gcd(a,b)*

first we show that *gcd(a,b) <= m*, for any common divisor *c* ,  *c | a* & *c | b* $\implies$  *c | sa + tb*
$\implies$ *c | m* , *gcd* is also a common divisor , so *gcd(a,b)* divides *m* , $\implies$ *gcd(a,b) <= m*.

to prove *m <= gcd(a,b)*, we need to show *m | a* and symmetrically *m | b*,

if *m | a* , *a = qm + r* , we know that *m = sa+tb*, 
*r = (1-qs)a + (-qt)b* , so *r* is linear combination of *a,b*, but *m* is the smallest positive linear combination of *a,b*, this means that *r* is not positive $\implies$ *r=0* ($0\leq r < m$) $\implies$ *a = qm + 0* 
so *m | a* and symmetrically *m | b* . so *m* is a common divisor of *a,b* and it will be less than the greatest common divisor , so we have *m <= gcd(a,b)* . 

*An interger is linear combination of a and b iff it is a multiple of gcd(a,b)*

suppose *d = gcd(a,b)* , *d | a* & *d | b*
*a = dx , b = dy*

for any linear combination, ai + bj  = dxi + dyj = d(xi+yj)
so *d | ai+bj* , therefore any linear combination of a,b is multiple of gcd(a,b).

$gcd(a,b)  = gcd(b,rem(a,b))$

- fill the smaller jug
- pour all the water of smaller jug in larger jug . if at any time larger jug becomes full empty it out and continue pouring water from smaller jug into the larger jug.

this method eventually generates all the multiples of the gcd of the jug capacities.

*fundamental theorem of arithmetic* - 

*Every positive integer n can be written in a unique way as a product of primes :*

$n = p_1 \cdot p_2 \ldots p_j$

the product of an empty set of numbers is defined to be 1. sum of empty set of numbers is 0.

*if p is a prime and p | ab then p | a or p | b* .

the $1/ln(n)$ formula is not a probability for the specific interger *n*, but a *heuristic* for the average likelihood that a number close to *n* is prime.

*Theorem* - 100% logically proved fact.

*Heuristic* - an educated guess, approximation, or method that helps us understand/predict something, but without strict proof.

**Modular Arithmetic** - 

*a is congruent to b modulo n iff n | (a-b)* 

$a \equiv b\ (mod\ n)$

$a\equiv b \ (mod\ n)$ *iff*  $rem(a,n) = rem(b,n)$

the *multiplicative inverse* of a number $x$ is another number $x^{-1}$ such that : 

$x\cdot x^{-1} = 1$ 

multiplicative inverses exist over real numbers. inverses generally do not exist over the integers.
but they do exists when we're working *modulo a prime number*.

$7\cdot 3 \equiv 1\ (mod\ 5)$ , here 3 is the multiplicative inverse of 7.

numbers congruent to 0 modulo 5 ( i.e multiples of 5) do not have inverses.

*if p is a prime and k is not a multiple of p, then k has a multiplicative inverse modulo p*.

*suppose p is prime and k is not a multiple of p. Then 

$ak \equiv bk \ (mod\ p) \implies a\equiv b \ (mod\ p)$ 

*Fermat's Little Theorem* - 

*suppose p is prime and k is not a multiple of p then,

$k^{p-1} \equiv 1 \ (mod\ p)$ 

you can use this find multiplicative inverses,

$k^{p-2}\cdot k \equiv 1 \ (mod \ p)$ , here $k^{p-2}$ is the multiplicative inverse of $k$.

then find $rem(k^{p-2},p)$ using repeated squaring,

eg. find inverse of $6\ mod \ 17$ .

*6 mod 17*   ->    $6^{16} \equiv 1\ (mod\ 17)$  ->  $6^{15}\cdot 6 \equiv 1\ (mod\ 17)$ 

find $6^{15}$ using repeated squaring method,

$6^2$ = 36 = 2 (36 mod 17)
$6^4 = 2^2$
$6^8 = 2^4$

$6^8\cdot 6^4\cdot 6^2\cdot 6 = 16\cdot4\cdot2\cdot6 = 3$

so 3 is multiplicative inverse of 6, $6\cdot3 \equiv 1\ (mod\ 17)$

*integers a and b are relatively prime iff gcd(a,b) = 1*

*let n be a positive integer. if k is relatively prime to n, then there exists an integer $k^{-1}$ such that* 

$k\cdot k^{-1} \equiv 1 \ (mod \ n)$ 

*Euler's theorem* - 

*the exponent of k needed to produce multiplicative inverse of k mod n depends on number of integers in the set {1,2,...,n} that are relatively prime to n*. This value is known as *Euler's Totient function*  $\phi(n)$ . 
for example $\phi(7) = 6$ . since 1,2,3,4,5,6 are all relatively prime to 7.
if *n* is prime then $\phi(n) = n-1$ .

*for any number n, if $p_1,p_2,...,p_j$ are distinct prime factors of n then,

$\phi(n) = n\cdot(1-\frac{1}{p_1}\cdot(1-\frac{1}{p_2})\ldots (1-\frac{1}{p_j}))$ 

*let n = pq , where p and q are different primes then*  $\phi(n) = (p-1)(q-1)$ 

*suppose n is a positive interger and k is relatively prime to n then*

$k^{\phi(n)} \equiv 1\ (mod\ n)$ 


---

*purple-red eye island* - 

suppose $p=2$ , then after the anouncement, on day 1, there are two purple eyes. there can be two cases from each purple eyes person perspective,
case 1 - he is red and the other purple eyed person will leave after day 1.
case 2 - he is purple, but he can only be sure of it when the other purple eyed doesn't leave after day 1.
on day 2, both purple eyed see's each other, if there was only one purple eyed person then she would have left on day1 (coz she would have seen that everyone is red , therefore she is purple and leave), since it is day 2,there must be more than 1 purple and these two purple eyed girls sees that there is only one purple and rest are red, so they conclude that they must be purple and leave.
so every purple eyed girl leaves at $p^{th}$ day.

$jk = cn+1$

$rem(a\cdot b,c) \equiv a\cdot b (mod\ c) \equiv rem(a,c)\cdot rem(b,c) (mod\ c) = rem((rem(a,c)\cdot rem(b,c)),c)$ 
this principle extends to an arbitary number of factors, such that :

$a_1\cdot a_2\cdot \ldots a_n \equiv rem(a_1,c)\cdot rem(a_2,c)\cdot \ldots rem(a_n,c)\ (mod\ c)$

*eg. find rem(23x61x19,17)*

###### cardinality rules : 

*bijection rule* - *if there is a bijection* $\mathcal{f} : A\to B$  between $A$ and $B$ , then $\lvert A\rvert = \lvert B\rvert$ .

star-bar

*product rule* - *if* $p_1,p_2,\ldots,p_n$ are sets then : $\lvert p_1\times p_2\times \ldots \times p_n\rvert = \lvert p_1 \rvert \cdot \lvert p_2 \rvert \dots \cdot \lvert p_n \rvert$

*sum rule* - *if* $a_1,a_2,\ldots a_n$ *are* **disjoint** *sets then* : $\lvert a_1 \cup a_2\cup \ldots \cup a_n \rvert = \lvert a_1 \rvert + \lvert a_2 \rvert + \ldots \lvert a_n \rvert$  

*generalized product rule* - 

*Let S be a set of length-k sequences. if there are :* 

- $n_1$ *possible first entries,*
- $n_2$ *possible second entries for each first entry*.
- $n_3$ *possisble third entries for each combination of first and second entries,etc.*

*then :*

$\lvert S \rvert = n_1\cdot n_2\cdot n_3 \ldots n_k$

*a **permutation** of a set S is a sequence that contains every element of S exactly once.*

a *k-to-1 function* maps exactly *k* elements of domain to every element of codomain.

*division rule* - *if $\mathcal{f} : A \to B$ is k-to-1, then $\lvert A\rvert = k\cdot \lvert B\rvert$

by division rule, number of circular seating arrangements is *(n-1)!* .
bcoz there is a *n-to-1* mapping from set of all permutations to set of circular arrangements.

$\binom{n}{k}$ ::= the number of *k*-element subsets of an *n*-element set. "*n choose k*"

$\binom{n}{k} = \frac{n!}{k!\cdot (n-k)!}$ ,     (k)(n-k) intuition

*subset split rule* -  *The number of $(k_1,k_2,\ldots k_m)$*-splits of an *n-element* set is :

$\binom{n}{k_1,\ldots,k_m} = \frac{n!}{k_1!,k_2!,\ldots,k_m!}$

**The Binomial Theorem** -

A *binomial* is a sum of two terms, such as *a + b*. now consider its 4th power, 

$(a+b)^4 = aaaa + aaab + aaba + \ldots + bbbb$

there is one term for every sequence of a's and b's. so there are $2^4$ terms.
the number of terms with *k* copies of *b* and *n-k* copies of *a* is $\binom{n}{k}$.
the coefficient of $a^{n-k} b^k$ is $\binom{n}{k}$ 

for n=4,

$(a+b)^4 = \binom{4}{0}\cdot a^4b^0 +\binom{4}{1}\cdot a^3b^1 +\binom{4}{2}\cdot a^2b^2 +\binom{4}{3}\cdot a^1b^3 +\binom{4}{4}\cdot a^0b^4$

*binomial theorem* - *For all n $\in \mathbb{N}$ and a, b$\in \mathbb{R}$ :*

$(a + b)^n = \sum_{k=0}^{n}\binom{n}{k}a^{n-k}b^{k}$ 

*for $n,k_1,\ldots,k_m \in \mathbb{N}$, such that $k_1 + k_2 +\ldots + k_m = n$ , define the multinomial coefficient

$\binom{n}{k_1,k_2,\ldots,k_m} = \frac{n!}{k_1!,k_2!,\ldots,k_m!}$ 


group together elements and count when necessary.

*inclusion-exclusion* - 

$\lvert S_1 \cup S_2 \cup \ldots \cup S_n\rvert =$

sum of sizes of all sets  -*minus*   sum of sizes of all two-way intersections  +*plus*   sum of sizes of all three-way intersections   -*minus*  sum of sizes of all four-way intersections ...

sign  of the *k-way* intersections $= (-1)^{k-1}$

you can use inclusion-exclusion principle to calculate euler's totient function $\phi$ , by calculating number of elements that are not co-prime with $n$ and subtracting then with total numbers.

eg. $\phi(175)$ , prime factorization $175 = 5*5*7$ , so the numbers which divides 175 must be multiple of $5$ and $7$ . so, the numbers that are not co-prime to 175 would be 

$= \lfloor \frac{175}{5}\rfloor +\lfloor \frac{175}{7}\rfloor - \lfloor \frac{175}{5*7}\rfloor = 55$

$\phi(175) = 175-55 = 120$

$\phi(n) = n\cdot\prod_{i=1}^{m}(1-\frac{1}{p_i})$  , m is the number of distinct prime factors of n.


*pascal identity* - 

$\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$ 

first part, if x is selected in team then choose k-1 from remaining n-1
second, if x is not selected then choose k from remaining n-1


$\binom{n}{r}\binom{2n}{n-r} = \binom{3n}{n}$ 

total 3n cards , 2n black and n reds, choose n cards from 3n.
choose r from n red-cards and n-r from 2n black cards.


***pigeonhole principle*** - 

*if $\lvert X\rvert > \lvert Y\rvert$, then for every total function $f:X\to Y$ , there exists two different elements of X that are mapped to the same element of Y.

*"if there are more pigeons than holes they occupy, then at least two pigeons must be in the same hole."*

*if $\lvert X\rvert > k\cdot\lvert  Y\rvert$, then every total function $f:X\to Y$ maps at least k+1 different elements of X to the same element of Y*.

###### generating functions -

generating functions transform problems about *sequences* into problems about *functions*.

the ordinary generating function for the sequence $<g_0,g_1,g_2,\ldots>$ is the power series,

$G(x) = g_0 + g_1\cdot x^1 + g_2\cdot x^2 + g_3\cdot x^3\ldots$



##### Probability -

use basic steps

the *sample space* for an experiment is the set of all possible outcomes.

an *outcome* also known as a *sample point* , consists of all the information about the experiment after it has been performed including values of all random choices. 

*monty hall problem* , 

use tree method to construct sample space for the case when the player switch doors.

*probability space* consists of a sample space $S$ and a probability function $P_r$ , that maps each sample point to a real number.

$P_r : S\to \mathbb{R}$  , such that

1. $\forall \omega \in S, 0\leq P(\omega)\leq 1$ 
2. $\sum_{\omega \in P} P_r(\omega) = 1$ 

*interpretation* , $\forall \omega \in S , P(\omega) =$ probability that $\omega$ will be the outcome.

the probability of a sample point is the product of the probabilities on the path of the tree leading to the sample point.

*event* , is a subset of the sample space.
the probability that a event $E$  occurs is $\sum_{\omega\in E} P_r(\omega)$ , sum of probabilities of all the sample points in the event.

$\mathcal{P}(E) = \frac{|E|}{|\mathcal{S}|} = \frac{no. of samples in event}{size of sample space}$ , this is only true when all outcomes in the event are equally likely, *i.e* event is uniform. 

*conditional probability* , 

$P_r(A|B) =$ probability of event $A$ given that event $B$ occured. 

if $P_r(B) \neq 0 ,\ \ P_r(A | B) = \frac{P_r(A \cap B)}{P_r(B)}$  , get intuition via venn diagram

if $A$ and $B$ are disjoint, then $P(A|B) = 0$ , bcoz $A\cap B = \phi$ , this means that if $B$ occured , then we can be sure that $A$ didn't occured , so $P(A) = 0$ .

![[conditional_probability.png]]

*product rule* ,

$P(A\cap B) = P(B)\cdot P(A | B)$  , probability of $A$ and $B$ occuring simultaneously.

think of it as a path in tree, probability that B occurs then probability that A occurs given that B occured.

*general product rule*,
$P_r(A_1 \cap A_2 \cap A_3 \ldots \cap A_n) = P(A_1)\cdot P(A_2 | A_1)\cdot P(A_3 | A_1\cap A_2)\ldots P(A_n | A_1\cap A_2\cap\ldots A_{n-1})$
so the leaves of the tree, gives us the probability that event $a_1,a_2,\ldots...a_n$(consider them as edge labels on a path to leaf) occured simultaneously.

*the four step method* - 

1. find the sample space (use tree method)
2. define events of interest
3. determine outcome probabilities
	1. assign edge probabilities
	2. compute outcome probabilites
4. compute event probabilities

**Q.** find the probability that you win the series given that you won the first game.
**Q.** probability that the series last 3 games.

$P(A|B)$  & $P(B|A)$  may or may not be same.



