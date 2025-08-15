# Minimal Distributed Scheduler — 4–6 Week Roadmap

## Step 1 — Foundations (Week 1)
**Goal:** Understand core distributed scheduling concepts.  
**Learn / Read:**
- Distributed Systems textbook chapters on:
  - Processes & Communication
  - Coordination & Synchronization
  - Fault Tolerance Basics
- Read short papers/notes on:
  - Leader Election
  - Heartbeat Mechanisms  
**Implement:**
- Simple Go program to send tasks over TCP between two processes.
- Go channels + goroutines practice.

---

## Step 2 — Basic Job Scheduling (Week 2)
**Goal:** Create a simple scheduler that assigns jobs to workers.  
**Learn / Read:**
- Go `net/http` or `grpc-go` for RPC communication.
- JSON encoding/decoding in Go.
- Basic scheduling policies (Round Robin, Random).  
**Implement:**
- Coordinator process that:
  - Accepts jobs from a client.
  - Stores them in a queue.
  - Assigns jobs to connected workers.

---

## Step 3 — Worker Registration & Heartbeats (Week 3)
**Goal:** Make the system aware of available workers and detect failures.  
**Learn / Read:**
- Heartbeat protocol concepts.
- gRPC bidirectional streaming basics (optional).  
**Implement:**
- Worker node that:
  - Registers with coordinator.
  - Sends heartbeat signals at intervals.
- Coordinator that:
  - Removes workers from active list if heartbeats stop.

---

## Step 4 — Fault Tolerance & Job Reassignment (Week 4–5)
**Goal:** Ensure jobs are completed even if workers fail.  
**Learn / Read:**
- Fault tolerance strategies.
- Job state tracking methods.  
**Implement:**
- Store job status (pending, running, completed).
- If worker fails mid-job:
  - Detect via heartbeat timeout.
  - Reassign job to another worker.

---

## Step 5 — Testing & Optimization (Week 5–6)
**Goal:** Simulate realistic distributed environment.  
**Learn / Read:**
- Docker basics for multi-node simulation.
- Testing frameworks in Go (`testing` package).  
**Implement:**
- Run coordinator + multiple workers in Docker containers.
- Simulate:
  - Random worker crashes.
  - High job load.
- Optimize:
  - Scheduling policy.
  - Heartbeat frequency.
  - Failure detection timeout.

---

## Extra Credit (Optional)
- Add a basic web dashboard showing:
  - Worker status.
  - Job queue length.
  - Completed jobs.
- Explore using etcd or Redis for distributed state management.
