CSP - communicating sequential processes

csp is a notation for describing concurrent systems(ones where there is more than one process making progress at the same time and are communicating with each other. these processes might be running on separate processors or time-sharing on a single cpu.

these systems have number of separate components which need to communicate with each other. eg vlsi systems with diff sub compo.
workstations sharing same file server.

in a concurrent system all the different components are in independent states. it is necessary to understand which combinations of states can arise and consequences of each.

*non-determinism*
a system exihibits non-determinism if two different copies of it may behave differently when given the exactly same inputs.

if there are three subprocesses P,Q,R. where P,Q are competing to be the first to communicate with R, which in turn bases its future behaviour upon which wins the race, then the whole system may veer one way or another that is uncontrollable and unobservable from the outside.

*deadlock*
a concurrent system is deadlocked if no component can make any progress, generally because each is waiting for communication with others.

*livelock*
divergence, where a program performs an infinite unbroken sequence of internal actions. this occurs when a network communicates infinetly internally without any component communicating externally.
user may hope eternally that some output will emerge eventually.


$\sum$ contains all possible communications for processes in the universe under consideration. think of communication as transaction or synchronization between two or more processes.

in CSP, we assume that an event only happens when all its participants are prepared to execute it. (*handshaken* communication)

###### CCS
*calculus of communicating systems*

is a process algebra, a formal language to describe concurrent systems.
is given semantics in terms of labelled transition system.

`CLOCK = tick` - defines a process CLOCK that simply executes the *action* tick and terminates
`CLOCK2 = tick.tock` - defines a process CLOCK2 that executes the action tick then tock then terminates.

*action prefixing*
`x.y.z.P` - this is a process that executes x->y->z->P, here P is a different process
`CLOCK = tick.tock.STOP`

*loops*
`CLOCK = tick.tock.CLOCK` - performs tick->tock->CLOCK , recursion

`VM = in50.outCoke.in20.outMars.VM` - no choice, alternates bw 50&coke/20mars

*choice*
non-determinism
`VM = (in50.outCoke.VM) + (in20.outMars.VM)`

`P+Q` - is a process that can either behave as process P or process Q

*choice equalities*
`P + (Q+R) = (P+Q) + R` , associativity
`P + Q = Q + P` , commutavity
`P + STOP = P` , neutral element
`P + P = P`  , idempotance

*branching time*

`VM = in50.(outCoke + outMars)` this machine allows user to choose product after inserting coin.
`VM = (in50.outCoke + in50.outMars)` the machine chooses what happens after coin is inserted. it may output mars or coke.

if we don't consider these two VMs equal, it is known as *bisimilarity*

*parallel composition*

`P | Q` - non deterministic interleaving of their actions

*synchronization*

every action `a` has an opposing coaction $\bar{a}$ 
action -> output , coaction -> input

$\tau$ , internal transition, execute both action and coaction

two parallel processes P and Q synchronize and proceed together only when one performs $a$ and the other performs $\bar{a}$.

*restriction*
force synchronization to happen and not allow certain actions to be taken alone.

a man eats every time a clock ticks
`CLOCK` = tick.CLOCK
`MAN` = $\overline{tick}$.eat.MAN
`EXAMPLE` = (MAN|CLOCK)\tick

---
###### LTS
we call a triple $\tau = (Pr,Act,\to)$ a *labeled transition system* if $Pr$ is a set of processes, $Act$ a set of actions such that $Pr\cap Act = \phi$ , and $\to\ \subseteq Pr\times Act\times Pr$ the *labeled transition relation* of $\tau$.

*traces* are finite sequences of action labels
$p\xrightarrow{\epsilon}p$ 
$p\xrightarrow{a\sigma}q$ , $\sigma\in Act^{*}$ 

$p\in Pr$ is called a *deadlock* *(process)* if $p\not\xrightarrow{a}$ for all $a\in Act$ 

*completed traces* are traces ending in deadlocks.

###### Trace Semantics

the *set of (partial) traces* of $p\in Pr$, traces(p), is defined inductively
- $\epsilon\in$ traces(p)
- if $\sigma \in$ traces(q) and $p\xrightarrow{a}q$ , then $a\sigma\in$ traces(q)

if traces(p) = traces(q), p is *trace equivalent* to q

![[traces.png|380]]

---

reasoning about all possible behaviors

non-determinism is inherent and unavoidable in concurrent systems.
eg. two threads incrementing a shared variable may yield different results based on the interleaving.

- shared memory - need mutual exclusion, prone to races,deadlocks
- message passing - no shared state, explicit communication

cache coherence problem,

modern processors use per-core caches, writes by one core may not be immediately visible to others, leads to inconsistent views of memory.

coherence property - all cores observe writes to the same location in consistent order

serializibility, 

every concurrent execution should be equivalent to some serial execution.

safety - mutual exclusion is never violated
liveness - every request is eventually served

concurrent systems are :
- interactive
- non-terminating
- composed of independently evolving agents

correctness depends on how components interact.

process calculus,
formal language for describing interacting processes
- syntax (how processes are built)
- operational semantics (how they evolve)
- behavioral equivalences (when two processes are the same)

process are not functions they are ongoing entities

CCS,

$a$ : input (receive)
$\overline{a}$ : output (send)
$\tau$ : internal (silent)

communication happens only when an input meets a matching output. the interaction itself become invisible ($\tau$).

$P ::= 0\ |\ \alpha.P\ |\ P + Q\ |\ P || Q\ |\ P\backslash L\ |\ A\ |\ P[f]$

where $A,P,Q$ are processes (or CCS expressions), $\alpha\in Actions$ and $f: Actions\to Actions$ 

- $0:$ inactive process
- $\alpha.P:$ prefix
- $P+Q:$ choice
- $P||Q:$ parallel composition
- $P\backslash Q:$ restriction
- $A:$ process constant (recursion)
- $P[f]:$ renaming

LTS

semantics given by transitions: $P\xrightarrow{\alpha}P'$ 
Nodes: process expressions
Edges: observable or internal actions

prefix rule, $\alpha.P\xrightarrow{a}P$ 
process is forced to perform its leading action. afterwards the continuation remains.

choice represents external non-determinism. The environment observes which action resolves the choice.
environment or internal non-determinism decides which branch is taken

restriction hides channels from the environment. Used to model private communication.

$A\ {\triangleq}\ a.A$ 
after performing $a$ behave like $A$ again.

renaming uniformly changes interface through which actions are observed. it does not change control structures of a process.
it preserves synchronization.

non-determinism arises from both choice and concurrency

$(a.0||\overline{a}.0)\backslash\{a\}$ 
- external observer sees only $\tau$ 
- communication becomes private

two processes are bisimilar if they can mimic each other.
step-by-step matching of actions.

$VM ::= coin.select.\overline{item}.VM$
$USER ::= \overline{coin}.\overline{select}.item.USER$

$SYSTEM = (VM\ |\ USER)\backslash\{coin,select,item\}$

effect of restriction:
- all interactions are synchronized
- system becomes closed
- only internal $(\tau)$ actions remain

https://chatgpt.com/share/69a04dd9-7958-8008-a2b4-cb7c9b3f580d

a path is a sequence: $s_0\xrightarrow{a_1}s_1\xrightarrow{a_2}\ldots$

a computation is a maximal path

paths describe executions; equivalences compare set of paths

a trace is the sequence of observable actions along a path, ignoring $\tau$ 
$a_1a_2a_3\ldots a_n$

$P := coin.(tea + coffee)$ 
$Q := coin.tea + coin.coffee$

trace equivalent, $Tr(P) = Tr(Q) = \{coin\ tea, coin\ coffee\}$
but not behaviourally equivalent

strong bisimulation

A relation $\mathcal{R}\subseteq \mathcal{S}\times \mathcal{S}$ is a strong bisimulation if:
whenever $(s,t)\in\mathcal{R}$:
- if $s\xrightarrow{a}s'$ , then $t\xrightarrow{a}t'$ with $(s',t')\in\mathcal{R}$
- and symmetrically from $t$.

step-by-step matching
observes all actions including $\tau$
preserves full branching structure

weak transitions

$s\xRightarrow{a}s'$ 
- zero or more $\tau$
- followed by action $a$
- followed by zero or more $\tau$

weak bisimulation

a relation $\mathcal{R}$ is a weak bisimulation if:
whenever $(s,t)\in\mathcal{R}$:
- if $s\xrightarrow{a}s'$ then $t\xRightarrow{a}t'$ with $(s',t')\in\mathcal{R}$
- and symmetrically for $t$.

different ATM implementations, different DB internals are weakly bisimilar not strongly.

behavioral equivalence matters for correctness of implementation, program refinement, protocol verification, model checking
equivalence justifies replacing one system by another.

LTS gives semantics to process
traces observe executions
bisimulation observes interaction structure
weak bisimulation abstracts from internals


computability, what problems can be solved at all
complexity theory, how efficiently problems can be solved

a problem is a mathematical object: $L\subseteq\Sigma^{*}$

a program/algorithm is a procedure that solves the problem.
complexity is a property of the problem.

Turing Machine

$M = (Q,\Sigma,\vdash,\delta,q_0,q_{acc},q_{rej})$

$Q$: finite set of states
$\Sigma$: input alphabet
$\vdash$: tape alphabet
$\delta$: $Q\times\vdash\ \to\ Q\times\vdash\times\{L,R\}$
$q_0$: start state
$q_{acc},q_{rej}$: halting states

tape corresponds to memory
head corresponds to instruction pointer
states represents control flow
transition relation models instruction execution

halting problem,

given a program and an input, will the program eventually halt or run forever?

model of computation: turing machines
input: a turing machine $M$ and a string $\omega$
question: does $M$ halt on input $\omega$?

$HALT =\ \{\ \langle M,\omega\rangle\ |\ M\ halts\ on\ input\ \omega \ \}$

decision problem, does $\langle M,\omega\rangle\in HALT$ ?

decidability

a language $L$ is decidable if there exists a turing machine $D$ st:
- $D$ halts on all inputs
- $D$ accepts exactly the string in $L$

determine whether $HALT$ is decidable.

Theorem

The Halting Problem is Undecidable.

proof by contradiction,
- assume HALT is decidable
- construct a paradoxical turing machine
- derive a contradiction
- conclue HALT is undecidable

Assume there exists a Turing Machine $H(\langle M,\omega\rangle)=$
accept if $M$ halts on $\omega$
reject otherwise

- $H$ halts on all inputs
- $H$ correctly decides $HALT$

define a turing machine $D$ as follows:
input: $\langle M\rangle$
- run $H$ on input $\langle M,M\rangle$
- if $H$ accepts, loop forever
- if $H$ rejects, halt and accept

Machine $D$:
- uses $H$ to predict its own behavior
- then deliberately does the opposite

this creates a self-referential paradox

critical question,
what happens when $D$ is run on its own description? $D(\langle D\rangle)$

case 1: $H(\langle D\rangle)$ accepts
- $H$ predicts that $D$ halts on input $\langle D\rangle$
- by definition of $D$, it loops forever
- contradiction: $D$ does not halt

case 2: $H(\langle D\rangle)$ rejects
- $H$ predicts that $D$ does not halt on input $\langle D\rangle$
- by definition of $D$, it halt and accepts.
- contradiction: $D$ halts

in both cases $H$ gives incorrect answer, this contradicts the assumption that $H$ is correct.

therefore, such a machine $H$ cannot exist.

$HALT$ is undecidable.

many programs do halt, but no algorithm decides all

time complexity of a turing machine, depends only on input length, max number of steps before halting on x.


a complexity class is a set of problems solvable within given resource bounds.

$P = \{L\ |\ L\ decidable\ in\ polynomial\ time\}$
$NP = \{L\ |\ L\ has\ polynomial\ time\ verifiable\ certificates\}$
nondeterministic polynomial time

$P\subseteq NP\subseteq PSPACE\subseteq EXPTIME\subseteq EXPSPACE$

savitch's theorem, $PSPACE = NPSPACE$
space is more powerful than time

not everything computable is efficient, and not everything definable is computable.


$\pi$-calculus

Definition. Processes $P,Q$ are built from a set of names $x,y,u,v\in\mathcal{N}$ representing communication channels.

$P,Q ::= 0$   Nil: inactive process
$\overline{x}\langle y\rangle.P$     Output: Send name $y$ on channel $x$, then $P$
$x(z).P$     Input: Receive on $x$, bind to $z$ in $P$
$P\ |\ Q$      Parallel: Concurrent execution
$(\mathcal{v}x)P$      Restriction: local name $x$ in $P$
$!P$        Replication: Persistent service/ Recursion
$P + Q$     Choice: Non-deterministic sum

$\overline{a}\langle b\rangle.0\ |\ a(x).\overline{x}\langle c\rangle.0$

in $\pi$-calculus, names are values.
- Input $x(z).P:$ Acts as a binder for $z$. $x$ is the address.
- Ouput $\overline{x}\langle y\rangle.P:$ Message $y$ is sent to address $x$.
- Restriction $(vx)P:$ Creates a private wire. only processes inside $P$ can see $x$.
- Replication $!P:$ Equivalent to $P\ |\ P\ |\ P\ |\ldots$

because names can be communicated, network topology can change during execution. this is mobility.

![[ccs_vs_pi.png]]

structural congruence $\equiv$ identifies processes that are semantically identical regardless of their syntactic representation.
cleans up the system.


$(P,|,0)$ and $(P,+,0)$ are symmetric monoids.
- commutativity: $P\ |\ Q\equiv Q\ |\ P$
- Associativity: $(P\ |\ Q)\ |\ R \equiv P\ |\ (Q\ |\ R)$
- Identity: $P\ |\ 0\equiv P$  

alpha-conversion
bound names can be renamed: $a(x).\overline{x}\langle y\rangle\equiv a(z).\overline z\langle y\rangle$



