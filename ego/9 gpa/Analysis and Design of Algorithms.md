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

*masters method* work on recurrences of the form :

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



when we say spanning tree of a graph we specifically mean - 
* one connected tree
* that includes all the vertices of the graph
* has no cycles 
* for a graph with n vertices, spanning tree always has n-1 edges

###### Dynamic Programming : 

dynamic programming solves problems by combining the solutions to subproblems.
dynamic programming applies when the subproblems overlap - that is, when subproblems share subsubproblems. dp solves each subsubproblem just once and saves its answer in a table.

dynamic programming typically applies to *optimization problems*. such problems can have many possible solutions. each solution has a value, and you want to find a solution with the optimal (minimum or maximum) value.

*rod cutting* - 

number of ways to cut a rod of length *n*  = $2^{n-1}$ 



###### Heaps :

**Max-heapify** - assumes left subtree(i) and right subtree (i) are max heaps, then it compares value of i node with its children and if it is less than it swaps it with the larger value and perform max heapify recursively on that child.

**Build-Max-Heap** - converts an array $A=[1\ldots n]$  into a max-heap by
calling **Max-heapify** in a bottom-up manner. the elements in the subarray $A=[n/2\ldots n]$  are all leaves of the tree, and so each is a 1-element heap to begin with. **Build-Max-Heap** goes through the remaining nodes of the tree and runs **Max-heapify** on each one.

**height h** can have at most $\lceil n / 2^{h+1} \rceil$ nodes . bcoz height zero (leaf) nodes are n/2, height one are n/4 ...

$\sum_{h=0}^{\lfloor\log n\rfloor} \lceil \frac{n}{2^{h+1}} \rceil ch = O(n)$ , here we are summing cost of each node at height h. that's why we 

are multiplying no. of nodes at height h with cost each node can take that is h and we are doing it for all heights till height of root.

**heap-sort** -  perform build-max-heap on given array, then exchanges root of heap with last element of array, decreases heap size and performs max-heapify on the new root, does this till heap size is 0. $O(n\cdot\log n)$ bcoz for each $n$ elements we call heapify $\log n$.

A **priority queue** is a data structure for maintaining a set S of elements, each with an associated value called a key. insert(),extract-max(),max(),increase-key().

**lower bound - binary search $\Omega(\log n)$ ,**

think of binary search as a comparison based decision tree
- each node is a comparison ( "target <,>,= middle")
- leaves represents final answers (either "found at i" or "not found")

there are $n$ possible outcomes and $1$ for not found , so total $n+1$ outcomes.
a binary tree of height $h$ has at most $2^h$ leaves.
to represent $n+1$ outcomes, $2^h \geq n+1 \implies h \geq \log_2(n+1)$

###### greedy algorithms - 

makes a locally optimal choice in the hope that this choice leads to a globally optimal solution.

*interval scheduling problem* - 

Intuition suggests that you should choose an activity that leaves the resource available for as many other activities as possible. therefore , choose the activity in $S$ with the earliest finish time.

Lets call the greedy choice $G = a_1$ (finishing at time $f_1$)
let $O={o_1,o_2,...o_k}$ be an optimal solution. activities in $O$ are odered by finish times.

two cases : 
- if $a_1 \in O$ , then our greedy choice aligns with optimal.
- if $a_1 \notin O$, then since our $a_1$ has the earliest finish time it must finish no later than $o_1$. replace $o_1$ with $a_1$ in $O$ , the new set $O'$ is still compatible and has the same cardinality has $O$. therefore, the greedy choice does not reduce optimality.

*earliest finish time first*, leaves as much *future space* as possible.
*latest start time first* , leaves as much *past space* as possible.
both ensure you're not blocking a larger solution.

*interval partitioning problem* - 

schedule *all* requests using as few resources as possible.
in any instance of interval partitioning, the number of resources needed is at least the depth of the set of intervals. *depth* - maximum number of overlapping partitions over a common point.

sort by start times and assign intervals to class whose last finish time is less that this intervals start time.

if you can't reuse a classroom, you can't reuse any other (since they all finish later).

you need to specify underlying container when creating a priority_queue in cpp.

`priority_queue<int,vector<int>,greater<int>> q;`

*scheduling with deadlines* - 

sort by earliest deadlines, use max-heap to remove longer duration courses with shorter ones to maximize number of coures selected. push durations in max-heap. 

*divide and conquer* - optimal substructure + independent subproblems
*dynamic programming* - optimal substructure + overlapping subproblems
*greedy algorithms* - optimal substructure + local optimal leads to global optimal.

**elements of greedy strategy -**

- determine the optimal substructure of the problem. *optimal substructure* is a property of a problem that say : an optimal solution to the whole problem can be constructed from optimal solutions to its subproblems.
- show that if you make the greedy choice, only one subproblem remains.
- prove that it is safe to make the greedy choice.

*weighted interval partitioning* -

given *n* intervals $[s_i,f_i]$ , each with **weight** $w_i$ .
select a subset of non-overlapping intervals that maximized the total weight.

