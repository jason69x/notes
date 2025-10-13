*a distributed system is a collection of independent computers that appears to its users as a single coherent system*

###### Synchronization -

modern computers use laser cutted quartz crystals that resonate at particular frequency to calculate time and synchronize with NTP/GPS servers to get more precision bcoz quartz time may drift. computers have two kind of time : 
1. wall-clock time
		comes from RTC - real time clock chip (quartz crystal inside) + corrections from NTP/GPS
		can jump forward or backward when corrected
2. monotonic time
		a strictly increasing time used for measuring durations
		based on CPU times like TSC - time stamp counter 64 bit, HPET
		never goes backward no matter what happens to wall-clock time
		counts ticks since boot

RTC chip stores current time and date , os reads on boot

leap seconds, on 30 june and 31 december at 23:59:59 UTC , clock will either add/subtract 1 second or do nothing.
unix time
smearing,strata

![[ntp.png]]


slew - converge , step - jump

solar day, interval between two consecutive transit (event of sun's reaching its highest apparent point in the sky)

earth rotation is slowing down. 300 million years ago earth took ~400 days to revolve around the sun.

TAI time is 3ms behind solar time. if it remains unsynced we will have afternoon in the morning time. that why leap seconds were introduced. UTC = TAI + leap seconds

stratum levels

berkeley unix algorithm, time daemon ask polls every machine from time to time to ask what time it is there. based on the answers it calculates average of their time and tell all the other machines to advance or slow their clocks to the new time. suitable for system with no UTC receiver. time daemons time must be set manually by the operator periodically. internal clock synchronization algorithm.

---
##### Chp.3 Processes -

scalable system - increasing amount of work done by adding more resources.
vertical scalability, horizontal scalability

interrupts may  cause a considerable, and relatively long-lasting reorganization of the cache,in turn, affecting the overall performance of an application.

user-level threads(go routines)->mapped by go runtime to -> os threads (kernel threads) -> mapped by os kernel to -> physical threads (8c/16t).

go runtime creates kernel threads by using system calls and then schedules go routines on these threads. scheduling,synchronization,context switching between go routines is handled in user space without sys calls.

*TLP - thread level parallelism* value

Threads make it possible to retain the idea of sequential processes that make blocking system calls and still achieve parallelism.

*types of virtual machines :* 
1.  process virtual machine - runtime system (provides abstract instruction set to execute applications eg. jrt)
2.  native virtual machine monitor - installed on top of hardware (need to implement device drivers)
3.  hosted virtual machine monitor - installed on top of existing operating system

virtual machines offers a means to run applications relying on a specific operating environment.

a *container* can be thought of a collection of binaries (also called images) that jointly constitute the software environment for running applications.

1. *namespaces* , by which a collection of processes associated with a container is given their own view of identifiers and such, independent of other containers.
2. *Union file system* , which allows to combine several file systems into a layered fashion with only the highest layer allowing for write operations.
3. *control groups* - cgroups, by which resource restriction can be imposed upon a collection of processes.

base image, specific images

*kubernetes* is an open source system for automating deployment, scaling, and management of containerized applications.

*amazon elastic compute cloud - ec2* , lets you rent virtual servers (instances) in the cloud.

*amazon simple storage service - s3*

*thin-client approach* - everything is processed and stored at the server.

every browser tab gets its own renderer process.


###### Chapter 5 - Coordination

In distributed systems, multiple processes might need access to the **same shared resource**.
**Mutual Exclusion** means making sure **only one process at a time** can use that resource.
in a single computer, that's easy with locks or semaphores. But in distributed systems : 

- There's no **single global clock**.
- Processes communicate **only by messages**.
- Failures can happen.

so ,we need algorithms (like Ricart-Agrawala, Lamport's algo, token-based algo) that coordinate across the network.

*Location system and Multi-dimensional plane*, imagine you have a huge distributed system (like cloud servers all around the world). you need to know **where processes and data** are located in a way that is efficient. A *location system* gives  you a kind of "coordinate" for each process. its like giving every process a coordinate in a giant virtual map. that way, when a process wants something , it can navigate the map instead of searching everywhere blindly.

if there was a **global clock** , we can use timestamp protocol for mutual exclusion, process with lowest timestamp gets the resources, other waits in a queue.

*gossip-based coordination* - in large distributed systems, you can't have every node talk to every other node directly - thats too expensive. instead, they use gossip protocols.
- each node periodically exchanges information with a few random peers.
- over time, the information spreads to everyone, probabilistically.


*reference broadcast synchronization (RBS)* - 

this protocol does not assume that there is a single node with an accurate account of time. instead of aiming to provide all nodes UTC time, it aims at merely internally synchronizing the clocks.
In RBS, a sender broadcasts a reference message that allows its receivers to adjust their clocks.

google TrueTime, TT.now() , TT.after() , TT.before() 
time-master, time-slave

**logical clocks** :

what usually matters is not that all processes agree on exactly what time it is, but rather they agree on the *order in which events occur*.

*lamport's logical clocks* - 

to synchronize logical clocks, lamport defined a relation called *happens-before*. $a\to b$

each message carries the sending time according to the sender's clock.
when a message arrives and the reciever's clock shows a value before the time the message was sent, the receiver fast forwards its clock to be one more than the sending time.

to implement lamport's logical clocks, each process $p_i$ maintains a counter $c_i$.
counters are updated according to the following steps : 

- before executing a event (sending a msg over network or delivering a msg to app. or some other internal event) $p_i$ increments $c_i + 1 \to c_i$ .
- when process $p_i$ sends a message $m$ to process $p_j$ it sets $m$'s timestamp *ts(m)* = $c_i$ , after executing the previous step.
- upon receiving message $m$ process $p_j$ sets its counter to $c_j \gets max(ts(m),c_j)$ , after which it then executes the first step and delivers message to the application.

we also use process id along with  counter values to create a timestamp to break ties in case if two events have same timestamp. <40,i> .

*totally ordered multicast* - a multicast operation by which all messages are delivered in the same order to each receiver. lamport's logical clocks can be used to implement totally ordered multicast in a completely distributed fashion.

each message is always timestamped with the current logical time of the sender. when a process receives a message, it is put into a queue, ordered according to its timestamp. the receiver multicast acknowledgement to other processes. a process can deliver a queued message to the application it is running only when that message is at the head of the queue and has been acknowledged by each other process.

totally ordered multicasting is an important vehicle for replicated services,

**Multicast** - 

multicast is a way of sending data from *one sender to multiple receivers* at the same time, but without flooding everyone on the network.
instead of sending data to every host or just one host, the sender targets a multicast group address.
hosts that want to receive multicast traffic "subscribe" to a group using protocols like IGMP.
routers then know which hosts are interested.
routers use multicast routing protocols (eg. PIM) to figure out the best way to deliver the data only to networks where there are subscribers, they build a multicast distribution tree.
routers replicate and forwards packets only where necessary.

distributed algorithm complexity is usually measured in **number of messages exchanged** , not local computation. these computation are negligible compared to **network message latency**.


casuality can be captured by **vector clocks** , which are constructed by letting each process $p_i$ maintain a vector $VC_i$ with the following properties :

- $VC_i[i]$ is the number of events that have occured so far at $p_i$ . in other words, $VC_i[i]$ is the local logical clock at process $p_i$ .
- if $VC_i[j] = k$ then $p_i$ know that *k* events have occured at $p_j$ . it is thus $p_i$'s knowledge of the local time at $p_j$ .

the first property is maintained by incrementing $VC_i[i]$ at the occurence of each new event that happens at process $p_i$ . the second property is maintained by piggybacking vectors along with the messages that are sent.

- before executing an event , $p_i$ executes $VC_i[i]\gets VC_i[i]+1$ .
- when process $p_i$ sends a message *m* to $p_j$ , it sets *m's* (vector) timestamp *ts(m)* equal to $VC_i$ after executing the previous step.
- upon the receipt of a message *m* , process $p_j$ adjusts its own vector by setting $VC_j[k] \gets max\{\ VC_j[k]\ ,\ ts(m)[k]\ \}$ , after which it executes the first step and delivers message to the application.

***Mutual Exclusion in distributed system*** - 

1. token-based solutions
2. permission-based approach

in token-based solutions mutual exclusion is achieved by passing a special message between the processes, known as *token*. there is only one token available , and who ever has that token is allowed to access the shared resource.

a centralized algorithm, one process is elected as the coordinator, processes request to coordinator to get access/release resources. uses a queue for waiting processes.
single coordinator may be a bottleneck and single point of failure.

**Ricart and agrawala** -

when a process wants to access a resource it sends a message containing resource name, process id, logical time to all the processes.
when a process receives a request message , the action it takes depends on its own state.

- if the receiver is not accessing the resource and does not want to, it sends OK msg back to the sender.
- if receiver already has access to the resource, it queues the request.
- if receiver wants to access the resource but havent got yet (i.e it has sent request to everyone), it compares the timestamp of the incoming message with the one contained in the message it has sent everyone. if incoming msg has lower *ts* it sends OK else it queues the request.

a process can use resource if every other process has given permission.

mutual exclusion is guaranteed without deadlock or starvation.

if total number of processes is N , a process need to send $2\cdot (N-1)$ number of messages before it can enter its critical section.

when we talk about *message  complexity* , we always count every message that crosses the network - both sent and received . network bandwidth is consumed no matter whether its a reply or a request.

this algo has N points of failure, if one processes crashes it will fail to respond to requests. we can overcome some difficulties by timeouts and reply messages. each process must respond back either grant or deny permission, if reply or request packet is lost the sender resends request after a timeout or conclude that destination is unreachable. sender should block after recieving deny.

**voting algorithm** - 

each resource is assumed to be replicated N times. Every replica has its own coordinator for controlling access by concurrent processes.
when a process wants to access a resource, it will simply need to get a majority vote from $m > N/2$ coordinators.

let $p = \delta t/T$  be the probability that a coordinator resets during a time interval $\delta t$ , while having a lifetime of $T$ .  the probability $\mathbb{P}[k]$ that *k* out of *m* coordinators resets during the same interval is :
$\ \ \ \ \mathbb{P}[k] = \binom{m}{k}\cdot p^k\cdot (1-p)^{m-k}$

less resource utilization problem arises when there are too many processes wants to access a shared resource, no one can get enough votes, leaving the resource unused.

delays can be expressed in MTTU - message transfer time unit

- it takes only two MTTU's to enter a critical region in the centralized case, caused by a request message and subsequent grant message by the coordinator. after that a release message is required to release the resource.
- the distributed algorithm required sending $N-1$ request messages, and receiving $N-1$ grant messages, adding up to $2\cdot(N-1)$ MTTU's.
- for the token ring, the delay varies from $0$ to $N-1$ MTTU's
- in case of decentralized, the delay is dependent on the number of times a process need to return the minority votes. with having to go through $k\geq1$ attempts, a process may see $2kN + (k-1)N/2$  MTTU's . second part of this expression, is when a process fails, n/2 because if a process doesn't get majority votes that means on avg. half of the coordinators are busy with other processes and you need to release the minority votes.

**zookeeper** provides a heirarchical namespace (like unix file system). implementing a lock using zookeeper can we done by creating a /lock node. the existence of that node means that a client has successfully acquired the lock. releasing the lock is simply done by deleting node /lock. any client wanting to create the same node will receive node already exists error. zookeeper sends notification that node is deleted to clients that are waiting for lock.

###### Election algorithms :

*the bully algorithm* - 

when any process notices that the coordinator is no longer responding to requests, it initiates an election. a process $p_k$ holds the election as follows : 
- $p_k$ sends an ELECTION msg to all the processes with higher identifiers - $p_{k+1},p_{k+2},\ldots ,p_{n-1}$ 
- if no one responds, $p_k$ wins the election and becomes the coordinator.
- if one of the higher up's answers, it takes over and $p_k$'s job is done.

when a ELECTION msg arrives, the receiver sends a OK msg to indicate that it is alive and will take over. the winning process annouces to all processes by sending a COORDINATOR msg that starting immediately it is the new coordinator.  when the highest id process recovers from a crash it declares itself (bullies them) to be the new coordinator.

*ring algorithm* -

logical ring, each process knows who its successor is. when any process notices that coordinator is not responding, it builds an ELECTION msg and forwards it to its successor, if it is down then to the next of successor or the one after that, until a running process is located. at each step sender adds its own identifier to the list in the msg.
eventually, the msg gets back to the process that started it all. at this point, the msg type is changed to COORDINATOR and is circulated once again, this time to inform everyone else who is the coordinator (the list member with the highest identifier) and who the members of new ring are. when this msg is circulated once, it is removed and everyone goes back to work.

//election in raft page : 289
###### Fault Tolerance

*fault torerant system* - whenever a failure occurs, the system should continue to operate in an acceptable way while repairs are being made.

dependable systems : 

- Availability
- Reliability
- Safety
- Maintainability

*availability* is defined as the property that a system is ready to be used immediately.

*reliability* refers to property that a system can run **continuously** without failure.

*safety* refers to the situation that when a system temporarily fails to operate correctly, no catastrophic event happens.

*maintainability* refers to how easily a failed system can be repaired.

*transient faults* occur once and then disappear. if the operation is repeated, the fault goes away.

*intermitten fault* occurs, then vanishes of its own accord, then reappears and so on.

*permanent fault* continues to exists until the faulty component is replaced.

*byzantine failures*, arbitary responses at arbitary times

*asynchronous system* , no assumptions about process execution speeds or message delivery times are made. can't conclude that the other process has crashed, it might be slow etc.

*synchronous system*, process execution speeds and message delivery times are bounded. if a process shows no more activity when expected to do, other processes can conclude that it crashed.


redundancy can be used to mask faults.

*information redundancy*, extra bits are added to allow recovery from garbled bits. eg hamming code

*time redundancy*, an action is performed, and then, if need be, it is performed again. if a transaction aborts, it can be redone with no harm because nothing has been finalized yet.

*physical redundancy*, extra equipment or processes are added to make it possible for the system to tolerate the loss or malfunctioning of some components.

key approach to tolerating a faulty process is to organize several identical processes into a **group**.
when a message is sent to a group, all members receive it.

- flat group  -  no coordinator, decisions are made by all processes
- hierarchical group - coordinator assigns job and manage all the workers

*k-fault tolerant* , survive fault in *k* components.

*2k+1* process need to achieve *k-fault tolerance* in case of arbitrary failure processes.

assumption : *in a fault-tolerant process group, every non-faulty process executes the same commands, in the same order as every other non-faulty process*.

this means that group members need to reach **consensus** on which command to execute.

*flooding-based consensus*

every processes share there command list with everyone else. 

complete graph (everyone directly sends to everyone), all have the correct list after round 1.
arbitary connected graph(neighbor-flooding), after D rounds every node has every other node's value. D is the diameter of the graph (longest shortest path between any two nodes). these values are without failures. if failures can occur then *f+1* in first case and *D+f* in second case.
considering synchronous rounds and reliable delivery.

*raft consensus protocol* - 

group of replicated servers, each server maintains a **log** of operations. commited operations have the same position in each of the respective servers log. one of the server operates as a leader and decides on the order in which pending operations are to be commited.
raft is a primary-backup protocol. with primary acting as leader and backups as followers.

each client request is appended to leaders log in a tuple $<o,t,k>$ , where $o$ is the client operation, $t$ term under which the current leader serves, $k$ index of $o$ in the leader's log. after electing next leader term for new operations will be $t+1$ .

assume leader has log of length $n$ and operating in term $t$ . it receives a client operation $<o,t,n+1>$ . it sends its entire log to other servers, along with the current value of $c$ (index of last operation that is commited) . all servers copy this entire log and returns a acknowledgement ensuring that all the operations upto $c$ are commited. after receiving majority acknowledgements , leader then executes operation $o$ and returns result to client and increases $c = n +1$ . next time it communicates with other servers it sends this updated log and $c$ value.
so that other servers can commit the operation $o$. 
in reality it will only send the last tuple instead of entire log.

a server will never vote to a candidate whose log is less updated then hers.



$2f+1 \implies f$ , intuition - majority

two consecuTtive majorities at least have one common server. avoids split brain.
so that the new leader knows about the previous state of the old leader.

###### Chapter 4 : Communication

Due to absence of shared memory, all communication in distributed systems is based on sending and receiving low level messages.
P and Q have to agree on the meaning of the bits being sent.

***Remote Procedure Call -***

allow programs to call procedures located on other machines. when a process on machine A calls a procedure on machine B, the calling process on machine A is suspended, and execution of the called procedure takes place on B. Information can be transported from the called to the callee in the parameters and can come back in the procedure result. no message passing at all is visible to the programmer.

the idea behind RPC is to make a remote procedure call look as much as possible as a local one.
(transparency).

RPC uses call-by-value under the hood.

when a function is actually a remote procedure, a different version of that function, called a **client stub** , is offered to the calling client. it packs the parameters into a message and requests that message to be sent to the server.
when a message arrives at the server, the server's operating system passes it to a **server stub**.
it is a piece of code that transforms request coming in over the network into local procedure calls.
the server performs the local call returns result to the server stub. when server stub gets the control back it packs the result into a message and send it to the client.
the client stub unpacks the result and returns it to the client.

![[rpc.png]]

packing parameters into a message is called **parameter marshalling**.

marshalling and unmarshalling is all about the transformation to neutral formats.
so that both parties can understand what does the bytes sent over the network means.

local referenced object are passed by value and remote referenced object are passed via reference.

both sides should follow the same RPC protocol.
we need client and server to agree on the message format and representation of data structures.

***gRPC*** - 

gRPC uses protocol buffers as both its interface definition language and as its underlying message interchange format.

*protocol buffers* , mechanism for serializing structured data ( converting it to byte stream)

field numbers should be unique, lower field numbers take less space in wire format.
do not use *reserved* field numbers.
binary version of a message just uses field numbers as the key.

`sintN` uses zig-zag encoding instead of two's complement, +ve number *p* as *2p* and -ve number *n* as $2\lvert n\rvert -1$ .  `(n << 1)^(n >> 31)` 

for **go** , the compiler generates a `.pb.go` file with a type for each message type in your file.

