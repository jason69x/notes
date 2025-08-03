*reference books* : CLRS,

*algorithm* is a well-defined computational procedure which takes a value or set of values as input and produces a value or set of values as output in a finite amount of time.

problem specifies the desired input/output relationship for a problem instance and a algorithm provides a computational procedure to achieve that input/output relationship in all problem instances.


A *decision problem* asks for a yes/no answer or determines whether a solution exists for a given input.

An *optimization problem* seeks the best solution from a set of feasible solutions, often minimizing or maximizing a specific objective.


***insertion sort*** - 

sorting algorithm similar to how we sort a deck of cards. suppose we have a unsorted deck of cards on a table. we pick one card from our righthand and arrange it at the correct position in our left hand by checking it against cards from right to left direction in our left handand shifting the cards to the right till we find the correct location where we can place it.

elements of array need not to be swapped with key, they need to be shifted right and make room for the key.

***loop invariants*** - 

they help us understand why an algorithm is correct. when using loop invariants you need to show three things:

* *initializaiton* : it is true prior to the first iteration of the loop.
* *maintenance* : if it is true before an iteration of the loop, it remains true before the next iteration.
* *termination* : when loop terminates, the invariant along with the reason that loop terminated gives us a useful property that help show that the algorithm is correct.

$\Theta(n^2)$ means "*roughly proportional to $n^2$ when n is large*".

###### divide and conquer method :

break the problem into several subproblems that are similar to the original problem but smaller in size, solve the subproblems recursively, and then combine these solutions to create a solution to the original problem.

$T(n) = aT(\frac{n}{b}) + d + c$

running time complexity of a algorithm can be describes as above,
where $a$ is the number of subproblems the original problem is divided into.
$n/b$ size of each subproblem
$d$ is the dividing cost
$c$ is the combining cost

it takes $T(\frac{n}{b})$ time to solve one subproblem , so to solve $a$ such problems it takes $aT(\frac{n}{b})$ time.

**// todo page 44 exercises

###### characterizing running times

$O$-notation characterizes a *upper bound* on the asymptotic behavior of a function. it says that the function grows *no faster* than a cetain rate , based on the highest order term .

consider a function $7n^2$ , we can write it is $O(n^2)$ , we can also write it is $O(n^3)$ , because it grows no faster than $n^3$. similarly $n^4$ and so on.

$\Omega$-notation characterizes a *lower bound* on the asymptotic behavior of a function. it says that the function grows *at least as fast* as a certain rate.

$\Theta$-notation characterizes a *tight bound*. it says that the function grows *precisely* at a certain rate.


$O(g(n))$ is a set of functions such that,

$f(n)$ : there exists positive constants $c$ and $n_0$ such that $0\leq f(n)\leq cg(n)$ for all $n\geq n_0$

$\Omega(g(n))$,

$f(n)$ : there exists positive constants $c$ and $n_0$ such that $0\leq cg(n)\leq f(n)$ for all $n\geq n_0$

$\Theta(g(n))$,

$f(n)$ : there exists positive constants $c$ and $n_0$ such that $0\leq c_1g(n)\leq f(n)\leq c_2g(n)$ for all $n\geq n_0$


$g(n)$ and $f(n)$  must be asymptotically non-negative.


For any two functions $f(n)$ and $g(n)$ , we have $f(n) = \Theta (g(n))$ if and only if
$f(n) = O(g(n))$ and $f(n)=\Omega(g(n))$.

$o(g(n))$,

$\lim_{n \to \infty}$ $\frac{f(n)}{g(n)} = 0$ , 

$\omega(g(n))$,

$\lim_{n \to \infty}$ $\frac{f(n)}{g(n)} = \infty$ , 


Asymptotic Analysis ,    Look at growth of $T(n)$ as $\lim{n \to \inf}$ 

a set in a formula represents a anonymous function in that set.

*masters method* work on recurrences in the form :

$T(n) = aT(\frac{n}{b}) + f(n)$ 

we compare $f(n)$ with $n^{\log_b{a}}$ 

here $n^{\log_b{a}}$ represent the number of leaves in the recursion tree.

if we divide a problem by $b$ on every level than total levels would be $\log_b{n}$ .

at the 1st level we have $a$ problems i.e $a^1$
at the 2nd level we have $a^2$ nodes

at the last level we have $a^h$ leaves, where $h$ is the height of the tree or no. of levels in the tree.

$a^h \implies a^{\log_b{n}} \implies n^{\log_b{a}}$ 

if we sum the terms at each level we get a geometric progression,

$f(n) + af(n/b) + a^2f(n/b^2) \ldots +\Theta(n^{\log_b{a}})$

we can find time complexity by checking how the cost decreases $f(n)$ / increases $\Theta(n^{\log_b{a}})$ geometrically and if the cost remains fairly similar in all levels then the cost becomes $f(n)*\log_b{n}$ , because cost at each level is $f(n)$ and height of the tree is $\log_b{n}$.

a *recurrence* is an equation that describes a function in terms of its value on other, typically smaller,arguments.


$3T(n/4) + n^2$ 

in recursion tree method, $n^2$ is the root node. it represents the cost incurred at this level. for the next level, cost of the $n/4$ subproblems  will be $(n/4)^2$ .