- sort intervals by finish time
- for each interval $i$ , 
	- Take it $\to$ add its weight $w_i$ + best solution from the last non-conflicting interval before $i$
	- Skip it $\to$ just take the best solution upto $i-1$.

$DP[i] = max(DP[i-1],w_i + DP[P(i)])$

$P(i)$ = rightmost interval $x$ such that  $f_x < s_i$
compute $P(i)$ using binary search after sorting by finish times.


greedy works on fractional knapsack ( sort by profit/weight ) , dp works on 0-1 knapsack. why tho??

//todo coin problems 

***Huffman encoding*** - 

DEFLATE = LZ77 (find repeats) + Huffman (encode efficiently).

LZ77 stores "abcabcabc" as "abc" then offset to "abc" followed by length of string to copy.

*prefix-free codes* - no codeword is a prefix of some other codeword.

optimal code is a *full* binary tree.
$C$ leaves and $C-1$ internal nodes. every character appears on the leaves.

for every character $c \in C$, its depth is also the length of its codeword.

thus, no. of bits required to encode a file is ,

$B(T) = \sum_{c\in C} c.freq\cdot d_T(c)$ ,


working of huffman encoding - 

create a min heap priority queue with frequency of characters as key and character as value.
pop two values from heap x,y and create a new tree node z  with x,y as its left and right child.
z.freq = x.freq + y.freq , push this z into the heap. repeat this process $n-1$ times and then pop and return the last element from the heap. this element is the root of the prefix tree. 
$O(n\cdot \log n)$ 

it maybe inefficient for small number of characters , bcoz you have to also store the prefix tree and dictionary, that will take extra space.

total costs of the tree (in bits) constructed is equal to the sum of the costs of its mergers (internal nodes).
root represents number of characters in the data.

*proving prefix-free property* -

huffman algorithm builds a full binary tree where each leaf corresponds to a unique symbol. suppose code for symbol $a$ is prefix of code for $b$. then the path to $a$ would be a prefix to the path to $b$, implying that the leaf for $a$ is an internal node on the path to $b$. but leaves have no children, so this is impossible.

*proving optimal prefix-free codes have the greedy property* - 

the idea of the proof is to take the tree $T$ representing an arbitrary optimal prefix-free code and modify it to make a tree representing another optimal prefix-free code such that the characters $x$ and $y$ appear as sibling leaves of maximum depth in the new tree. in such a tree, the codewords for $x$ and $y$ have the same length and differ only in the last bit.

consider two characters $a$ and $b$ at leaves with maximum depth in tree $T$ and $x,y$ be any where in the tree. since $x,y$ are two characters with least frequencies we have $x \leq y$ and since $a,b$ are at leaves with max depth $a\leq b$ , assume $x\leq a$ and $y\leq b$ . we can create a new tree $T'$ by exchanging $x$ and $a$. now we have to prove that total cost of this new tree is no more than $T$.
$B(T) - B(T') \geq 0$ , similarly, by exchangin $y,b$ we get $T''$ and this has cost $\leq B(T')$ . but since we assumed $T$ tree to be optimal , tree $T' , T''$ cannot have cost less than $T$ . therefore, these trees are also optimal with $x,y$ appearing as sibling at leaves with maximum depth.

*proving optimal substructure property for huffman prefix-free trees* - 

if we create a new tree with $x,y$ removed a new node $z$ with freq. $x.freq + y.freq$ and make $x,y$ its children, then this represents a optimal tree for the original set of characters. 

$x.freq\cdot d(x) + y.freq\cdot d(y) = z.freq\cdot d(z) + x.freq + y.freq$

$B(T') = B(T) - x.freq - y.freq$ 

assuming $T$ is optimal for $C$.
if we merge $x,y$ into $z$, the resulting $T'$ must also be optimal for $C'$.
Otherwise, if there were a better tree $T''$ for $C'$ , expanding $z$ back into $x,y$ would give a better tree for $C$ than $T$.
This contradicts $T$ being optimal.
so $T'$ must be optimal.

*proof interval partitioning* - 

depth = lower bound on resources.
greedy never uses more resources than depth. greedy always picks a free resources first if available. so at any time , the number of resources = number of overlapping intervals at that time.

sorting by *start time* ensures that you never waste a resource on a later interval if an earlier one could have used it.

counter example for sorting by finish time asc. =  [1,4],[3,7],[5,6],[0,2]
because greedy always chooses resource with least finish time till now.
coz it assigns early finish time firsts and this might make resources that finish later but start early use more resources.

*proof by contradiction* - 

suppose the greedy algorithms uses $m>d$ resources. let $i$ be the first interval that causes the creation of $d+1$-th resource. this means that all resources are busy and have intervals assigned to them that finishes after $s_i$ . these intervals must have started on or before $s_i$ , so each of these intervals are overlapping with $i$ at time $s_i$ , this gives $d+1$ intervals conflicting at any time and this **contradicts** with the fact that we assumed $d$ as the maximum overlap.




