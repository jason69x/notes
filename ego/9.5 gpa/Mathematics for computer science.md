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

***Theorem 2.4.1*** -  *For any nonnegative integer, n, the set of integers greater than or equal to n is well ordered.*  \\\todo

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

