# Distributed Task Scheduler with Leader Election

## Project Report

---

## 1. Executive Summary

This project implements a **distributed task scheduling system** using gRPC and Go. The system features automatic leader election using the **Bully algorithm**, health monitoring via heartbeats, and graceful handling of worker and leader failures. Workers process computational tasks (prime number generation) while the leader coordinates task distribution.

---

## 2. System Architecture

### Overview
- **Leader Node (Node 3)**: Receives tasks, maintains worker registry, performs health monitoring
- **Worker Nodes (Nodes 1, 2)**: Process tasks, monitor leader health, participate in elections
- **Client**: Submits tasks to the leader, retries on failure

### Communication Flow
```
Client → Leader (submit task)
Leader → Worker (delegate task)
Worker → Leader (heartbeat/registration)
All Nodes ↔ All Nodes (election messages)
```

---

## 3. Key Components

### 3.1 Bully Algorithm (Leader Election)
- **When triggered**: Leader failure detected via heartbeat timeout
- **Process**:
  1. Node detects leader is dead (2 consecutive heartbeat failures)
  2. Node sends election message to all higher-numbered nodes
  3. If any respond, that node takes over
  4. If none respond, sender becomes new leader
  5. New leader announces itself to all peers

- **Advantage**: Highest ID node always becomes leader - deterministic and simple

### 3.2 Heartbeat Monitoring
- **Leader monitors workers**: Every 5 seconds
  - Sends heartbeat RPC to each registered worker
  - Marks worker as dead if 2 failures occur
  - Tasks only go to alive workers

- **Workers monitor leader**: Every 3 seconds
  - Workers check if leader is responding
  - After 2 failures, trigger election
  - If new leader elected, workers update state

### 3.3 Worker Registration
- Workers auto-register with leader on startup
- Leader maintains in-memory registry of worker addresses
- Workers register via `RegisterWorker` RPC

### 3.4 Task Distribution
- Leader receives prime computation requests
- Assigns to any alive worker (simple round-robin strategy)
- Returns result to client with worker ID

---

## 4. Protocols Used

### 4.1 gRPC Services
```protobuf
service Scheduler {
  rpc ListPrimes(PrimeReq) returns (PrimeRes);
}

service Worker {
  rpc ListPrimes(PrimeReq) returns (PrimeRes);
  rpc Heartbeat(HeartbeatReq) returns (HeartbeatRes);
}

service LeaderRegistry {
  rpc RegisterWorker(RegisterReq) returns (RegisterRes);
}

service Election {
  rpc StartElection(ElectionReq) returns (ElectionRes);
  rpc AnnounceLeader(LeaderReq) returns (LeaderRes);
}
```

