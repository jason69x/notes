
Name - Abhishek Meena
Roll no - 254101002
Course - M.tech CSE (1st yr)
Email - abhishekm1002@iitg.ac.in

# Distributed Task Scheduler with Leader Election


This project implements a **distributed task scheduling system** using gRPC and Go. The system features automatic leader election using the **Bully algorithm**, health monitoring via heartbeats, and handling of worker and leader failures. Workers process computational tasks (prime number generation in this case) while the leader coordinates task distribution.

- **Leader Node (Node 3)**: Receives tasks, maintains worker registry, performs health monitoring
- **Worker Nodes (Nodes 1, 2)**: Process tasks, monitor leader health, participate in elections
- **Client**: Submits tasks to the leader, retries on failure

###### Communication Flow -

```
Client → Leader (submit task)
Leader → Worker (delegate task)
Worker → Leader (heartbeat/registration)
All Nodes ↔ All Nodes (election messages)
```

##### Key Components -

###### Bully Algorithm (Leader Election)

- **When triggered**: Leader failure detected via heartbeat timeout
- **Process**:
  1. Node detects leader is dead (2 consecutive heartbeat failures)
  2. Node sends election message to all higher-numbered nodes
  3. If any respond, that node takes over
  4. If none respond, sender becomes new leader
  5. New leader announces itself to all peers

- **Advantage**: Highest ID node always becomes leader - deterministic and simple
###### Heartbeat Monitoring

- **Leader monitors workers**: Every 5 seconds
  - Sends heartbeat RPC to each registered worker
  - Marks worker as dead if 2 failures occur
  - Tasks only go to alive workers

- **Workers monitor leader**: Every 3 seconds
  - Workers check if leader is responding
  - After 2 failures, trigger election
  - If new leader elected, workers update state

###### Worker Registration

- Workers auto-register with leader on startup
- Leader maintains registry of worker addresses
- Workers register via `RegisterWorker` RPC

###### Task Distribution

- Leader receives prime computation requests
- Assigns to any alive worker (simple round-robin strategy)
- Returns result to client with worker ID

##### Failure Scenarios Handled -

###### Scenario 1: Worker Failure

- **Detection**: Leader heartbeat fails 2x
- **Action**: Mark worker dead, don't assign new tasks
- **Result**: Tasks route to remaining alive workers

###### Scenario 2: Leader Failure

- **Detection**: Worker heartbeat fails 2x
- **Trigger**: Worker initiates election
- **Process**: Bully algorithm selects new leader (highest ID)
- **Result**: New leader takes over, maintains worker registry, continues serving

###### Scenario 3: Partial Network Partition

- Nodes unable to communicate may trigger election
- Eventually highest reachable ID becomes leader
- System continues with reduced capacity

###### Scenario 4: Multiple Simultaneous Failures

- Elections coordinate via message passing
- Highest ID always wins (deterministic)
- System reaches consistent state quickly

##### Implementation Details

###### 1. Technologies

- **Language**: Go
- **RPC Framework**: gRPC with Protocol Buffers
- **Containerization**: Docker & Docker Compose
- **Concurrency**: Go goroutines with mutexes

###### 2. Code Structure

```
dist_scheduler/
├── cmd/
│   ├── leader/main.go      (Leader node implementation)
│   ├── worker/main.go      (Worker node implementation)
│   └── client/main.go      (Client implementation)
├── proto/
│   ├── scheduler.proto     (Protocol definitions)
│   ├── scheduler.pb.go     (Generated code)
│   └── scheduler_grpc.pb.go
├── docker-compose.yml      (3 nodes + client)
├── Dockerfile.leader
├── Dockerfile.worker
├── Dockerfile.client
├── go.mod & go.sum
```

###### 3. Key Functions

**Leader**:

- `ListPrimes()`: Receives task, delegates to worker
- `RegisterWorker()`: Worker registration
- `monitorWorkers()`: Health check loop (5s interval)
- `StartElection()`: Bully algorithm initiator
- `becomeLeader()`: Announce leadership to all peers

**Worker** :

- `ListPrimes()`: Compute primes (main task)
- `Heartbeat()`: Respond to leader pings
- `monitorLeader()`: Check leader health (3s interval)
- `runElection()`: Participate in Bully algorithm
- `becomeLeader()`: Transition to leader role if needed

**Client** :

- Tries connecting to any node (leader or worker)
- Retries until successful
- Submits prime computation requests every 5 seconds

##### Testing & Results

###### 1. Normal Operation -

```
Client connects to leader 
Client submits task 
Leader assigns to worker 
Worker computes primes 
Result returned to client 
```

###### 2. Worker Failure Test -

```
docker-compose kill worker1
→ Leader detects failure 
→ Tasks route to worker2
→ System continues normally
```

###### 3. Leader Failure Test -

```
docker-compose kill leader
→ Worker1 detects failure
→ Triggers election
→ Worker2 (node 2) becomes new leader
→ Client reconnects to worker2
→ System continues
```

###### 4. Multiple Failures -

```
Kill leader + worker1 simultaneously
→ Worker2 detects both failures
→ Becomes new leader (highest ID among alive)
→ Client reconnects
→ System recovers
```

##### Advantages & Limitations -

###### Advantages

✓ Automatic failure detection
✓ Simple deterministic leader election
✓ Containerized for easy deployment
✓ Observable logging for debugging
✓ Handles multiple concurrent failures

###### Limitations

✗ Single task type
✗ No data persistence
✗ no load balancing
✗ No authentication/encryption
✗ Fixed 3-node setup

##### Conclusion -

This project demonstrates a working distributed system with automatic leader election, health monitoring, and failure recovery. The Bully algorithm provides a simple mechanism for leader election. The system successfully handles various failure scenarios and maintains availability.

###### How to Run -

**github** - https://github.com/jason69x/distributed_task_scheduler


```bash

// install git and docker desktop
// run docker desktop

// clone github repository
git clone https://github.com/jason69x/distributed_task_scheduler.git

// change current directory to /distributed_task_scheduler
cd distributed_task_scheduler

// build containers
docker-compose build --no-cache

// Run the containers
docker-compose up

// Test
docker-compose kill worker1

// See new leader elected (check logs)
// Client continues working
```

