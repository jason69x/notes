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

if don't consider these two VMs equal, it is known as *bisimilarity*

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