### 4.2 Message Format
- **PrimeReq**: `int32 Num` (compute primes up to this number)
- **PrimeRes**: `repeated int32 PrimeList`, `string ProcessedBy`
- **HeartbeatReq/Res**: Simple alive check
- **ElectionReq**: `int32 SenderID` (requester's node ID)

---

## 5. Failure Scenarios Handled

### Scenario 1: Worker Failure
- **Detection**: Leader heartbeat fails 2x
- **Action**: Mark worker dead, don't assign new tasks
- **Result**: Tasks route to remaining alive workers

### Scenario 2: Leader Failure
- **Detection**: Worker heartbeat fails 2x
- **Trigger**: Worker initiates election
- **Process**: Bully algorithm selects new leader (highest ID)
- **Result**: New leader takes over, maintains worker registry, continues serving

### Scenario 3: Partial Network Partition
- Nodes unable to communicate may trigger election
- Eventually highest reachable ID becomes leader
- System continues with reduced capacity

### Scenario 4: Multiple Simultaneous Failures
- Elections coordinate via message passing
- Highest ID always wins (deterministic)
- System reaches consistent state quickly

---

## 6. Implementation Details

### 6.1 Technologies
- **Language**: Go 1.24.5
- **RPC Framework**: gRPC with Protocol Buffers
- **Containerization**: Docker & Docker Compose
- **Concurrency**: Go goroutines with mutexes

### 6.2 Code Structure
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

### 6.3 Key Functions

**Leader**:
- `ListPrimes()`: Receives task, delegates to worker
- `RegisterWorker()`: Worker registration
- `monitorWorkers()`: Health check loop (5s interval)
- `StartElection()`: Bully algorithm initiator
- `becomeLeader()`: Announce leadership to all peers

**Worker**:
- `ListPrimes()`: Compute primes (main task)
- `Heartbeat()`: Respond to leader pings
- `monitorLeader()`: Check leader health (3s interval)
- `runElection()`: Participate in Bully algorithm
- `becomeLeader()`: Transition to leader role if needed

**Client**:
- Tries connecting to any node (leader or worker)
- Retries until successful
- Submits prime computation requests every 5 seconds

---

## 7. Testing & Results

### 7.1 Normal Operation
```
Client connects to leader ✓
Client submits task ✓
Leader assigns to worker ✓
Worker computes primes ✓
Result returned to client ✓
```

### 7.2 Worker Failure Test
```
docker-compose kill worker1
→ Leader detects failure after 10 seconds
→ Tasks route to worker2
→ System continues normally ✓
```

### 7.3 Leader Failure Test
```
docker-compose kill leader
→ Worker1 detects failure after 6 seconds
→ Triggers election
→ Worker2 (node 2) becomes new leader
→ Client reconnects to worker2
→ System continues ✓
```

### 7.4 Multiple Failures
```
Kill leader + worker1 simultaneously
→ Worker2 detects both failures
→ Becomes new leader (highest ID among alive)
→ Client reconnects
→ System recovers ✓
```

---

## 8. Performance Characteristics

| Metric | Value |
|--------|-------|
| Heartbeat Interval | 3-5 seconds |
| Failure Detection Time | ~6 seconds (2 failures × 3s) |
| Election Completion | ~2 seconds |
| Task Processing | <1 second (for N=100) |
| Network Latency | <10ms (Docker bridge) |

---

## 9. Advantages & Limitations

### Advantages
✓ Automatic failover without manual intervention
✓ Simple deterministic leader election
✓ In-memory registry - fast lookups
✓ Containerized for easy deployment
✓ Observable logging for debugging
✓ Handles multiple concurrent failures

### Limitations
✗ Single task type (prime computation)
✗ No data persistence (registry lost on leader restart)
✗ In-memory state - no replication
✗ Simple round-robin scheduling (no load balancing)
✗ No authentication/encryption
✗ Fixed 3-node setup (not easily scalable)

---

## 10. Future Improvements

1. **Persistent State**: Use etcd/Consul for distributed consensus
2. **Load Balancing**: Monitor worker CPU/memory before assignment
3. **Replication**: Replicate leader state to backup nodes
4. **Multiple Task Types**: Support varied task types
5. **Dynamic Scaling**: Add/remove nodes without restart
6. **Security**: TLS encryption, authentication
7. **Monitoring**: Prometheus metrics, Grafana dashboards
8. **More Sophisticated Algorithms**: Raft consensus instead of Bully

---

## 11. Conclusion

This project demonstrates a working distributed system with automatic leader election, health monitoring, and failure recovery. The Bully algorithm provides a simple yet effective mechanism for leader election. The system successfully handles various failure scenarios and maintains availability through graceful degradation.

The implementation provides a solid foundation for understanding distributed systems concepts and could be extended with more sophisticated features like persistent state management and advanced scheduling algorithms.

---

## 12. How to Run

```bash
# Clone and build
docker-compose build --no-cache

# Run the system
docker-compose up

# Test failover
docker-compose kill worker1

# See new leader elected
# Client continues working
```

---

## References
- Bully Algorithm (Garcia-Molina, 1982)
- gRPC Documentation
- Go Concurrency Patterns
- Docker Best Practices